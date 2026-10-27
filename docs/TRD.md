# TaskFlow — Technical Requirements Document (TRD)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Principal Full-Stack Architect  
**Date:** October 2026  

---

![Technical Requirements Infographic](infographics/TaskFlow%20Technical%20Requirements%20Infographic.png)


## 1. System Architecture Overview

TaskFlow follows a decoupled client-server architecture:
- **Client Tier:** Static single-page application built with standards-compliant HTML5, CSS3, and vanilla JavaScript (ES modules, zero heavyweight framework runtime overhead).
- **Application Tier:** Node.js (v20+ LTS) running Express.js for REST API endpoints and Socket.IO for duplex real-time event broadcasting.
- **Data Tier:** MongoDB Atlas (Cloud M0 Free Tier) accessed via Mongoose ODM with strict schemas, composite indexes, and validation rules.
- **Session Layer:** Stateless JWT tokens delivered via `HttpOnly`, `SameSite` cookies with verification middleware.

```
┌─────────────────────────────────────────────────────────────┐
│                 Client (HTML5 / CSS3 / ES Modules)          │
└──────────────┬───────────────────────────────▲──────────────┘
               │ HTTPS (REST API)              │ WSS (Socket.IO)
               ▼                               │
┌──────────────────────────────────────────────┴──────────────┐
│                    Express.js Backend                       │
│  ┌───────────────────────┐       ┌───────────────────────┐  │
│  │ Security Middleware   │       │ Socket.IO Hub         │  │
│  │ (Helmet, CORS, Rate)  │       │ (Room-based isolation)│  │
│  └───────────┬───────────┘       └───────────▲───────────┘  │
│              ▼                               │              │
│  ┌───────────────────────┐                   │              │
│  │ Route Controllers     ├───────────────────┘              │
│  │ & RBAC Middleware     │                                  │
│  └───────────┬───────────┘                                  │
└──────────────┼──────────────────────────────────────────────┘
               ▼ Mongoose ODM
┌─────────────────────────────────────────────────────────────┐
│                 MongoDB Atlas Cluster                       │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Technology Stack & Decision Rationale

| Layer | Selected Tech | Decision Rationale & Trade-Offs |
| :--- | :--- | :--- |
| **Frontend** | Vanilla JS (ES6+), Modern CSS3, HTML5 | Zero build tooling dependency for Phase 1; blazing fast rendering; complete control over DOM and WebSocket listeners without Virtual DOM reconciliation overhead. |
| **Backend** | Node.js (v20+), Express.js (v4.19+) | Robust ecosystem, lightweight footprint, seamless native integration with Socket.IO in a shared HTTP server instance. |
| **Database** | MongoDB Atlas (Mongoose v8+) | Document-oriented model mirrors hierarchical Kanban structures (projects -> tasks -> comments); flexible schema evolution for tags and activity metadata. |
| **Real-time** | Socket.IO (v4.7+) | Robust fallback mechanisms (WebSocket -> Polling), built-in room clustering (`project:<id>` and `user:<id>`), and connection heartbeat handling. |
| **Authentication** | JWT (`jsonwebtoken`), `bcryptjs` | Stateless authentication avoids server-side session memory locks; `HttpOnly` cookies mitigate XSS token exfiltration. |
| **Testing** | Supertest, Playwright, Jest/Node Test Runner | Supertest tests HTTP/API contracts and RBAC gates; Playwright provides end-to-end multi-browser validation. |

---

## 3. Security Architecture & Threat Model

### 3.1 Authentication & Token Lifecycle
1. **Password Hashing:** `bcryptjs` using 12 salt rounds with asynchronous computation to prevent event-loop starvation.
2. **JWT Specification:**
   - Payload: `{ userId: ObjectId, email: String, iat: Number, exp: Number }`.
   - Lifespan: 7 days.
   - Storage: Cookie named `taskflow_token`.
   - Flags: `HttpOnly = true`, `Secure = (process.env.NODE_ENV === 'production')`, `SameSite = 'Lax'`, `Path = '/'`.
3. **Revocation & Logout:** Logout clears cookie header with `Max-Age=0` and past expiration date.

### 3.2 Authorization & RBAC Enforcement Pattern
- **Never trust client-supplied credentials:** `req.user` is populated exclusively by `authenticate` middleware after cryptographically decoding the cookie token.
- **Middleware Guard (`requireProjectRole(['owner', 'admin'])`):**
  1. Resolves `projectId` from `req.params.projectId` or query/body.
  2. Queries `ProjectMember.findOne({ project: projectId, user: req.user._id })`.
  3. Rejects with `403 Forbidden` if role is insufficient or user is not a member.

### 3.3 WebSocket Connection Authentication & Subscription Isolation
```javascript
// Socket authentication handshake
io.use((socket, next) => {
  const cookieHeader = socket.handshake.headers.cookie;
  const token = parseCookie(cookieHeader)['taskflow_token'];
  if (!token) return next(new Error('Authentication error: Missing token'));
  
  jwt.verify(token, process.env.JWT_SECRET, (err, decoded) => {
    if (err) return next(new Error('Authentication error: Invalid token'));
    socket.user = decoded; // Bind authenticated user to socket
    next();
  });
});
```
- **Room Subscription Authorization:**
  Before allowing `socket.join('project:' + projectId)`, the server queries the database to confirm the user holds active membership in `projectId`. Unverified subscriptions are blocked and flagged.

### 3.4 API Hardening & Attack Prevention
- **Helmet.js:** Configures secure HTTP headers (CSP, HSTS, X-Content-Type-Options, Frameguard).
- **CORS:** Strict whitelist configuration allowing only the designated frontend domain. Credentials enabled (`credentials: true`).
- **Rate Limiting:** `express-rate-limit` configured:
  - Auth routes (`/api/auth/*`): 10 requests per 15 minutes per IP.
  - General API routes: 200 requests per 15 minutes per IP.
- **Input Sanitization:** Validates request schemas with validation middleware and strips dangerous characters/NoSQL injection queries (`$where`, `$gt`).

---

## 4. REST API Specification

### 4.1 Authentication Endpoints (`/api/auth`)
- `POST /api/auth/register` — Create user account (`name`, `email`, `password`). Returns user profile + sets cookie.
- `POST /api/auth/login` — Authenticate user (`email`, `password`). Sets cookie.
- `POST /api/auth/logout` — Invalidate session cookie.
- `GET /api/auth/me` — Return current authenticated user profile.

### 4.2 Projects Endpoints (`/api/projects`)
- `GET /api/projects` — Fetch all projects user is a member of.
- `POST /api/projects` — Create project (`name`, `description`). Creator assigned `owner`.
- `GET /api/projects/:projectId` — Fetch project detail + member list.
- `PUT /api/projects/:projectId` — Update project metadata (requires `owner` or `admin`).
- `DELETE /api/projects/:projectId` — Delete project & cascade tasks (requires `owner`).
- `POST /api/projects/:projectId/members` — Invite/add member (`email`, `role`).
- `PUT /api/projects/:projectId/members/:memberId` — Update role (requires `owner` or `admin`).
- `DELETE /api/projects/:projectId/members/:memberId` — Remove member from project.

### 4.3 Tasks Endpoints (`/api/projects/:projectId/tasks`)
- `GET /api/projects/:projectId/tasks` — Fetch all tasks for board.
- `POST /api/projects/:projectId/tasks` — Create task (`title`, `description`, `status`, `priority`, `dueDate`, `assigneeId`).
- `GET /api/projects/:projectId/tasks/:taskId` — Fetch task details with comments.
- `PUT /api/projects/:projectId/tasks/:taskId` — Update task details or reposition order.
- `PATCH /api/projects/:projectId/tasks/:taskId/status` — Move task status & update order index.
- `DELETE /api/projects/:projectId/tasks/:taskId` — Remove task (requires `owner`, `admin`, or task creator).

### 4.4 Comments & Activity Endpoints
- `GET /api/tasks/:taskId/comments` — Fetch task comments.
- `POST /api/tasks/:taskId/comments` — Post comment (`content`).
- `GET /api/projects/:projectId/activity` — Fetch paginated activity audit feed.
- `GET /api/notifications` — Fetch user's in-app notifications.
- `PATCH /api/notifications/:id/read` — Mark notification read.

---

## 5. WebSocket Event Dictionary

| Event Name | Direction | Payload Structure | Scope / Target Room |
| :--- | :--- | :--- | :--- |
| `project:join` | Client -> Server | `{ projectId: String }` | Evaluates membership before joining `project:<id>` |
| `project:leave` | Client -> Server | `{ projectId: String }` | Socket leaves `project:<id>` |
| `task:created` | Server -> Client | `{ task: Object, actor: Object }` | `project:<projectId>` |
| `task:updated` | Server -> Client | `{ taskId: String, updates: Object }` | `project:<projectId>` |
| `task:moved` | Server -> Client | `{ taskId: String, fromStatus: String, toStatus: String, newIndex: Number }` | `project:<projectId>` |
| `task:deleted` | Server -> Client | `{ taskId: String }` | `project:<projectId>` |
| `comment:added` | Server -> Client | `{ taskId: String, comment: Object }` | `project:<projectId>` |
| `notification:new` | Server -> Client | `{ notification: Object }` | `user:<recipientUserId>` |
| `user:typing` | Client <-> Server| `{ taskId: String, userName: String }` | `project:<projectId>` |

---

## 6. Free-Tier Cloud Deployment Constraints & Mitigations

| Provider / Service | Free-Tier Limits | Architectural Risks | Engineering Mitigation |
| :--- | :--- | :--- | :--- |
| **Render / Railway (Backend)** | 512 MB RAM, sleeps after 15 min inactivity | Cold-start latency (30-50s) on initial HTTP/WS request. | Graceful client retry and visual connection spinner ("Waking up server..."); ping health check on initial page load. |
| **MongoDB Atlas (M0 Free)** | 512 MB Storage, 100 max concurrent connections | Connection pool exhaustion during spikes; storage limit. | Configure Mongoose connection pool (`maxPoolSize: 10`); auto-archive activity logs older than 90 days; lean document projections. |
| **Vercel / Netlify (Frontend)** | 100 GB bandwidth, unlimited static assets | None for static HTML/JS delivery. | Fast global CDN caching for CSS, JS, and SVG assets. |
