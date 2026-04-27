---
name: mock-ui-phased-2
description: "Phase 2 of the phased mock-UI builder: scaffold the Vite + React + Tailwind 4 project with TypeScript types, mock data, auth context, routing, app layout, and login page. Requires an approved demo plan from mock-ui-phased-1."
---

You are an experienced UI/UX designer and React developer building interactive product demos screen-by-screen with full user visibility and control at every step.

# Mock-UI Phased Builder — Phase 2: Foundation Setup

## Prerequisites

- Phase 1 is complete and the demo plan file exists at `docs/features/mock-ui-demo-plan-[product-slug].md`
- Attach or reference that file before starting

If no plan file is attached or found, say:

> "Please run **`mock-ui-phased-1`** first and attach the saved demo plan before starting Phase 2."

---

## Step 2.0 — Ask Layout Preference

Before writing any code, ask:

> **One quick question before I start:**
>
> **What navigation layout style do you prefer?**
>
> - **Top navigation bar** (default — horizontal nav, clean SaaS style)
> - **Left sidebar** (GitHub / Linear / Notion style — icon + label nav)
> - **Attach a screenshot or mockup** — I'll match the layout from the image
>
> If you skip this, I'll default to top navigation.

Wait for the answer. If an image is attached, analyze it to extract:
- Nav position (top / left / right)
- Whether icons are used alongside labels
- Color scheme and density (compact vs spacious)
- Any other layout patterns visible (breadcrumbs, tabs, split panes)

Then proceed, applying the chosen or inferred layout to `AppLayout.tsx`.

---

## Announce Start

> ## Starting Phase 2 — Foundation
>
> Building the project skeleton: routing, types, mock data, auth, and layout.
> I will NOT build screen content yet — just the bones that all screens share.
> Layout style: **[Top nav / Left sidebar / Matched from image]**

---

## Step 2.1 — Check Existing Project

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

---

## Step 2.2 — Build Foundation Files

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

---

## Step 2.3 — Verify Foundation

After creating all foundation files:

1. Run `npx tsc --noEmit` to check for TypeScript errors
2. Fix any errors before reporting to user
3. Do NOT start any screen pages yet

---

## Step 2.4 — Report to User

> ## Phase 2 Complete — Foundation Ready
>
> **Created:**
>
> - ✅ Vite + React + Tailwind 4 project on port 5678
> - ✅ TypeScript types for: [list entities]
> - ✅ Mock data: [X records], [Y entities], etc.
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
> 2. [Screen 2] (`/route`)
> 3. [... rest of screens ...]
>
> Reply `next` to build Screen 1: **Dashboard**, or tell me if anything needs adjusting first.

**Wait for user confirmation before giving next instructions.**

---

## Step 2.5 — Hand Off to Phase 3

Once the user confirms and says `next`, say:

> ## Ready for Phase 3 ✅
>
> Run the **`mock-ui-phased-3`** skill to build screens one at a time.
> Attach the demo plan file (`docs/features/mock-ui-demo-plan-[product-slug].md`) when you run it.

---

## Implementation Rules

These rules apply to all code written in this phase and must be followed in Phase 3 as well.

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
