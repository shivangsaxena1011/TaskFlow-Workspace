# TaskFlow — Product Requirements Document (PRD)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Architecture & Product Strategy Team  
**Date:** October 2026  

---

![Product Requirements Infographic](infographics/TaskFlow%20Product%20Requirements%20Infographic.png)


## 1. Executive Summary & Vision
**TaskFlow** is a premium, lightweight, real-time collaborative project management web application designed for high-velocity teams, developers, and organizations. Blending the intuitive fluidity of modern Kanban boards with real-time multi-user synchronization, TaskFlow streamlines task orchestration, context-rich discussions, and activity auditing without bloated enterprise overhead.

---

## 2. Target Users & Personas

| Persona | Role | Key Goals & Pain Points |
| :--- | :--- | :--- |
| **Dev Lead / Project Owner** | Account Owner | Needs to create projects, invite members with strict role privileges, track milestone velocity, and ensure secure auditability. |
| **Full-Stack Contributor** | Member / Admin | Requires instant task state updates, low friction drag-and-drop Kanban workflow, markdown task descriptions, and task-specific discussions without page reloads. |
| **Cross-Functional Reviewer** | Viewer / Member | Tracks task progress, receives real-time desktop/in-app notifications when mentioned or assigned, and leaves actionable comments. |

---

## 3. Product Goals & Success Metrics

### 3.1 Core Objectives
1. **Zero-Latency Feel:** Instant UI updates via WebSockets (Socket.IO) for all board changes, assignments, and discussions.
2. **Enterprise-Grade Security on Free-Tier:** Strict cookie-based HttpOnly JWT sessions, database-level role authorization, sanitization, and rate-limiting.
3. **Frictionless Onboarding:** Intuitive onboarding from registration to first project creation in under 60 seconds.

### 3.2 Success Metrics (KPIs)
- **Active User Retention:** Weekly active project engagement > 60%.
- **Task Interaction Velocity:** Average task state update broadcast latency < 150ms.
- **Error Budget:** Zero authorization leakage incidents (100% authorization gate coverage).

---

## 4. Feature Scope Matrix (MVP vs. Future Roadmap)

### 4.1 In Scope (Phase 1 & MVP Implementation)
- **Authentication & Identity:**
  - Secure registration, login, logout with bcrypt password hashing (cost factor 12).
  - HttpOnly, SameSite strict session JWT cookies.
  - Avatar management and profile info.
- **Workspace & Project Management:**
  - Create, edit, archive, and delete projects.
  - Granular project membership roles: `Owner`, `Admin`, `Member`, `Viewer`.
  - Project invitation handling by email or secure link token.
- **Kanban Task Engine:**
  - Standard & custom workflow columns (`To Do`, `In Progress`, `Review`, `Done`).
  - Task creation, drag-and-drop status ordering, priority badges (`Low`, `Medium`, `High`, `Urgent`), due date tracking, assignee assignments.
  - Detailed task modal with rich markdown/text description and tag pills.
- **Comments & Mentions:**
  - Task-scoped real-time discussion thread.
  - User mention tag matching (`@username`).
- **Real-Time Collaboration & Notifications:**
  - Live board synchronization (no manual refresh needed).
  - In-app notification bell with unread indicators for assignment, mentions, and project invitations.
- **Audit & Activity Logs:**
  - Project-level audit log recording all critical lifecycle events.

### 4.2 Out of Scope (Future Phases)
- Cloud file attachments exceeding 5MB (Cloudinary/S3 integration reserved for Phase 2).
- Native mobile applications (iOS/Android wrappers; web will remain fully mobile responsive).
- Third-party OAuth providers (Google/GitHub single-sign-on deferred to Phase 2).
- Paid billing tiers and Stripe subscription management.

---

## 5. User Roles and RBAC Matrix

| Permission / Action | Owner | Admin | Member | Viewer |
| :--- | :---: | :---: | :---: | :---: |
| Delete Project | ✅ | ❌ | ❌ | ❌ |
| Transfer Project Ownership | ✅ | ❌ | ❌ | ❌ |
| Manage Project Settings | ✅ | ✅ | ❌ | ❌ |
| Invite / Remove Members | ✅ | ✅ | ❌ | ❌ |
| Change Member Roles | ✅ | ✅ (Non-owner) | ❌ | ❌ |
| Create / Edit / Move Tasks | ✅ | ✅ | ✅ | ❌ |
| Assign / Unassign Members | ✅ | ✅ | ✅ | ❌ |
| Comment on Tasks | ✅ | ✅ | ✅ | ❌ (Read-only) |
| View Boards, Tasks, Activity | ✅ | ✅ | ✅ | ✅ |

---

## 6. Acceptance Criteria (By Feature)

### AC-01: Authentication & Session Management
- Passwords must be at least 8 characters containing upper, lower, and digit characters.
- Successful login must issue a signed JWT inside an `HttpOnly`, `Secure` (in production), `SameSite=Lax` cookie.
- Unauthenticated requests to protected endpoints must return HTTP `401 Unauthorized`.

### AC-02: Project Authorization Isolation
- Users cannot read or manipulate projects or project resources (tasks, comments, logs) unless an explicit `ProjectMember` record exists.
- Attempting to access an unauthorized project must return HTTP `403 Forbidden` (or `404 Not Found` to prevent project enumeration).

### AC-03: Real-Time Kanban Synchronization
- When User A moves a task from "To Do" to "In Progress", all clients connected to room `project:<projectId>` must receive a `task:moved` event and reposition the card within 200ms without DOM re-render flicker.

### AC-04: Notifications & Activity Logging
- Every task assignment must create a persistent `Notification` document for the assignee and broadcast `notification:new` to that specific user's socket room (`user:<userId>`).
