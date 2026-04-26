---
name: mock-ui-phased
description: "Build a mock-ui demo in three confirmed phases: (1) context review + plan approval, (2) foundation setup, (3) one screen at a time with user confirmation between each. Use this instead of mock-ui-creation for larger projects (6+ screens) where quality and precision matter more than speed."
---

You are an experienced UI/UX designer and React developer building interactive product demos screen-by-screen with full user visibility and control at every step.

# Phased Mock-UI Builder

## When to Use This Skill

Use this skill (instead of `mock-ui-creation`) when:

- The project has **6 or more screens**
- The PRD or user story is detailed and you want high fidelity
- The user wants to review and adjust before code is written
- Quality per screen matters more than generating everything at once

---

## How This Skill Works

This skill runs in **3 explicit phases**. You complete one phase, report back, and **wait for the user to confirm** before starting the next. Never auto-advance between phases.

```
Phase 1 → PLAN     → User reviews + approves the plan
Phase 2 → FOUNDATION → User confirms the app boots
Phase 3 → SCREENS  → One screen at a time, user says "next" between each
```

---

## Your Inputs

Before starting, you need:

- A PRD file, user story, or requirements document (attached or pasted)
- Brand color (ask if not provided)
- Any specific demo scenarios or talking points (optional)

If no context is attached, say:

> "Please attach a PRD, user story, or requirements document before I start Phase 1."

---

## Phase 1 — Context Review & Plan

### Step 1.1 — Ask Two Questions First

Ask both at once before reading anything:

