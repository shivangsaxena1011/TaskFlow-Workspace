# TaskFlow — UI/UX Design Brief & Design System (UI_UX_BRIEF.md)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Lead UI/UX Designer & Frontend Architect  
**Date:** October 2026  

---

![UI/UX Design Brief](infographics/TaskFlow%20UI_UX%20Design%20Brief.png)


## 1. Design Principles & Brand Identity

TaskFlow embodies a **modern, clean, and professional SaaS aesthetic** reminiscent of top-tier tools like Linear, GitHub Projects, and Asana. 

### Key Visual Tenets:
1. **Clarity & Content Density:** High information density without visual clutter. Generous whitespace around interactive boundaries and clear visual hierarchy.
2. **Instant Micro-Feedback:** Smooth transitions for drag interactions, hover states, modal entries, and real-time live pulse badges.
3. **Responsive-First Architecture:** Uncompromised experience from 375px mobile viewports (stacked column navigation, swipeable tabs) up to ultra-wide 4K multi-column monitors.

---

## 2. Color Palette & Design Tokens

```css
:root {
  /* Brand Primary Palette */
  --color-primary-50:  #EEF2FF;
  --color-primary-100: #E0E7FF;
  --color-primary-500: #6366F1; /* Primary Indigo */
  --color-primary-600: #4F46E5;
  --color-primary-700: #4338CA;

  /* Secondary / Accent */
  --color-secondary-500: #8B5CF6; /* Purple */
  --color-accent-500:    #06B6D4; /* Cyan */

  /* Neutral Surface Palette (Light Theme Baseline) */
  --color-surface-bg:   #F8FAFC; /* Slate 50 */
  --color-surface-card: #FFFFFF;
  --color-surface-border: #E2E8F0; /* Slate 200 */
  --color-text-primary: #0F172A; /* Slate 900 */
  --color-text-secondary: #64748B; /* Slate 500 */
  --color-text-muted:   #94A3B8; /* Slate 400 */

  /* Semantic Feedback */
  --color-success: #10B981; /* Emerald */
  --color-warning: #F59E0B; /* Amber */
  --color-danger:  #EF4444; /* Rose */
  --color-info:    #3B82F6; /* Blue */

  /* Elevation Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  --shadow-drawer: -4px 0 24px rgba(0, 0, 0, 0.15);

  /* Radius */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-full: 9999px;
}
```

---

## 3. Typography Hierarchy

Primary Font: `Inter`, system-ui, -apple-system, sans-serif.

| Level | Size | Weight | Line Height | Usage |
| :--- | :--- | :--- | :--- | :--- |
| **Display / H1** | 32px (2rem) | 700 (Bold) | 1.2 | Landing page hero, primary dashboard greeting |
| **Heading 2 / H2** | 24px (1.5rem) | 600 (Semi-Bold) | 1.3 | Project titles, modal headers |
| **Heading 3 / H3** | 18px (1.125rem) | 600 (Semi-Bold) | 1.4 | Column headers, widget titles |
| **Body Large** | 16px (1rem) | 500 (Medium) | 1.5 | Primary buttons, navigation items |
| **Body Regular** | 14px (0.875rem) | 400 (Regular) | 1.5 | Task card descriptions, comment text, table cells |
| **Caption / Tiny** | 12px (0.75rem) | 500 (Medium) | 1.4 | Badges, timestamps, avatar initials |

---

## 4. Key UI Screens & Component Layout Specifications

### 4.1 Authentication Screen (Login / Signup)
- Split screen or centered elevated modal (max-width 440px).
- TaskFlow logo with indigo gradient wave icon and clean wordmark.
- Clean inputs with focus ring (`--color-primary-500`), password visibility toggle, clear validation feedback, and persistent "Remember me" toggle.

### 4.2 Dashboard (Overview & Projects Hub)
- **Top Navigation Bar:**
  - Left: Brand logo + workspace switcher dropdown.
  - Center: Global search input (`Ctrl+K` shortcut cue).
  - Right: Quick `+ Create` button, real-time notification bell (with unread badge counter), and user avatar dropdown menu.
- **Main Area:**
  - Quick summary metric cards: *Active Projects*, *Tasks Assigned to Me*, *Due Soon*, *Completed This Week*.
  - Projects grid displaying project cards with member avatar stacks, task progress completion ring, and settings menu.

### 4.3 Kanban Project Board (Core Experience)
- **Sub-header Bar:**
  - Project name, star favorite icon, visibility pill (`Private`), member avatar pile with `+ Invite` button.
  - View switcher: `Board` (active), `List`, `Activity`, `Settings`.
  - Filter bar: Filter by assignee, priority tag, or text query.
- **Board Grid:**
  - 4 standard columns: `To Do`, `In Progress`, `Review`, `Done`.
  - Column header includes column title, counter badge (`4`), and quick `+ Add Card` button.
  - Column cards with draggable handles, priority pill (e.g. `High` in Rose), tag chips, assignee avatar, comment count icon, and due date indicator (highlighted amber if due < 24h).

### 4.4 Task Detail & Conversation Drawer
- Slides in from right viewport (width 540px on desktop, full-width on mobile).
- Breadcrumbs indicating project and column status.
- Inline editable task title and markdown description editor.
- Metadata sidebar: Assignee picker dropdown, priority selector, due date calendar picker, tags.
- Embedded conversation thread: Chronological comments, markdown support, avatar headers, and relative timestamps ("10 minutes ago").

### 4.5 Responsive Mobile Breakpoint Adaptations
- Under 768px:
  - Collapsible sidebar transforms into bottom navigation bar or top hamburger drawer.
  - Kanban board features a horizontal column swipe tab bar (`[To Do (3)] [In Progress (2)] [Done (5)]`) to prevent awkward horizontal squishing while preserving drag-drop.
