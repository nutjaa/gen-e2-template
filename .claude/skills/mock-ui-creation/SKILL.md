---
name: mock-ui-creation
description: "Generate a fully functional product demo with realistic mock data. Creates a Vite + React + Tailwind 4 + shadcn + React Router app at apps/mock-ui/ on port 5678 that looks and feels like the real product."
---

You are an experienced UI/UX designer and React developer creating interactive product demos with realistic mock data for customer presentations and live demonstrations.

# Product Demo Generation Prompt

## Your inputs

Attach one of the following before running:

- A user story file from `docs/features/user-stories/` (e.g. `story-001-feature-name.md`)
- Customer requirements or feature specification (paste or attach)
- Product specification with data models and business context
- PRD file for domain understanding and demo scenarios

If no context is provided, ask the user to attach a story or PRD before continuing.

---

## Before generating — ask TWO questions

Ask the user both questions at once:

> **1. What is the primary brand color for this demo?**  
> (E.g. `#0066CC`, `#FF6B35`, `emerald`, or describe: "brand purple", "dark green")
>
> **2. Any specific demo scenarios or customer talking points you want to highlight?**  
> (Optional: Also specify secondary color, accent color, or "dark mode preferred")
>
> _Note: Screens, data models, and metrics will be automatically extracted from your attached user story or PRD._

Wait for answers before generating.

---

## Task

Analyze the provided user story, PRD, or requirements to extract all screens/pages and data models. Then generate a fully functional Vite + React + Tailwind 4 + shadcn + React Router product demo that runs immediately with **realistic mock data populated throughout**.

**Automatic extraction from attached context:**

- ✅ Identify all data models from the PRD or story (customers, orders, transactions, products, etc.)
- ✅ Extract screen names from feature specs and acceptance criteria
- ✅ Infer metrics and KPIs based on domain (e.g., financial app → show revenue, transaction volume, etc.)
- ✅ Generate realistic sample data that demonstrates the product's value

**Screen detection:**

- Read acceptance criteria and feature specs to identify user-facing screens/pages
- Extract screen names from descriptions (e.g. AC mentions "dashboard with data table" → create DataDashboard)
- If unclear, infer from feature context and product flow

**Mock data generation:**

- Generate realistic sample data that shows the product in action (not empty states)
- Create 10–50+ sample records per entity type so tables/lists look real
- Use contextually correct values for your domain (realistic amounts, dates, statuses)
- Store all mock data in `src/mocks/data.ts` for easy reuse and customization

**Customer-ready content language:**

- Use **real, business-ready UI copy** suitable for customer sign-off.
- Do not use placeholder or fake-looking labels such as: "Lorem ipsum", "Item 1", "Sample", "Demo User", "Test Data", or similar filler text.
- Use domain-accurate screen titles, button labels, helper text, alerts, and table headings.
- Keep wording concise, specific, and presentation-ready.

## Output — Project Initialization

Before creating components, initialize the Vite project:

```bash
npm create vite@latest apps/mock-ui -- --template react-ts
cd apps/mock-ui
npm install
npm install react-router-dom
npm install -D tailwindcss postcss autoprefixer
npm install tailwindcss@4
npm install -D shadcn-ui
```

Then populate the folder structure below with generated page components.

## Output — Folder Structure

After initialization, create or update `apps/mock-ui/` with this structure:

```
apps/mock-ui/
├── package.json (configured for port 5678)
├── vite.config.ts
├── tailwind.config.ts
├── postcss.config.js
├── tsconfig.json
├── tsconfig.app.json
├── index.html
├── src/
│   ├── main.tsx
│   ├── App.tsx (React Router setup)
│   ├── index.css (Tailwind 4 imports)
│   ├── pages/
│   │   ├── LoginPage.tsx
│   │   ├── DashboardPage.tsx
│   │   └── [other screens extracted from story]
│   ├── components/
│   │   └── [shadcn components used]
│   ├── lib/
│   │   └── utils.ts (shadcn utility)
│   ├── types/
│   │   └── index.ts (TypeScript interfaces for data models)
│   ├── mocks/
│   │   └── data.ts (realistic mock data generator)
│   └── hooks/
│       └── useData.ts (optional: hook for loading/filtering mock data)
├── public/
├── SCREENS.md (screen-to-AC mapping and demo scenarios)
└── README.md (how to run, port info, demo focus)
```

