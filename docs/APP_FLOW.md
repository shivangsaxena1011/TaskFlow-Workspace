# TaskFlow — Application Flow & User Journeys (APP_FLOW.md)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Product & UX Engineering  
**Date:** October 2026  

---

![Application Flow Infographic](infographics/TaskFlow%20Application%20Flow%20Infographic.png)


## 1. End-to-End System Navigation Flow

```
                      ┌──────────────────────┐
                      │     Landing Page     │
                      └──────────┬───────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
       ┌──────────────────┐            ┌──────────────────┐
       │  Register Flow   │            │    Login Flow    │
       └─────────┬────────┘            └────────┬─────────┘
                 │                              │
                 └───────────────┬──────────────┘
                                 ▼
                    ┌─────────────────────────┐
                    │  Authenticated Session  │
                    │  (HttpOnly JWT Cookie)  │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     Main Dashboard      │
                    │   - Projects Grid       │
                    │   - Quick Stats & Tasks │
                    │   - Notifications Bell  │
                    └────────────┬────────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 ▼                               ▼
       ┌──────────────────┐            ┌──────────────────┐
       │  Create Project  │            │  Select Project  │
       │     (Modal)      │            │     Workspace    │
       └──────────────────┘            └────────┬─────────┘
                                                │
       ┌────────────────────────────────────────┴────────────────────────────────────────┐
       ▼                                        ▼                                        ▼
┌──────────────┐                         ┌──────────────┐                         ┌──────────────┐
│ Kanban Board │                         │ Members Tab  │                         │ Activity Log │
│ (Real-time)  │                         │ & Roles RBAC │                         │  Audit Feed  │
└──────┬───────┘                         └──────────────┘                         └──────────────┘
       │
       ├─────────────────────────────────┐
       ▼                                 ▼
┌──────────────┐                  ┌──────────────┐
│ Create Task  │                  │  Task Detail │
│   (Modal)    │                  │  Side-Drawer │
└──────────────┘                  │  & Comments  │
                                  └──────────────┘
```

---

## 2. Core User Journey Specifications

### Journey 1: New User Registration & First Workspace
1. **Discovery:** User visits root URL `/`.
2. **Input:** Selects "Get Started", enters name, email, and strong password.
3. **Execution:**
   - Client sends `POST /api/auth/register`.
   - Server validates payload, hashes password with `bcryptjs`, writes `User` record, generates signed JWT, and returns `Set-Cookie: taskflow_token=...`.
4. **Onboarding Action:** User lands on empty dashboard with welcome banner and primary CTA: **"Create Your First Project"**.
5. **Project Instantiation:** User submits project title and description. System automatically creates `Project` and links `ProjectMember` with `role: "owner"`.

### Journey 2: Kanban Task Lifecycle & Collaborative State Updates
1. **Board Initialization:**
   - User navigates to `/project/:id`.
   - Client establishes WebSocket connection (`io()`), emitting `project:join` with `{ projectId }`.
   - Client concurrently fetches initial board state via `GET /api/projects/:id/tasks`.
2. **Task Creation:**
   - User clicks `+ Add Task` in "To Do" column.
   - Submits modal form (Title, Priority: High, Assignee: Teammate, Due Date: 2026-11-05).
   - Server writes to MongoDB, creates notification for Assignee, and emits `task:created` to room `project:<projectId>`.
   - All open screens in that project dynamically append the card without full page refresh.
3. **Task Progression (Drag and Drop):**
   - User drags card from "To Do" to "In Progress".
   - Client optimistically updates UI state and emits `PATCH /api/projects/:projectId/tasks/:taskId/status` with payload `{ status: 'in-progress', position: 2 }`.
   - Server validates user's project role (`owner`, `admin`, or `member`), writes to database, logs an `ActivityLog` item, and broadcasts `task:moved` to the project room.

### Journey 3: In-Task Discussions & User Mentions
1. **Modal Opening:** User clicks a task card. A detailed side-drawer slides into view showing full markdown description, metadata badges, and comment thread.
2. **Comment Submission:**
   - User types `"@shivang please check the latest schema updates"`.
   - Submits comment via `POST /api/tasks/:taskId/comments`.
   - Backend detects `@mention`, finds target user ID, inserts comment into database, and emits:
     - `comment:added` to `project:<projectId>`.
     - `notification:new` to `user:<targetUserId>`.
3. **Notification Consumption:**
   - Mentioned user sees badge counter on notification bell increment immediately.
   - Clicking bell opens drawer showing: *"Shivang mentioned you in Design Homepage"*.
   - Clicking notification focuses the exact task modal directly.

### Journey 4: Member Invitation & Role Authorization
1. **Invite Action:** Project Owner or Admin navigates to "Members" tab and clicks `Invite Member`.
2. **Submission:** Enters registered user email and selects role (`admin`, `member`, `viewer`).
3. **Security Gate:** Backend checks `req.user` role in `ProjectMember`. If `viewer` or `member` attempts this, request returns `403 Forbidden`.
4. **Propagation:** System creates `ProjectMember` entry and alerts invited user via in-app notification.

---

## 3. UI State Transitions & Edge Case Handling

| Trigger / Condition | Expected UI Reaction | Fallback / Recovery Strategy |
| :--- | :--- | :--- |
| **Token Expiry (401 response)** | Redirect to `/login` with prompt toast: *"Session expired. Please log in again."* | Preserve route state in `sessionStorage` (`redirectAfterLogin`) to return user to exact board. |
| **Unauthorized Action (403)** | Action rejected; toast error displayed: *"You do not have permission to perform this action."* | Roll back optimistic UI changes to previous verified state. |
| **WebSocket Disconnection** | Top status bar displays amber pill: *"Connecting to real-time sync..."* | Socket.IO auto-reconnects with exponential backoff. Upon reconnection, client refetches task delta to avoid desync. |
| **Empty Project Board** | Display friendly illustrated empty state cards with guided CTA. | Provide sample template tasks if requested. |
