# Slide Console Style Guide

This document defines the visual design language for the Slide Console application. Use these guidelines when creating new pages or components.

---

## Color Palette

### Light Mode

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-primary` | `#ffffff` | Page background, cards |
| `--bg-secondary` | `#f7f9fc` | Subtle backgrounds, table rows (alternating) |
| `--bg-sidebar` | `#ffffff` | Sidebar navigation |
| `--bg-header` | `#ffffff` | Header background |
| `--text-primary` | `#1a1a2e` | Main body text |
| `--text-secondary` | `#6b7280` | Labels, muted text |
| `--border-default` | `#e5e7eb` | Borders, dividers |
| `--accent-primary` | `#2196f3` | Primary actions, active navigation items, links |
| `--accent-success` | `#4caf50` | Success states, positive indicators, checkmarks |
| `--accent-warning` | `#f59e0b` | Warning states, "behind" indicators |
| `--accent-danger` | `#ef4444` | Error states, failed operations |
| `--accent-info` | `#3b82f6` | Informational badges, secondary actions |

### Dark Mode

| Token | Value | Usage |
|-------|-------|-------|
| `--bg-primary` | `#1a1a2e` | Page background |
| `--bg-secondary` | `#252541` | Cards, panels, elevated surfaces |
| `--bg-sidebar` | `#1a1a2e` | Sidebar navigation |
| `--bg-header` | `#1a1a2e` | Header background |
| `--text-primary` | `#f3f4f6` | Main body text |
| `--text-secondary` | `#9ca3af` | Labels, muted text |
| `--border-default` | `#374151` | Borders, dividers |
| `--accent-primary` | `#3b82f6` | Primary actions, active navigation |
| `--accent-success` | `#22c55e` | Success states |
| `--accent-warning` | `#eab308` | Warning states |
| `--accent-danger` | `#ef4444` | Error states |

---

## Typography

### Font Family
- **Primary**: System font stack (clean, readable sans-serif)
- **Monospace**: For code, IP addresses, version numbers

### Type Scale

| Element | Size | Weight | Line Height |
|---------|------|--------|-------------|
| Page Title (h1) | 24px | 600 | 1.3 |
| Section Title (h2) | 18px | 600 | 1.4 |
| Card Title (h3) | 16px | 600 | 1.4 |
| Body Text | 14px | 400 | 1.5 |
| Small/Caption | 12px | 400 | 1.4 |
| Button Text | 14px | 500 | 1 |

---

## Layout

### Page Structure

```
┌─────────────────────────────────────────────────────┐
│ Header (56px height)                                │
├──────────┬──────────────────────────────────────────┤
│ Sidebar  │ Main Content Area                        │
│ (200px)  │                                          │
│          │                                          │
│          │                                          │
└──────────┴──────────────────────────────────────────┘
```

### Header
- Fixed height: 56px
- Background: follows theme (light `#ffffff` / dark `#1a1a2e`)
- Contains: hamburger menu, theme toggle, logo (centered)
- Logo displays "slide" with sparkle icon
- Border-bottom: subtle border matching theme (`--border-default`)
- Box shadow for subtle elevation

### Sidebar Navigation
- Width: 200px (collapsible)
- Grouped sections separated by subtle dividers
- Active item: blue text (`#2196f3`) with blue left border indicator
- Icons: 20px, positioned left of text with 12px gap
- Item padding: 12px vertical, 16px horizontal
- Hover state: subtle background highlight

### Main Content
- Max-width: contained or full-width depending on content type
- Padding: 24px
- Background: theme background color

---

## Components

### Cards

```css
.card {
  background: var(--bg-primary);
  border-radius: 8px;
  border: 1px solid var(--border-default);
  padding: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}
```

**Card Header**:
- Title left-aligned
- Optional "See All" link right-aligned, blue text

### Stat Cards (Dashboard)

- Grid layout: responsive 2-4 columns
- Large number display: 32px, bold
- Subtext: 12px, muted color
- Success indicator: green checkmark icon aligned right
- Background: slightly elevated surface

### Data Tables

**Table Header**:
- Background: slightly darker than body
- Font-weight: 600
- Text-transform: none (sentence case)
- Sortable columns: arrow indicator, blue text when active

**Table Rows**:
- Light mode: white background, hover to `#f7f9fc`
- Dark mode: `#252541` background, hover to lighter shade
- Selected row: blue left border accent
- Height: 48-52px
- Cell padding: 12px 16px

**Column Types**:
- Name: bold, primary text color
- Status: badge component (see Badges)
- Links: blue text, underline on hover
- Dates: formatted as "Dec 20, 2025 9:00 PM"
- IP Addresses: monospace font, copy icon on hover
- Icons: centered, 16-20px

### Buttons

**Primary Button**:
```css
.btn-primary {
  background: #2196f3;
  color: white;
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: 500;
  display: inline-flex;
  align-items: center;
  gap: 8px;
}
```

**Secondary Button**:
```css
.btn-secondary {
  background: transparent;
  color: var(--accent-primary);
  border: 1px solid var(--border-default);
  padding: 8px 16px;
  border-radius: 6px;
}
```

**Icon Button**:
```css
.btn-icon {
  padding: 8px;
  border-radius: 6px;
  background: transparent;
}
```

**Action Button** (table actions):
- Dark background: `#374151`
- White text
- Smaller size: 8px 12px padding
- Include icon (like X for "UNDO RESOLVE")

### Badges/Pills

**Status Badge**:
```css
.badge {
  padding: 4px 10px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 500;
  display: inline-flex;
  align-items: center;
  gap: 4px;
}
```