---

## Implementation Rules

### Vite Configuration

Update `vite.config.ts` to set port 5678:

```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5678,
  },
});
```

### Package.json

- Port: **5678** (configured in vite.config.ts above)
- Key dependencies: react, react-dom, vite, tailwindcss@4, shadcn-ui, react-router-dom
- Scripts (auto-created by vite create):
  ```json
  {
    "scripts": {
      "dev": "vite --port 5678",
      "build": "tsc && vite build",
      "preview": "vite preview"
    }
  }
  ```

### Tailwind Configuration with Theme Colors

Create or update `tailwind.config.ts` with the user-provided brand colors:

```typescript
import type { Config } from "tailwindcss";

export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: {
        primary: {
          50: "#f0f9ff",
          100: "#e0f2fe",
          200: "#bae6fd",
          300: "#7dd3fc",
          400: "#38bdf8",
          500: "#0ea5e9", // Replace with user's primary color
          600: "#0284c7",
          700: "#0369a1",
          800: "#075985",
          900: "#0c3d66",
        },
        secondary: {
          // Secondary color palette (if user provided one)
        },
        accent: {
          // Accent color palette (if user provided one)
        },
      },
    },
  },
  plugins: [],
} satisfies Config;
```

**Note:** Replace the primary color values with the user's chosen color. If user provided hex (e.g. `#0066CC`), generate a full color palette using that as the 500 shade. Use online color palette generators if needed.

### Shadcn/UI Component Theme

When installing shadcn/ui components, they will automatically inherit the theme from the `tailwind.config.ts` file. Use `className="bg-primary-500 text-white"` (or similar) to apply theme colors.

### Pages and Components

- **One file per screen** under `src/pages/`
- Include a dedicated `SitesPage.tsx` and route path `/sites` for site inventory and health overview.
- Use **shadcn/ui components** (Button, Card, Input, Select, Dialog, Table, etc.) via import
- Style with **Tailwind classes** only — no inline CSS
- Export each as a React component: `export default function LoginPage() { ... }`
- **Import mock data** from `src/mocks/data.ts` and render it on every screen
- Show realistic data volumes (e.g., 15–50 items in a list, not 1–2)

### Mock Login Flow (Required)

- Implement a **login screen that sets the active role and redirects to the app** — nothing more.
- Store `user` (name, role, scope) in React context so all screens can display it.
- **No route guards, no blocked pages.** Every route is freely accessible. The login screen is just a starting point for demos.
- Visiting `/login` while a role is already selected may redirect to `/dashboard`, but navigating directly to any route (e.g. `/sites`) always works.
- **Do not implement real authentication** (no OAuth/OIDC/SAML, no token exchange, no backend calls).
- Simulate a brief login delay (200–500 ms) so the button press feels real, then resolve from mock identity data.
- Persist the selected role in `localStorage` so page refresh keeps the demo running without re-login.
- Include at least one role-aware behavior in UI (e.g., an action disabled or hidden based on `user.role`).

### Mock Data Generation (src/mocks/data.ts)

Create a **data generator** that produces realistic sample data matching your product domain:

```typescript
// src/mocks/data.ts
export interface Product {
  id: string;
  name: string;
  status: "active" | "inactive" | "pending";
  createdAt: string;
  value: number;
  metadata: Record<string, any>;
}

export interface DashboardMetrics {
  totalCount: number;
  activeCount: number;
  completionRate: number;
  alertCount: number;
}

// Mock data functions
export const generateMockItems = (count: number): Product[] => {
  const statuses: Array<"active" | "inactive" | "pending"> = [
    "active",
    "inactive",
    "pending",
  ];
  return Array.from({ length: count }, (_, i) => ({
    id: `item-${i + 1}`,
    name: `Item ${i + 1}`,
    status: statuses[i % 3],
    createdAt: new Date(
      Date.now() - Math.random() * 90 * 24 * 60 * 60 * 1000,
    ).toISOString(),
    value: Math.floor(Math.random() * 10000),
    metadata: {
      category: `Category ${(i % 5) + 1}`,
      owner: `Owner ${(i % 8) + 1}`,
    },
  }));
};

export const mockItems = generateMockItems(30);
export const mockMetrics = {
  totalCount: 150,
  activeCount: 98,
  completionRate: 87.5,
  alertCount: 2,
};
```

