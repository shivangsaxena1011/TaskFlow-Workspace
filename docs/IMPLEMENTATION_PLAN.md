# TaskFlow — Full-Stack Implementation Roadmap (IMPLEMENTATION_PLAN.md)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Technical Program Manager & Lead Architect  
**Timeline:** 7 Strategic Milestones  

---

![Full-Stack Implementation Roadmap](infographics/TaskFlow%20Full-Stack%20Implementation%20Roadmap.png)


## 1. Proposed Project Folder Structure

```
taskflow/
├── docs/
│   ├── PRD.md
│   ├── TRD.md
│   ├── APP_FLOW.md
│   ├── DATABASE_SCHEMA.md
│   ├── UI_UX_BRIEF.md
│   ├── IMPLEMENTATION_PLAN.md
│   └── TEST_PLAN.md
├── client/
│   ├── index.html                  # Main SPA entry
│   ├── css/
│   │   ├── variables.css           # Design tokens & color palette
│   │   ├── base.css                # Reset & typography
│   │   ├── components.css          # Buttons, modals, forms, badges
│   │   └── board.css               # Kanban board & task card styling
│   ├── js/
│   │   ├── app.js                  # Main client application bootstrap
│   │   ├── api.js                  # Fetch wrapper with cookie credentials
│   │   ├── socket.js               # Socket.IO client singleton & event dispatcher
│   │   ├── state.js                # Reactive client state store
│   │   ├── views/
│   │   │   ├── authView.js         # Login & registration rendering
│   │   │   ├── dashboardView.js    # Projects overview & metrics
│   │   │   ├── boardView.js        # Drag-and-drop Kanban engine
│   │   │   └── taskModalView.js    # Task detail & comments drawer
│   │   └── utils/
│   │       ├── router.js           # Client-side hash/history router
│   │       └── notifications.js    # Toast & notification bell handler
│   └── assets/
│       ├── icons/                  # Clean SVG icons
│       └── logo.svg                # TaskFlow official vector logo
├── server/
│   ├── package.json
│   ├── server.js                   # Express + HTTP + Socket.IO bootstrap
│   ├── config/
│   │   ├── db.js                   # Mongoose connection & pool settings
│   │   └── socket.js               # Socket.IO initialization & room middleware
│   ├── middleware/
│   │   ├── auth.js                 # JWT cookie verification
│   │   ├── rbac.js                 # Project role permissions enforcement
│   │   ├── rateLimiter.js          # Route-level rate limiting
│   │   └── errorHandler.js         # Global JSON error formatting
│   ├── models/
│   │   ├── User.js
│   │   ├── Project.js
│   │   ├── ProjectMember.js
│   │   ├── Task.js
│   │   ├── Comment.js
│   │   ├── Notification.js
│   │   └── ActivityLog.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── projectRoutes.js
│   │   ├── taskRoutes.js
│   │   ├── commentRoutes.js
│   │   └── notificationRoutes.js
│   └── sockets/
│       ├── projectHandler.js       # Board sync & task moving events
│       └── notificationHandler.js  # Direct-to-user alerts
├── tests/
│   ├── unit/
│   │   └── models.test.js          # Mongoose validation & constraints
│   ├── integration/
│   │   ├── auth.test.js            # Register/login cookie issuance
│   │   ├── rbac.test.js            # Project permission gates
│   │   └── tasks.test.js           # Task CRUD & re-ordering
│   └── e2e/
│       └── kanban-flow.spec.js     # Playwright multi-user board sync
├── .env.example
├── .gitignore
└── README.md
```

---

## 2. Phased Development Roadmap

### Phase 1: Environment Setup, Database & Security Foundation (Week 1)
- Initialize Node.js environment and configure Express server with Helmet, CORS, and cookie-parser.
- Connect MongoDB Atlas with resilient reconnection logic and index verification.
- Implement `User` model, password hashing with bcrypt, JWT token generation, and `authenticate` middleware.
- Build auth endpoints (`/api/auth/register`, `/api/auth/login`, `/api/auth/logout`, `/api/auth/me`).
- Deliverable: Authenticated API responding with secured HttpOnly cookies.

### Phase 2: Workspace & Granular RBAC Engine (Week 2)
- Implement `Project` and `ProjectMember` models with unique compound index `{ project: 1, user: 1 }`.
- Write `requireProjectRole` middleware enforcing owner/admin/member/viewer permissions.
- Implement project CRUD endpoints, member invitation, and role escalation/demotion routes.
- Deliverable: Verified project isolation preventing unauthorized user access.

### Phase 3: Kanban Engine & Task State Management (Week 3)
- Implement `Task` model with composite index `{ project: 1, status: 1, position: 1 }`.
- Implement task CRUD endpoints and status move route with index normalization.
- Wire frontend static board view with native HTML5 Drag and Drop API (`dragstart`, `dragover`, `drop`).
- Deliverable: Working Kanban board with persistent task updates via REST.

### Phase 4: Real-Time Duplex Synchronization via Socket.IO (Week 4)
- Attach Socket.IO to the Express HTTP instance.
- Implement socket authentication middleware verifying the `taskflow_token` cookie.
- Implement room subscriptions (`project:<id>` and `user:<id>`) guarded by project membership checks.
- Broadcast `task:created`, `task:moved`, and `task:updated` events to room subscribers.
- Deliverable: Simultaneous multi-user live synchronization across browsers without refresh.

### Phase 5: Discussions, Mentions & Notification Center (Week 5)
- Implement `Comment` and `Notification` models.
- Build task drawer modal with real-time comment stream and `@mention` parsing.
- Emit targeted `notification:new` events to assigned and mentioned users.
- Deliverable: Real-time collaborative discussions and live notification bell counters.

### Phase 6: Activity Audit Feed & Polish (Week 6)
- Implement `ActivityLog` tracking all lifecycle events (task moved, member joined, etc.).
- Responsive polish across mobile, tablet, and desktop viewports.
- Keyboard shortcuts (`Escape` to close modal, `N` for new task).
- Deliverable: Production-ready SaaS interface with complete audit trail.

### Phase 7: Automated Testing, Security Audit & Cloud Deployment (Week 7)
- Run Supertest integration suites for authentication and RBAC boundary isolation.
- Execute Playwright end-to-end multi-client tests verifying real-time sync.
- Deploy backend to Render/Railway and frontend to static CDN hosting.
- Deliverable: Fully verified, live web application with documented acceptance.

---

## 3. Environment Variables Specification (`.env.example`)

```bash
# Server Environment
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/taskflow?retryWrites=true&w=majority

# Authentication Security
JWT_SECRET=super_secret_jwt_signing_key_replace_in_production_min32chars
JWT_EXPIRES_IN=7d

# Client URL (For CORS Whitelist)
CLIENT_URL=http://localhost:3000

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=200
```