**Badge Variants**:

| Variant | Background | Text | Use Case |
|---------|------------|------|----------|
| Success | `#dcfce7` / `#166534` (dark) | `#166534` / `#22c55e` | "No issues", "Fully Replicated" |
| Warning | `#fef3c7` / `#854d0e` (dark) | `#854d0e` / `#eab308` | "13 Hours Behind" |
| Danger | `#fee2e2` / `#991b1b` (dark) | `#991b1b` / `#ef4444` | "Backup Failed" |
| Info | `#dbeafe` / `#1e40af` (dark) | `#1e40af` / `#3b82f6` | "Missed Scheduled Backup" |
| Neutral | `#f3f4f6` / `#374151` (dark) | `#6b7280` / `#9ca3af` | "Slide Box Not Checking In" |

### Form Controls

**Text Input**:
```css
.input {
  background: var(--bg-primary);
  border: 1px solid var(--border-default);
  border-radius: 6px;
  padding: 10px 12px;
  font-size: 14px;
  width: 100%;
}

.input:focus {
  border-color: var(--accent-primary);
  outline: none;
  box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
}
```

**Select/Dropdown**:
- Same styling as text input
- Chevron down icon aligned right
- Dropdown panel: elevated with shadow, same background as input

**Search Input**:
- Search icon (magnifying glass) positioned inside, left
- Placeholder: "Search"
- Clear button appears when filled

### Detail Panel (Drawer)

- Slides in from right
- Width: 400-500px
- Header: close button (X) top-right
- Tabbed navigation: "OVERVIEW", "ALERTS", "SETTINGS"
- Active tab: blue underline indicator
- Content sections with clear visual separation

**Detail Header**:
- Large icon/thumbnail (64px)
- Title below icon
- Subtitle/meta info in muted text
- Primary action button (e.g., "BACKUP NOW")

**Info List**:
- Two-column layout
- Label: muted text, left-aligned
- Value: primary text, right-aligned
- Rows separated by subtle dividers or spacing

### Progress Bars

```css
.progress {
  height: 8px;
  background: var(--bg-secondary);
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #2196f3, #21cbf3);
  border-radius: 4px;
}
```

### Charts

**Bar Charts**:
- Green bars for success (`#4caf50`)
- Gray bars for failure/neutral
- Y-axis labels: muted text
- X-axis: date labels, rotated if needed
- Legend: dot + label, positioned top-right

**Color coding**:
- Success/Replicated: green (`#4caf50`)
- Failure/Not Replicated: gray (`#9ca3af`)

### Status Indicators

**Online/Connected Dot**:
- Size: 8px circle
- Green (`#22c55e`) for healthy
- Positioned left of item name

**Checkmark Icons** (backup attempts):
- Small checkmarks in a row
- Green for success, gray outline for pending

### Alert/Notification Banner

```css
.alert-success {
  background: #dcfce7;
  border: 1px solid #86efac;
  border-radius: 8px;
  padding: 16px;
  color: #166534;
}
```

Include:
- Icon left-aligned
- Title: bold
- Description: regular weight
- Action link if applicable

---

## Iconography

- Icon library: Consistent line-weight icons
- Size: 16px (inline), 20px (navigation), 24px (headers)
- Color: inherit from text color or specific semantic color

**Common Icons**:
- Dashboard: grid/squares
- Protected Systems: monitor/desktop
- Slide Boxes: server/box
- Snapshots: camera/snapshot
- Restores: clock/history
- Alerts: flag/exclamation
- Settings: gear/cog
- Users: person
- Clients: building
- Networks: nodes/connections
- Billing: credit card
- Logs: list/document

---

## Spacing Scale

| Token | Value |
|-------|-------|
| `--space-xs` | 4px |
| `--space-sm` | 8px |
| `--space-md` | 16px |
| `--space-lg` | 24px |
| `--space-xl` | 32px |
| `--space-2xl` | 48px |

---

## Responsive Behavior

### Breakpoints

| Name | Width | Behavior |
|------|-------|----------|
| Mobile | < 768px | Sidebar collapsed, hamburger menu |
| Tablet | 768px - 1024px | Sidebar collapsible |
| Desktop | > 1024px | Sidebar always visible |

### Mobile Adaptations
- Tables become cards or horizontally scrollable
- Multi-column layouts stack vertically
- Detail panel becomes full-screen modal

---

## Animation & Transitions

```css
/* Default transition */
transition: all 0.15s ease;

/* Page transitions */
transition: opacity 0.2s ease, transform 0.2s ease;

/* Drawer slide */
transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
```

### Micro-interactions
- Button hover: slight background color shift
- Row hover: background highlight
- Active states: immediate feedback (no delay)
- Loading states: skeleton screens or spinner

---

## Empty States

When no data is available:
- Centered illustration (optional, like the whale mascot)
- Clear heading: "Lost at sea?" or contextual message
- Helpful description
- Suggested action links

---

## Accessibility

- Maintain 4.5:1 contrast ratio minimum
- Focus states visible with outline or ring
- Interactive elements minimum 44x44px touch target
- ARIA labels on icon-only buttons
- Skip to content link available

---

## Dark Mode Implementation

Toggle via button in header (moon/sun icon). Colors adjust via CSS custom properties:

```css
:root {
  --bg-primary: #ffffff;
  /* ... light mode values */
}

[data-theme="dark"] {
  --bg-primary: #1a1a2e;
  /* ... dark mode values */
}
```

Both modes should feel cohesive with the same visual hierarchy and component structure.