**Key principles:**

- Generate **10–50+ records** per entity (not 1–2) so tables look realistic
- Use **realistic domain values** (actual solar capacity ranges, dates, numbers)
- Include **edge cases** (e.g., one panel with low efficiency to show alert)
- Store in a single `data.ts` file for easy modification during demo

### App.tsx with React Router

Set up React Router for screen navigation with a **product-like layout**. Avoid forcing a left demo sidebar unless user explicitly asks for it.

```tsx
import { BrowserRouter, Navigate, Route, Routes } from "react-router-dom";
import LoginPage from "./pages/LoginPage";
import DashboardPage from "./pages/DashboardPage";
import SitesPage from "./pages/SitesPage";
import { useAuth } from "./auth/useAuth";
import AppLayout from "./layouts/AppLayout";

// No route guards — all pages are freely accessible.
// Login is only a role-selector that redirects to /dashboard.
export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<LoginPage />} />
        <Route
          path="/*"
          element={
            <AppLayout>
              <Routes>
                <Route path="/dashboard" element={<DashboardPage />} />
                <Route path="/sites" element={<SitesPage />} />
                {/* Add other routes here */}
                <Route
                  path="*"
                  element={<Navigate to="/dashboard" replace />}
                />
              </Routes>
            </AppLayout>
          }
        />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## Output — SCREENS.md (if requested)

Create `apps/mock-ui/SCREENS.md`:

```markdown
# Product Demo Screens — [Feature Name]

## Screen Inventory

| Screen               | Purpose         | Demo Data                          | Interactive Features       | Status        |
| -------------------- | --------------- | ---------------------------------- | -------------------------- | ------------- |
| Dashboard            | Main overview   | 30+ items, key metrics             | Filter, search, drill-down | ✅ Demo-ready |
| Details/Analytics    | Deep-dive view  | Full context, history, comparables | Toggle filters, export     | ✅ Demo-ready |
| Notifications/Alerts | Status & health | Sample alerts with priority levels | Dismiss, categorize        | ✅ Demo-ready |
| [Screen]             | [Purpose]       | [Sample data]                      | [Demo features]            | ✅ Demo-ready |

---

## Screen Details & Demo Scenarios

### Dashboard

**Product Focus:** Give customers an at-a-glance view of key metrics
**Demo Data:**

- 30+ active items/records
- 3–5 top-level metrics (total count, active items, completion rate)
- 2–3 sample alerts or notifications

**Customer Talking Points During Demo:**

1. "See all your [domain objects] tracked in real-time"
2. "Metrics update when data changes"
3. "Status indicators show health at a glance"
4. "Click an item to see detailed information"

**Interactive Demo Flows:**

- Hover over metrics for tooltips
- Click an item to view detailed view
- Use filters or date range to drill down
- Interact with alerts or notifications

---

### Item Details / Deep Dive

**Product Focus:** Deep analysis and troubleshooting
**Demo Data:**

- Complete history or timeline for one item
- Related metadata and context
- Comparable items or benchmarks
- Historical actions or events

**Customer Talking Points:**

1. "Get detailed insights into individual items"
2. "See what's affecting performance or status"
3. "Compare with similar items"
4. "Historical data helps inform decisions"

---

## Next Steps After Demo

