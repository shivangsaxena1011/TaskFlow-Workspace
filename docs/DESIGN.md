---
name: TaskFlow
colors:
  surface: '#faf8ff'
  surface-dim: '#d2d9f4'
  surface-bright: '#faf8ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f2f3ff'
  surface-container: '#eaedff'
  surface-container-high: '#e2e7ff'
  surface-container-highest: '#dae2fd'
  on-surface: '#131b2e'
  on-surface-variant: '#464554'
  inverse-surface: '#283044'
  inverse-on-surface: '#eef0ff'
  outline: '#767586'
  outline-variant: '#c7c4d7'
  surface-tint: '#494bd6'
  primary: '#4648d4'
  on-primary: '#ffffff'
  primary-container: '#6063ee'
  on-primary-container: '#fffbff'
  inverse-primary: '#c0c1ff'
  secondary: '#6b38d4'
  on-secondary: '#ffffff'
  secondary-container: '#8455ef'
  on-secondary-container: '#fffbff'
  tertiary: '#006577'
  on-tertiary: '#ffffff'
  tertiary-container: '#008096'
  on-tertiary-container: '#f9fdff'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#e1e0ff'
  primary-fixed-dim: '#c0c1ff'
  on-primary-fixed: '#07006c'
  on-primary-fixed-variant: '#2f2ebe'
  secondary-fixed: '#e9ddff'
  secondary-fixed-dim: '#d0bcff'
  on-secondary-fixed: '#23005c'
  on-secondary-fixed-variant: '#5516be'
  tertiary-fixed: '#acedff'
  tertiary-fixed-dim: '#4cd7f6'
  on-tertiary-fixed: '#001f26'
  on-tertiary-fixed-variant: '#004e5c'
  background: '#faf8ff'
  on-background: '#131b2e'
  surface-variant: '#dae2fd'
typography:
  display-lg:
    fontFamily: Inter
    fontSize: 3rem
    fontWeight: '700'
    lineHeight: 3.5rem
    letterSpacing: -0.025em
  headline-xl:
    fontFamily: Inter
    fontSize: 2.25rem
    fontWeight: '600'
    lineHeight: 2.75rem
    letterSpacing: -0.02em
  headline-xl-mobile:
    fontFamily: Inter
    fontSize: 1.75rem
    fontWeight: '600'
    lineHeight: 2.25rem
    letterSpacing: -0.015em
  headline-lg:
    fontFamily: Inter
    fontSize: 1.5rem
    fontWeight: '600'
    lineHeight: 2rem
    letterSpacing: -0.015em
  headline-sm:
    fontFamily: Inter
    fontSize: 1.25rem
    fontWeight: '600'
    lineHeight: 1.75rem
    letterSpacing: -0.01em
  title-md:
    fontFamily: Inter
    fontSize: 1.125rem
    fontWeight: '500'
    lineHeight: 1.5rem
    letterSpacing: -0.01em
  body-lg:
    fontFamily: Inter
    fontSize: 1rem
    fontWeight: '400'
    lineHeight: 1.5rem
    letterSpacing: -0.005em
  body-md:
    fontFamily: Inter
    fontSize: 0.875rem
    fontWeight: '400'
    lineHeight: 1.25rem
    letterSpacing: 0em
  body-sm:
    fontFamily: Inter
    fontSize: 0.75rem
    fontWeight: '400'
    lineHeight: 1rem
    letterSpacing: 0em
  label-md:
    fontFamily: Inter
    fontSize: 0.875rem
    fontWeight: '500'
    lineHeight: 1.25rem
    letterSpacing: 0.005em
  label-sm:
    fontFamily: Inter
    fontSize: 0.75rem
    fontWeight: '500'
    lineHeight: 1rem
    letterSpacing: 0.01em
  label-xs:
    fontFamily: Inter
    fontSize: 0.6875rem
    fontWeight: '600'
    lineHeight: 0.875rem
    letterSpacing: 0.02em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  gutter: 1rem
  gutter-tablet: 1.25rem
  gutter-desktop: 1.5rem
  margin: 1rem
  margin-tablet: 1.5rem
  margin-desktop: 2rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 0.75rem
  space-lg: 1rem
  space-xl: 1.5rem
---

## Brand & Style

