# TaskFlow — Quality Assurance & Testing Plan (TEST_PLAN.md)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** QA Engineering & Reliability Team  
**Date:** October 2026  

---

![QA and Testing Strategy Board](infographics/TaskFlow%20Project%20Documentation%20Board.png)


## 1. Testing Strategy Matrix

To guarantee stability, data integrity, and strict access isolation, TaskFlow adopts a multi-tiered testing strategy:

```
          ┌────────────────────────────────────────┐
          │      End-to-End Tests (Playwright)     │  Multi-client real-time sync,
          │             ~15 Scenarios              │  browser interactions & auth flow
          ├────────────────────────────────────────┤
          │     Integration Tests (Supertest)      │  API routes, JWT cookies,
          │             ~50 Test Cases             │  RBAC authorization gates
          ├────────────────────────────────────────┤
          │         Unit Tests (Node / Jest)       │  Mongoose schema validation,
          │             ~70 Test Cases             │  hash generation & token logic
          └────────────────────────────────────────┘
```

---

## 2. Test Cases & Acceptance Matrix

### 2.1 Authentication & Session Security (Supertest)

| Test ID | Test Scenario | Input Data | Expected Status / Result |
| :--- | :--- | :--- | :--- |
| **AUTH-01** | Register with valid credentials | `{ name: "Alice", email: "alice@test.com", password: "Password123!" }` | `201 Created`, user profile returned, `Set-Cookie` contains `taskflow_token`. |
| **AUTH-02** | Register with duplicate email | Duplicate `alice@test.com` | `400 Bad Request` or `409 Conflict`, error: "Email already registered". |
| **AUTH-03** | Register with weak password (<8 chars) | `{ password: "pass" }` | `422 Unprocessable Entity`, validation error returned. |
| **AUTH-04** | Login with correct credentials | Valid email & password | `200 OK`, `Set-Cookie` header present with `HttpOnly` flag. |
| **AUTH-05** | Login with incorrect password | Valid email, wrong password | `401 Unauthorized`, error: "Invalid credentials". |
| **AUTH-06** | Access protected route without cookie | `GET /api/projects` without cookie | `401 Unauthorized`, error: "Authentication token missing". |
| **AUTH-07** | Logout clears cookie | `POST /api/auth/logout` | `200 OK`, `Set-Cookie` contains expired/cleared token. |

### 2.2 RBAC & Multi-Tenant Authorization Gates (Supertest)

| Test ID | Test Scenario | Actor & Context | Expected Status / Result |
| :--- | :--- | :--- | :--- |
| **RBAC-01** | Non-member accesses private project | User B requests Project A | `403 Forbidden` or `404 Not Found`. Data never leaked. |
| **RBAC-02** | Member creates task in project | User with `member` role | `201 Created`, task saved with `createdBy: User._id`. |
| **RBAC-03** | Viewer attempts to create task | User with `viewer` role | `403 Forbidden`, error: "Write permission denied". |
| **RBAC-04** | Member attempts to delete project | User with `member` role | `403 Forbidden`, only `owner` can delete project. |
| **RBAC-05** | Admin invites new member | User with `admin` role | `201 Created`, `ProjectMember` created. |
| **RBAC-06** | Member attempts to invite new member | User with `member` role | `403 Forbidden`, only `owner` or `admin` can invite. |

### 2.3 Kanban & Real-Time Sync (Playwright E2E)

| Test ID | Test Scenario | Execution Procedure | Expected Behavior |
| :--- | :--- | :--- | :--- |
| **E2E-01** | Multi-Browser Task Move Sync | Open Browser 1 (User A) and Browser 2 (User B) on same project board. User A drags Task from "To Do" to "In Progress". | Browser 2 moves card into "In Progress" column within 250ms without page reload. |
| **E2E-02** | Live Comment & Mention Alert | User A opens Task Modal and posts `@userB please review`. | Browser 2 increments unread notification count on bell icon in real time. |
| **E2E-03** | Task Filter & Search | User types query in board search bar. | Board instantaneously hides non-matching cards, keeping column count indicators consistent. |

---

## 3. Socket.IO Test Plan & Edge Scenarios

1. **Unauthenticated Connection:** Attempting socket handshake without cookie must disconnect with `Authentication error: Missing token`.
2. **Unauthorized Project Room Join:** Client emits `project:join` for a project where the user is not a member. Server must reject subscription and NOT join socket to room.
3. **Reconnection Grace:** Severing network connection displays amber connection bar; restoring connection emits auto-reconnect and queries missing task updates.

---

## 4. Test Automation Pipeline (CI/CD)

```yaml
# Pipeline Execution Flow
1. Lint & Format Check: ESLint + Prettier
2. Unit Tests: Schema checks & helper validation
3. API Integration Tests: In-memory MongoDB (mongodb-memory-server) + Supertest
4. E2E Tests: Playwright headless Chromium against local build
5. Security Audit: npm audit --audit-level=high
```