1. Show customer all screens live at http://localhost:5678
2. Gather feedback on features, data displayed, workflows
3. Iterate mock data or screens based on feedback
4. When approved, handoff to dev team with this as UI/UX reference
```

---

## README.md Content

Generate a README for `apps/mock-ui/README.md`:

````markdown
# Product Demo — [Feature Name]

**Purpose:** A fully interactive product demo with representative business data for customer presentations, sales, and user feedback.

## Features

- ✅ Complete product workflows with representative business data
- ✅ All screens populated with 15–50+ sample records per table
- ✅ Interactive elements: filters, searches, navigation
- ✅ Realistic domain data with customer-ready copy (no placeholder text)
- ✅ One-click to run, no setup required

## Quick Start

```bash
cd apps/mock-ui
npm install
npm run dev
```
````

Visit **http://localhost:5678** in your browser.

## Demo Walkthrough

### What's in the demo:

1. **Dashboard** — Overview with key metrics and sample data
2. **Details View** — Deep-dive with context and history
3. **Notifications** — Real-time alerts and status updates
4. **[Other screens]** — [Brief description]

### How to present:

1. Open the dashboard and show key metrics
2. Click an item to drill down into details
3. Show how to filter or change time ranges
4. Interact with alerts or notifications
5. Navigate between screens using the product header/tabs (or sidebar only if requested)

## Customizing Demo Data

Edit `src/mocks/data.ts` to change:

- Number of items or records
- Metric values and counts
- Status distributions
- Any domain-specific values

Refresh the browser to see changes immediately.

## Structure

- `src/pages/` — One screen per product feature
- `src/components/` — Reusable UI components (shadcn/ui)
- `src/mocks/data.ts` — All realistic sample data
- `SCREENS.md` — Demo scenarios and talking points
- `src/types/index.ts` — TypeScript interfaces for data models

## Next Steps

1. Demo with customer at http://localhost:5678
2. Gather feedback on features, UX, and data display
3. Iterate mock data based on feedback
4. Handoff to dev team with this as the UI/UX spec

## Tools

- **Vite** — Lightning-fast dev server
- **React** — UI framework
- **Tailwind CSS 4** — Modern styling
- **shadcn/ui** — Professional component library
- **React Router** — Screen navigation

```

---

## Save Output

**Path:** `apps/mock-ui/` (create or update entire folder)

After generation:
1. Confirm app structure is created
2. List **all generated screens**
3. Include a screen inventory table in the response with: `screen name`, `route path`, and `purpose`
4. If SCREENS.md was requested, confirm mapping file was created
5. Provide the command to run: `cd apps/mock-ui && npm install && npm run dev`

---

## After Generating

Tell the user:

> **Product Demo is ready for customer presentation!**
>
> 1. Run the demo: `cd apps/mock-ui && npm install && npm run dev`
> 2. Open http://localhost:5678 and walk through all screens
> 3. Use [SCREENS.md](./apps/mock-ui/SCREENS.md) for demo talking points and scripts
> 4. Customize mock data in `src/mocks/data.ts` as needed
> 5. Share with customer and gather feedback
> 6. When approved, dev team uses this as the UI/UX specification
>
> **Demo Tips:**
> - Show realistic data volumes (tables with 20+ rows)
> - Click through workflows to show interactivity
> - Use SCREENS.md talking points to align demo with customer goals
> - Mention: "All data is mockable — we can adjust numbers/metrics as you request"

---

## Rules

- ✅ **Always populate screens with 15–50+ mock records** — never show empty tables or "item 1, item 2"
- ✅ **Use realistic, domain-appropriate data** — match your actual product values and business logic
- ✅ **Show realistic behavior with simulated delays** — add brief loading states (200-500ms) to simulate real API calls, but resolve immediately from mock data
- ✅ **Store all mock data in `src/mocks/data.ts`** — easy to iterate during demos
- ✅ **Use customer-ready real text** — no placeholder/filler/mock-looking UI copy in any screen
- ✅ **Use shadcn/ui components only** — don't build custom components
- ✅ **Style with Tailwind 4 only** — no inline CSS or custom styles
- ✅ **Use React Router for navigation** — product-like navigation by default; sidebar only if explicitly requested
- ✅ **Login is a role-selector, not a gate** — selecting a role persists to localStorage and redirects to the app; no page blocking, no route guards; every URL is always accessible
- ✅ **Include dedicated `/sites` page** — product-like site map and navigation entry must exist
- ✅ **Include feature/AC mapping in SCREENS.md** — align demo talking points with requirements
- ✅ **Production-ready appearance** — every screen should impress customers
- ⚠️ Don't over-engineer — this is a demo, not a production backend
```