This design system embodies high-velocity SaaS execution tailored for cross-functional engineering, product, and operations teams. Rooted in modern productivity philosophies, the visual identity pairs relentless clarity with fluid responsiveness. It optimizes for sustained focus, immediate spatial comprehension, and frictionless keyboard-driven workflows.

The design movement mixes **Corporate / Modern minimalism** with **subtle technical precision**:
- **Information Density with Breathing Room**: Tight, metric-driven alignments paired with deliberate micro-spacing keep data scan times minimal without overwhelming cognitive load.
- **Engineered Calm**: High contrast in typography is anchored against quiet slate surfaces, avoiding visual noise during prolonged sprint planning or issue triage sessions.
- **Kinetic Precision**: State transitions, hover effects, and modals communicate real-time synchronization, speed, and confidence.

## Colors

The palette balances vivid technical hues against a structural slate foundation:

- **Primary (`#6366F1`) & Primary Dark (`#4F46E5`)**: Applied to key interactive triggers, selected navigation routes, active filters, and confirmed task states.
- **Secondary (`#8B5CF6`)**: Reserved for cross-project initiatives, milestones, sprint epics, and automation triggers.
- **Tertiary Accent (`#06B6D4`)**: Used for velocity metrics, live collaborator cursors, telemetry badges, and analytical highlights.
- **Surfaces**:
  - Global Canvas: `#F8FAFC`
  - Cards & Floating Panels: `#FFFFFF`
  - Subtle Borders & Dividers: `#E2E8F0`
- **Typography Tokens**:
  - Primary Text: `#0F172A`
  - Secondary Text: `#64748B`
  - Muted Text & Placeholders: `#94A3B8`
- **Semantic Statuses**:
  - Emerald (`#10B981`): Completed issues, healthy deploy signals, positive velocity.
  - Amber (`#F59E0B`): Blocked tasks, impending sprint cutoffs, review requests.
  - Rose (`#EF4444`): P0/Critical bugs, failed builds, destructive actions.
  - Blue (`#3B82F6`): In-progress indicators, contextual tooltips, active sync states.

## Typography

Inter serves as the singular typeface across all hierarchical roles, leveraging its native x-height and tabular figures for high-density interfaces:

- **Headlines & Page Titles**: Tightly tracked (`-0.025em` to `-0.015em`) with semi-bold to bold weights to establish crisp visual anchors across kanban headers and workspace overviews.
- **Body Content**: Standardized at `0.875rem` (`body-md`) for task descriptions, comments, and changelogs to maximize readability while accommodating split-pane layouts.
- **Labels & Micro-copy**: Elevated to medium or semi-bold weights with slight tracking expansion (`+0.01em` to `+0.02em`) to guarantee legibility inside compact issue badges, status pills, and keyboard shortcut glyphs.

## Layout & Spacing

The system implements a fluid, high-density layout anchored on a strict **4px/8px baseline grid rhythm**:

- **App Shell Architecture**:
  - Collapsible primary navigation rail: `240px` fixed width on desktop; collapses to icon-only `56px` rail on compact desktop screens.
  - Dynamic canvas: Fluid multi-column board (Kanban) or list layout utilizing horizontal scrolling with sticky column headers when columns exceed viewport boundaries.
  - Contextual peek drawer/modal: Overlays canvas at `640px` or `45vw` width for deep-dive issue inspection.
- **Breakpoints**:
  - **Mobile (< 768px)**: Single-column view with bottom navigation; margins collapse to `margin` (`1rem`). Kanban converts to tabbed column carousels.
  - **Tablet (768px - 1023px)**: 2-column or list-first view with collapsible sidebar overlay; margins scale to `margin-tablet` (`1.25rem`).
  - **Desktop (≥ 1024px)**: Full multi-column view with persistent workspace nav and secondary detail inspectors; section margins scale to `margin-desktop` (`2rem`).

## Elevation & Depth

Visual hierarchy is maintained through subtle, low-opacity ambient shadows paired with razor-thin borders (`1px solid #E2E8F0`):

- **Base Surfaces (`Level 0`)**: The global page canvas (`#F8FAFC`) has zero elevation.
- **Flat Containers (`Level 1`)**: Kanban columns, table rows, and list cards sit directly on the canvas, bounded by `#E2E8F0` borders without drop shadows.
- **Interactive Cards on Hover (`Level 2`)**: Interactive task cards lift with a featherweight ambient shadow:
  - `0 1px 3px 0 rgba(15, 23, 42, 0.04), 0 1px 2px -1px rgba(15, 23, 42, 0.03)`