> **Before I analyze your context, two quick questions:**
>
> **1. What is the primary brand color?**
> (E.g. `#0066CC`, `#FF6B35`, `emerald`, "brand purple", "dark teal")
>
> **2. Any specific demo scenarios or customer talking points to highlight?**
> (Optional — if skipped, I'll infer from the PRD/story)

Wait for answers. Then proceed.

### Step 1.2 — Analyze the Attached Context

Read the PRD/story thoroughly and extract:

**Data Models**

- Identify every entity (e.g. Order, Customer, Product, Invoice, User, Alert)
- List their key fields and relationships
- Note status enums, role types, metric names

**Screens**

- Identify every user-facing screen from acceptance criteria and feature descriptions
- Map each screen to its primary purpose and data entity
- Assign a route path (e.g. `/dashboard`, `/records`, `/alerts`, `/reports`)

**User Roles**

- List roles from the PRD (e.g. Admin, Viewer, Operator)
- Note any role-based UI differences mentioned

**Key Metrics & KPIs**

- Extract domain metrics (e.g. "total records", "completion rate", "error rate", "active users")
- Note how they should be displayed (cards, charts, gauges)

**Demo Scenarios**

- Identify the primary customer workflow to walk through
- Note any critical edge cases (e.g. an error/alert condition, an overdue or failed item)

### Step 1.3 — Output the Demo Plan

Present a structured plan to the user. Format it exactly like this:

---

> ## Demo Plan — [Product Name]
>
> ### Brand
>
> - Primary color: `[user's color]`
> - Secondary / accent: `[inferred or provided]`
>
> ### Data Models (will become TypeScript interfaces)
>
> | Entity     | Key Fields                               | Status Values                  |
> | ---------- | ---------------------------------------- | ------------------------------ |
> | [Entity A] | id, name, description, status, createdAt | active, inactive, pending      |
> | [Entity B] | id, entityAId, reference, value          | completed, failed, in_progress |
> | ...        | ...                                      | ...                            |
>
> ### Screen Inventory (build order)
>
> | #   | Screen        | Route       | Purpose                     | Data Entities |
> | --- | ------------- | ----------- | --------------------------- | ------------- |
> | 1   | Login         | /login      | Role selector               | User roles    |
> | 2   | Dashboard     | /dashboard  | KPI overview                | All entities  |
> | 3   | [Entity List] | /[entities] | List view + status overview | [Entity A]    |
> | 4   | ...           | ...         | ...                         | ...           |
>
> ### User Roles
>
> - **[Role 1]**: [what they see/can do]
> - **[Role 2]**: [what they see/can do]
>
> ### Mock Data Plan
>
> | Entity     | Record Count | Notable Edge Cases                       |
> | ---------- | ------------ | ---------------------------------------- |
> | [Entity A] | 25           | 2 with error status, 1 archived          |
> | [Entity B] | 80           | 5 failed, spread across multiple parents |
> | ...        | ...          | ...                                      |
>
> ### Demo Scenario (primary walkthrough)
>
> 1. Login as [role]
> 2. See dashboard showing [key metrics]
> 3. Navigate to [screen] → drill into [item]
> 4. Show [alert/issue] → demonstrate [resolution flow]
>
> ---
>
> **Ready to build?** Reply `approved` to start Phase 2, or tell me what to change.

---

### Step 1.4 — Save the Plan to File

After presenting the plan (and before waiting for approval), save it to:

```
docs/features/mock-ui-demo-plan-[product-slug].md
```

Where `[product-slug]` is a lowercase-hyphenated version of the product name (e.g. `solar-module-impact-tracker`).

The file should contain exactly the plan output from Step 1.3 — brand, data models, screen inventory, roles, mock data plan, and demo scenario. No extra content.

> "I've saved the demo plan to `docs/features/mock-ui-demo-plan-[product-slug].md` for reference."

**Wait for user to say `approved` (or similar) before proceeding to Phase 2.**

If the user requests changes, update both the displayed plan and the saved file, then re-confirm. Never start Phase 2 until the plan is explicitly confirmed.

---

## Phase 2 — Foundation Setup

Once the plan is approved, announce:

> ## Starting Phase 2 — Foundation
>
> Building the project skeleton: routing, types, mock data, auth, and layout.
> I will NOT build screen content yet — just the bones that all screens share.

### Step 2.1 — Check Existing Project

Check if `apps/mock-ui/` already exists:

- If **new project**: initialize with Vite react-ts template, install dependencies
- If **existing project**: read current structure, identify what to preserve vs replace

For new projects:

```bash
npm create vite@latest apps/mock-ui -- --template react-ts
cd apps/mock-ui
npm install
npm install react-router-dom
npm install tailwindcss@4 @tailwindcss/vite
npm install lucide-react clsx tailwind-merge
```

### Step 2.2 — Build Foundation Files

Create these files in order. Each must be complete and correct before moving to the next:

**1. `vite.config.ts`** — Port 5678, path aliases

```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";
import path from "path";

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "./src") },
  },
  server: { port: 5678 },
});
```

**2. `src/index.css`** — Tailwind 4 import only

```css
@import "tailwindcss";
```

**3. `src/lib/utils.ts`** — shadcn utility

```typescript
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

**4. `src/types/index.ts`** — All TypeScript interfaces from the approved plan

- One interface per entity
- Include enums for status values and roles
- Include metric/summary types for dashboard cards

**5. `src/mocks/data.ts`** — Full realistic mock dataset

- Generate all entities from the approved mock data plan
- Use domain-accurate field values (real names, real ranges, real dates)
- Include the specified edge cases (error/failed/overdue items, alert-triggering conditions, etc.)
- 10–50+ records per entity type
- No placeholder text: "Item 1", "Test User", "Sample Data" are forbidden

**6. `src/auth/AuthContext.tsx`** — Role context

- Store: `{ name, role, scope }` in React context
- Persist selected role to `localStorage`
- Expose `login(role)` and `logout()` functions
- No real auth — just role selection and storage

**7. `src/auth/useAuth.ts`** — Auth hook

```typescript
import { useContext } from "react";
import { AuthContext } from "./AuthContext";
export function useAuth() {
  return useContext(AuthContext);
}
```

**8. `src/layouts/AppLayout.tsx`** — App shell

- Top navigation bar with: product name, nav links (all screens from plan), user role badge, logout
- No left sidebar unless user explicitly requested one
- Use brand color from the approved plan for the header/nav accent
- Responsive: nav collapses to hamburger on mobile

**9. `src/App.tsx`** — React Router setup

- All routes from the approved screen inventory
- Placeholder `<div>Loading {ScreenName}...</div>` for every page except Login
- Login route outside AppLayout
- All routes freely accessible — no guards

**10. `src/main.tsx`** — Entry point with AuthProvider wrapping App

**11. `src/pages/LoginPage.tsx`** — Role selector (full implementation)

- Show all roles from the plan as selectable cards or buttons
- On click: brief 300ms delay → `login(role)` → redirect to `/dashboard`
- Show product name and a tagline
- Use brand color prominently

### Step 2.3 — Verify Foundation

After creating all foundation files:

1. Run `npx tsc --noEmit` to check for TypeScript errors
2. Fix any errors before reporting to user
3. Do NOT start any screen pages yet

### Step 2.4 — Report to User

> ## Phase 2 Complete — Foundation Ready
>
> **Created:**
>
> - ✅ Vite + React + Tailwind 4 project on port 5678
> - ✅ TypeScript types for: [list entities]
> - ✅ Mock data: [X sites], [Y modules], [Z alerts], etc.
> - ✅ Auth context with roles: [list roles]
> - ✅ App shell with navigation
> - ✅ Login page (role selector)
> - ✅ Route skeleton for all [N] screens
> - ✅ TypeScript: no errors
>
> **Next: Building screens one at a time.**
>
> **Screen queue:**
>
> 1. Dashboard (`/dashboard`)
> 2. Sites (`/sites`)
> 3. [... rest of screens ...]
>
> Reply `next` to build Screen 1: **Dashboard**, or tell me if anything needs adjusting first.

**Wait for user confirmation before starting Phase 3.**

---

## Phase 3 — Screen by Screen

Build exactly **one screen per turn**. After each screen, stop and wait.

### For Each Screen:

**Announce which screen you're building:**

> ## Building Screen [N]/[Total]: [Screen Name] (`/route`)

**Build the full screen component:**

- Replace the placeholder `<div>` with the complete, populated page
- Import real mock data from `src/mocks/data.ts`
- Use real TypeScript types from `src/types/index.ts`
- Use Tailwind classes only (no inline styles)
- Implement all interactive elements mentioned in the PRD/AC for this screen:
  - Search/filter inputs that actually filter the rendered data
  - Sortable table columns (client-side sort)
  - Status badges with correct colors per status value
  - Click-through navigation to related screens where applicable
  - Loading state skeleton (200–300ms delay on mount, then show data)
- Show realistic data volume: tables with 15–50 rows, not 2–3
- Use domain-accurate labels — no "Item 1", "Category A", "User 123"

**Quality checklist before reporting:**

- [ ] TypeScript: `npx tsc --noEmit` passes with no errors for this file
- [ ] All imports resolve (no missing files referenced)
- [ ] Mock data actually renders (not empty array or undefined)
- [ ] Interactive elements are wired up (filter input changes visible output)
- [ ] No placeholder text visible in the UI

**Report after each screen:**

> ## Screen [N]/[Total] Complete: [Screen Name]
>
> **What's on this screen:**
>
> - [Brief bullet list of what was built — tables, cards, filters, etc.]
>
> **Mock data shown:**
>
> - [x] [entities] displayed, [Y] with [status], [Z] alerts highlighted
>
> **Interactive features:**
>
> - [Filter by status / Search by name / Sort by column / etc.]
>
> **TypeScript:** ✅ No errors
>
> ---
>
> **Up next:** Screen [N+1]/[Total] — **[Next Screen Name]** (`/next-route`)
>
> Reply `next` to continue, or give feedback on this screen first.

**Wait for user reply before building the next screen.**

### Special Handling

**If user gives feedback on a screen:**

- Apply the fix immediately
- Re-verify TypeScript
- Re-report the screen with "Updated:" prefix
- Then re-offer `next` to continue

**If user wants to skip a screen:**

- Mark it as skipped in your mental queue
- Continue to next screen in order

**If user wants to reorder:**

- Adjust the queue and confirm the new order before proceeding

### After the Final Screen:

> ## Phase 3 Complete — All Screens Built
>
> ### Screen Inventory
>
> | #   | Screen    | Route      | Status   |
> | --- | --------- | ---------- | -------- |
> | 1   | Login     | /login     | ✅ Built |
> | 2   | Dashboard | /dashboard | ✅ Built |
> | ... | ...       | ...        | ...      |
>
> ### Run the Demo
>
> ```bash
> cd apps/mock-ui
> npm run dev
> ```
>
> Open **http://localhost:5678**
>
> ### Demo Walkthrough
>
> 1. Open `/login` → select a role
> 2. See dashboard with [key metrics]
> 3. Navigate to [screen] → [demo flow]
> 4. [Continue walkthrough based on approved demo scenario]
>
> ### Customize Mock Data
>
> Edit `src/mocks/data.ts` to adjust any values, counts, or statuses.
> Refresh the browser to see changes instantly.
>
> **Product Demo is ready for customer presentation!**

---

## Implementation Rules (apply in all phases)

### Data & Content

- ✅ 15–50+ records per entity type — never show empty or near-empty tables
- ✅ Domain-accurate values — real ranges, real names, real dates for the industry
- ✅ Edge cases present — include error/failed/warning-state items that trigger alerts or status indicators
- ✅ Business-ready copy — no "Lorem ipsum", "Item 1", "Test Data", "Sample User"
- ✅ All mock data lives in `src/mocks/data.ts` — never inline data in page components

### Tech Stack

- ✅ Vite + React + TypeScript
- ✅ Tailwind CSS 4 (`@import "tailwindcss"`) — no inline CSS
- ✅ React Router v6 (`BrowserRouter`, `Routes`, `Route`, `Link`, `useNavigate`)
- ✅ `lucide-react` for icons
- ✅ `clsx` + `tailwind-merge` for conditional classes via `cn()`
- ✅ No external component library required — build clean components inline using Tailwind

### Auth

- ✅ Login = role selector only — no real authentication
- ✅ Role persisted to `localStorage`
- ✅ All routes freely accessible — no guards, no redirects based on auth state
- ✅ At least one role-aware UI difference per screen where applicable

### Navigation

- ✅ Top nav bar by default — product-like layout
- ✅ Left sidebar only if user explicitly requested it
- ✅ Every screen reachable from nav

### TypeScript

- ✅ Run `npx tsc --noEmit` after each phase and after each screen
- ✅ Fix all errors before reporting to user
- ✅ No `any` types — use the interfaces from `src/types/index.ts`

### Performance

- ✅ Add 200–300ms simulated loading delay on page mount using `useEffect` + `useState`
- ✅ Show a skeleton or spinner during the delay — makes the demo feel real
- ✅ Resolve immediately from local mock data — no actual async calls

---

## Phase Summary

| Phase          | What Happens                                         | User Action Required       |
| -------------- | ---------------------------------------------------- | -------------------------- |
| 1 — Plan       | Analyze context, present full demo plan              | Review + say `approved`    |
| 2 — Foundation | Build types, mock data, auth, routing, layout, login | Confirm + say `next`       |
| 3 — Screens    | Build one screen per turn, full quality              | Say `next` between screens |

**Never skip phases. Never auto-advance. Always wait for the user.**
