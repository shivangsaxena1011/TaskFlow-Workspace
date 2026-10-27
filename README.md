# TaskFlow — Modern Collaborative Project Management Workspace

<div align="center">

![TaskFlow Project Documentation Board](docs/infographics/TaskFlow%20Project%20Documentation%20Board.png)

### *Plan &bull; Build &bull; Test &bull; Deploy*

[![Architecture Status](https://img.shields.io/badge/Architecture-Phase%201%20Approved-4F46E5?style=for-the-badge&logo=blueprint)](docs/TRD.md)
[![Frontend](https://img.shields.io/badge/Frontend-Vanilla%20ES6%2B%20%7C%20CSS3%20%7C%20HTML5-06B6D4?style=for-the-badge&logo=javascript)](prototypes/)
[![Backend](https://img.shields.io/badge/Backend-Node.js%20v20%2B%20%7C%20Express%20v4-10B981?style=for-the-badge&logo=nodedotjs)](docs/TRD.md)
[![Real-time](https://img.shields.io/badge/Real--Time-Socket.IO%20Duplex-8B5CF6?style=for-the-badge&logo=socketdotio)](docs/TRD.md)
[![Database](https://img.shields.io/badge/Database-MongoDB%20Atlas%20%7C%20Mongoose-47A248?style=for-the-badge&logo=mongodb)](docs/DATABASE_SCHEMA.md)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)

[**Explore Interactive Prototypes**](prototypes/) &bull; [**Architecture Specifications**](docs/) &bull; [**Implementation Roadmap**](docs/IMPLEMENTATION_PLAN.md)

</div>

---

## 📖 Executive Summary & Vision

**TaskFlow** is a modern, lightweight, real-time collaborative SaaS project management platform built for modern engineering squads, product managers, and remote-first organizations. Designed to eliminate enterprise software bloat, TaskFlow delivers an ultra-fast Kanban workflow, sub-150ms multi-user synchronization, granular role-based access control (RBAC), context-rich task discussions with `@mentions`, and in-app activity notifications.

### Key Architectural Tenets
- ⚡ **Zero-Latency Feel:** Instant UI updates powered by Socket.IO duplex event broadcasting across active project rooms.
- 🛡️ **Zero-Trust Security:** Strict `HttpOnly` JWT cookie session management, preventing XSS exfiltration and CSRF risks without client-side token storage.
- 👥 **Granular Multi-Tenant RBAC:** Database-enforced role permissions (`Owner`, `Admin`, `Member`, `Viewer`) validating every REST endpoint and WebSocket room subscription.
- ☁️ **Free-Tier Optimized:** Engineered to run seamlessly on cost-effective cloud tiers (Render / Railway, MongoDB Atlas M0, and Vercel/Netlify CDNs) with connection pooling and cold-start mitigations.

---

## 🎨 Interactive Prototypes Showcase

High-fidelity interactive mockups built with clean HTML5, modern design tokens, and vanilla ES6 scripts are available directly in the [`prototypes/`](prototypes/) workspace:

| Prototype View | Preview | Description & Features | Launch |
| :--- | :---: | :--- | :---: |
| **Interactive Kanban Board** | [View Screen](prototypes/kanban/screen.png) | Native HTML5 drag-and-drop task workflow, multi-column status lanes (`To Do`, `In Progress`, `Review`, `Done`), live search filtering, and task discussion drawer with real-time comment stream. | [**Open Board**](prototypes/kanban/index.html) |
| **Workspace Dashboard** | [View Screen](prototypes/dashboard/screen.png) | High-impact sprint metrics, active project portfolio overview, velocity gauges, team member distribution, and notifications dropdown. | [**Open Dashboard**](prototypes/dashboard/index.html) |
| **Modern SaaS Landing Page** | [View Screen](prototypes/landing/screen.png) | Conversion-optimized product homepage with feature spotlights, live interactive demo links, pricing plans, and engineering trust signals. | [**Open Landing**](prototypes/landing/index.html) |

> 💡 **Try it locally:** Open [`prototypes/index.html`](prototypes/index.html) directly in any browser for a unified prototype navigation dashboard.

---

## 📊 Comprehensive Infographics & Visual Guides

All Phase 1 architectural infographics are cataloged in high resolution within [`docs/infographics/`](docs/infographics/):

| Infographic Guide | High-Resolution Visual | Corresponding Specification |
| :--- | :--- | :--- |
| **Product Requirements & Personas** | [Product Requirements Infographic](docs/infographics/TaskFlow%20Product%20Requirements%20Infographic.png) | [Product Requirements Document (PRD)](docs/PRD.md) |
| **Technical System Architecture** | [Technical Requirements Infographic](docs/infographics/TaskFlow%20Technical%20Requirements%20Infographic.png) | [Technical Requirements Document (TRD)](docs/TRD.md) |
| **Application Flow & Navigation** | [Application Flow Infographic](docs/infographics/TaskFlow%20Application%20Flow%20Infographic.png) | [Application Flow & User Journeys](docs/APP_FLOW.md) |
| **Backend Database Schema** | [Backend Database Schema](docs/infographics/TaskFlow%20Backend%20Database%20Schema.png) | [Database Schema & Models](docs/DATABASE_SCHEMA.md) |
| **UI/UX Design Tokens & System** | [UI/UX Design Brief](docs/infographics/TaskFlow%20UI_UX%20Design%20Brief.png) | [UI/UX Design Brief](docs/UI_UX_BRIEF.md) & [DESIGN.md](docs/DESIGN.md) |
| **Full-Stack Implementation Plan** | [Full-Stack Implementation Roadmap](docs/infographics/TaskFlow%20Full-Stack%20Implementation%20Roadmap.png) | [Implementation Roadmap](docs/IMPLEMENTATION_PLAN.md) |
| **Project Documentation Board** | [Project Documentation Board](docs/infographics/TaskFlow%20Project%20Documentation%20Board.png) | [Testing & QA Strategy](docs/TEST_PLAN.md) |

---

## 📂 Phase 1 Documentation Index

Each specification document has been approved and cross-linked with its corresponding architectural visual:

1. 📋 **[Product Requirements Document (PRD)](docs/PRD.md)**  
   *Executive vision, user personas, MVP scope matrix, RBAC permission tables, and feature acceptance criteria.*
2. 🏗️ **[Technical Requirements Document (TRD)](docs/TRD.md)**  
   *Client-server architecture, security threat model, REST API endpoint dictionary, and WebSocket event matrix.*
3. 🗺️ **[Application Flow & User Journeys (APP_FLOW.md)](docs/APP_FLOW.md)**  
   *End-to-end user navigation flow, registration/login state machines, drag-and-drop lifecycles, and UI edge cases.*
4. 🗄️ **[Database Schema & Data Models (DATABASE_SCHEMA.md)](docs/DATABASE_SCHEMA.md)**  
   *Production Mongoose schemas for Users, Projects, ProjectMembers, Tasks, Comments, Notifications, and ActivityLogs with composite indexes.*
5. 🎨 **[UI/UX Design Brief & Tokens (UI_UX_BRIEF.md)](docs/UI_UX_BRIEF.md)** & **[Theme Tokens (DESIGN.md)](docs/DESIGN.md)**  
   *Color system, typography scales, elevation shadows, component states, and responsive layout guidelines.*
6. 🚀 **[Full-Stack Implementation Roadmap (IMPLEMENTATION_PLAN.md)](docs/IMPLEMENTATION_PLAN.md)**  
   *Proposed target repository structure, 7 strategic development phases, and environment configuration.*
7. 🧪 **[QA & Testing Plan (TEST_PLAN.md)](docs/TEST_PLAN.md)**  
   *Supertest integration suites, RBAC security boundary test cases, and Playwright multi-client E2E test plan.*

---

## 🏛️ System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                 Client (HTML5 / CSS3 / ES Modules)          │
│            - Native Drag & Drop Kanban Engine               │
│            - Responsive Navigation & Modals                 │
│            - Real-Time Live Notification Center             │
└──────────────┬───────────────────────────────▲──────────────┘
               │ HTTPS (REST API)              │ WSS (Socket.IO)
               ▼                               │
┌──────────────────────────────────────────────┴──────────────┐
│                    Express.js Backend                       │
│  ┌───────────────────────┐       ┌───────────────────────┐  │
│  │ Security Middleware   │       │ Socket.IO Hub         │  │
│  │ (Helmet, CORS, Rate)  │       │ (Room: project:<id>)  │  │
│  └───────────┬───────────┘       │ (Room: user:<id>)     │  │
│              ▼                   └───────────▲───────────┘  │
│  ┌───────────────────────┐                   │              │
│  │ Route Controllers     ├───────────────────┘              │
│  │ & RBAC Middleware     │ (Broadcast task:moved,           │
│  └───────────┬───────────┘  comment:added, notif:new)       │
└──────────────┼──────────────────────────────────────────────┘
               ▼ Mongoose ODM
┌─────────────────────────────────────────────────────────────┐
│                 MongoDB Atlas Cluster                       │
│     - Users            - Projects        - ProjectMembers   │
│     - Tasks            - Comments        - ActivityLogs     │
│     - Notifications    (Composite unique & query indexes)   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔒 Security Architecture Highlights

- **Stateless HttpOnly Cookie Sessions:** Mitigates XSS token theft. JWT tokens are delivered exclusively in `HttpOnly`, `SameSite=Lax` cookies; tokens are never stored in client `localStorage`.
- **Zero-Trust Server Authorization:** Every endpoint verifies project membership and user permissions (`Owner`, `Admin`, `Member`, `Viewer`) via database lookups before executing business logic.
- **Authenticated WebSockets:** Sockets decrypt and verify session cookies during handshake; clients are prevented from subscribing to unauthorized project rooms.
- **Rate Limiting & Sanitization:** Targeted protection against brute-force authentication attacks and NoSQL query injection.

---

## 👥 Role-Based Access Control (RBAC) Matrix

| Action / Permission | Owner | Admin | Member | Viewer |
| :--- | :---: | :---: | :---: | :---: |
| **Delete Project** | ✅ | ❌ | ❌ | ❌ |
| **Transfer Ownership** | ✅ | ❌ | ❌ | ❌ |
| **Project Settings** | ✅ | ✅ | ❌ | ❌ |
| **Invite / Remove Members** | ✅ | ✅ | ❌ | ❌ |
| **Change Member Roles** | ✅ | ✅ (Non-owner) | ❌ | ❌ |
| **Create / Move / Edit Tasks** | ✅ | ✅ | ✅ | ❌ |
| **Assign / Unassign Members** | ✅ | ✅ | ✅ | ❌ |
| **Post Comments & Mentions** | ✅ | ✅ | ✅ | ❌ (Read-only) |
| **View Board, Tasks & Activity** | ✅ | ✅ | ✅ | ✅ |

---

## 🛠️ Full-Stack Technology Stack

| Layer | Technology | Version | Purpose |
| :--- | :--- | :--- | :--- |
| **Frontend** | Vanilla ES6+, HTML5, CSS3 | Standard | Blazing-fast DOM rendering, zero build-step overhead, native drag & drop |
| **Styling** | Tailwind CSS / Material 3 Tokens | v3+ / M3 | Design system tokens, responsive flexbox & grid |
| **Server** | Node.js & Express | v20+ LTS / v4.19+ | RESTful API controllers, security middleware |
| **Real-Time** | Socket.IO | v4.7+ | Duplex room-isolated event synchronization |
| **Database** | MongoDB Atlas / Mongoose | v8+ | Document store with compound indexes and validation schemas |
| **Auth** | JWT & bcryptjs | Standard | Stateless HttpOnly cookie authentication |
| **Testing** | Supertest & Playwright | Latest | Automated API, RBAC security gates, and multi-browser E2E testing |

---

## 📁 Repository Structure

```
TaskFlow-Workspace/
├── README.md                      # Master architecture & project documentation
├── docs/                          # Phase 1 technical specification library
│   ├── PRD.md                     # Product Requirements Document
│   ├── TRD.md                     # Technical Requirements Document
│   ├── APP_FLOW.md                # Application Flow & User Journeys
│   ├── DATABASE_SCHEMA.md         # Mongoose Schemas & ERD Models
│   ├── UI_UX_BRIEF.md             # UI/UX Design System Brief
│   ├── IMPLEMENTATION_PLAN.md     # 7-Milestone Development Roadmap
│   ├── TEST_PLAN.md               # QA Testing & Verification Strategy
│   ├── DESIGN.md                  # Material Design 3 Theme Tokens
│   └── infographics/              # High-resolution architectural infographics
│       ├── TaskFlow Application Flow Infographic.png
│       ├── TaskFlow Backend Database Schema.png
│       ├── TaskFlow Full-Stack Implementation Roadmap.png
│       ├── TaskFlow Product Requirements Infographic.png
│       ├── TaskFlow Project Documentation Board.png
│       ├── TaskFlow Technical Requirements Infographic.png
│       └── TaskFlow UI_UX Design Brief.png
└── prototypes/                    # Interactive UI prototypes
    ├── index.html                 # Unified prototype navigator & launcher
    ├── dashboard/                 # Workspace metrics & portfolio dashboard
    │   ├── index.html
    │   └── screen.png
    ├── kanban/                    # Interactive drag-and-drop Kanban board
    │   ├── index.html
    │   └── screen.png
    └── landing/                   # High-converting SaaS landing page
        ├── index.html
        └── screen.png
```

---

## 🚀 Target Development Milestones

- **Milestone 1:** Environment setup, Express scaffolding, MongoDB Atlas connection, bcrypt + JWT cookie auth.
- **Milestone 2:** Workspace projects and granular RBAC authorization engine (`owner`, `admin`, `member`, `viewer`).
- **Milestone 3:** Kanban engine, task CRUD endpoints, and native drag-and-drop state management.
- **Milestone 4:** Real-time duplex synchronization via Socket.IO room isolation.
- **Milestone 5:** Task discussion drawer, `@mention` matching, and in-app notification center.
- **Milestone 6:** Activity audit logging, mobile responsive polish, and keyboard shortcuts.
- **Milestone 7:** Automated testing with Supertest & Playwright, security auditing, and cloud deployment.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