- **Floating Overlays & Popovers (`Level 3`)**: Dropdowns, command palettes (Cmd+K), and filter flyouts:
  - Surface: `#FFFFFF`
  - Shadow: `0 4px 6px -1px rgba(15, 23, 42, 0.07), 0 2px 4px -2px rgba(15, 23, 42, 0.05)`
  - Border: `1px solid rgba(226, 232, 240, 0.9)`
- **Translucent Modals & Backdrops (`Level 4`)**: Issue detail sheets and confirmation modals:
  - Backdrop: `rgba(15, 23, 42, 0.4)` paired with `backdrop-filter: blur(4px)`
  - Surface: `#FFFFFF` with `0 20px 25px -5px rgba(15, 23, 42, 0.08), 0 8px 10px -6px rgba(15, 23, 42, 0.04)`

## Shapes

The system uses a **Soft (Level 1)** geometric curve discipline to maintain structural rigor across technical interfaces:

- **Base Components (`rounded`: `0.25rem`)**: Applied to inline status tags, input fields, dropdown trigger buttons, table rows, and keyboard shortcut chips (`kbd`).
- **Containers & Modals (`rounded-lg`: `0.5rem`)**: Applied to task cards, popovers, contextual drawers, and modal dialogue containers.
- **Section Enclosures (`rounded-xl`: `0.75rem`)**: Applied to high-level workspace boards, empty states, and project summary dashlets.
- **Pill Exception**: Avatar initials, user indicators, and notification count pips utilize full circular radii (`rounded-full` / `9999px`) to visually contrast against square structural data blocks.

## Components

### Buttons
- **Primary**: Solid background `#6366F1` with white text. Hover state shifts to `#4F46E5`. Active state applies `transform: scale(0.98)`. Border radius is `0.25rem`.
- **Secondary / Ghost**: White surface with `1px solid #E2E8F0` border and `#0F172A` text. Hover shifts to `#F8FAFC` background and `#CBD5E1` border.
- **Subtle / Utility**: Transparent background, `#64748B` text, transitioning to `#F1F5F9` on hover for high-frequency actions (e.g., issue quick-add, column overflow menus).

### Chips & Status Badges
- **Semantic Badges**: Compact padding (`0.125rem 0.375rem`), `label-xs` typography, `0.25rem` radius.
  - In Progress: Tinted cyan background (`rgba(6, 182, 212, 0.1)`), `#0891B2` text.
  - Review: Tinted amber background (`rgba(245, 158, 11, 0.1)`), `#D97706` text.
  - Done: Tinted emerald background (`rgba(16, 185, 129, 0.1)`), `#059669` text.
  - Priority (Urgent): Tinted rose background (`rgba(239, 68, 68, 0.1)`), `#DC2626` text.

### Task Cards (Kanban / Backlog)
- Background: `#FFFFFF`.
- Border: `1px solid #E2E8F0`.
- Padding: `space-md` (`0.75rem`).
- Content order: Issue identifier (e.g., `TASK-104`) + status icon on header row, task title in `body-md` medium weight, followed by assignee avatar and due-date pill in footer row.

### Form Inputs & Selects
- Height: `2rem` (compact) or `2.25rem` (default).
- Surface: `#FFFFFF` with `1px solid #E2E8F0` border and `0.25rem` radius.
- Focus: `border-color: #6366F1` with a `2px` focus ring tinted at `rgba(99, 102, 241, 0.2)`.

### Checkboxes & Selection Rows
- Checkboxes: `1rem x 1rem`, `0.25rem` radius, `#E2E8F0` border. When checked, fills `#6366F1` with an inner white check glyph.
- Row Selection: Active items receive an inset left border highlight of `2px solid #6366F1` and a soft background tint of `rgba(99, 102, 241, 0.04)`.

### Command Palette (Cmd+K)
- Centered overlay modal with backdrop blur.
- Input: Flush borderless search bar at `headline-sm` font scale.
- Results list: Categorized sections (Recent Tasks, Navigation, Quick Actions) with keyboard shortcut badges pinned to right edge.