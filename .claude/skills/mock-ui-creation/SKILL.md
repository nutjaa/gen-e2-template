---
name: mock-ui-creation
description: "Generate a runnable mock UI from customer requirements or user stories. Creates a Vite + React + Tailwind 4 + shadcn + React Router app at apps/mock-ui/ on port 5678."
---

You are an experienced UI/UX designer and React developer creating interactive mock UIs for customer validation and handoff.

# Mock UI Generation Prompt

## Your inputs

Attach one of the following before running:

- A user story file from `docs/features/user-stories/` (e.g. `story-001-feature-name.md`)
- Customer requirements or design brief (paste or attach)
- Feature specification with wireframes or screen descriptions

If no context is provided, ask the user to attach a story or requirements before continuing.

---

## Before generating — ask TWO questions

Ask the user both questions at once:

> **1. Should we generate a SCREENS.md mapping file?**  
> (This maps each screen to Acceptance Criteria for later validation. Default: yes)
>
> **2. What is the primary brand color for this UI?**  
> (e.g. `#0066CC`, `blue`, `#FF6B35`, or describe: "brand purple", "dark green")
> 
> Optional: Also specify secondary color, accent color, or "dark mode preferred" if desired.
>
> The screens will be auto-detected from the attached user story or requirements context.

Wait for answers before generating.

---

## Task

Analyze the provided user story or requirements to extract all screens/pages needed. Then generate a fully functional Vite + React + Tailwind 4 + shadcn + React Router UI mock app that runs immediately.

**Screen detection:**
- Read acceptance criteria to identify user-facing screens/pages
- Extract screen names from AC descriptions (e.g. AC-001 mentions "login page" → create LoginPage)
- If unclear, infer from feature context

---

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
│   └── types/
│       └── index.ts (TypeScript interfaces)
├── public/
└── README.md (how to run, port info)
```

---

## Implementation Rules

### Vite Configuration

Update `vite.config.ts` to set port 5678:

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5678,
  },
})
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
import type { Config } from 'tailwindcss'

export default {
  content: [
    './index.html',
    './src/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          200: '#bae6fd',
          300: '#7dd3fc',
          400: '#38bdf8',
          500: '#0ea5e9',  // Replace with user's primary color
          600: '#0284c7',
          700: '#0369a1',
          800: '#075985',
          900: '#0c3d66',
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
} satisfies Config
```

**Note:** Replace the primary color values with the user's chosen color. If user provided hex (e.g. `#0066CC`), generate a full color palette using that as the 500 shade. Use online color palette generators if needed.

### Shadcn/UI Component Theme

When installing shadcn/ui components, they will automatically inherit the theme from the `tailwind.config.ts` file. Use `className="bg-primary-500 text-white"` (or similar) to apply theme colors.

### Pages and Components

- **One file per screen** under `src/pages/`
- Use **shadcn/ui components** (Button, Card, Input, Select, Dialog, etc.) via import
- Style with **Tailwind classes** only — no inline CSS
- Export each as a React component: `export default function LoginPage() { ... }`

### App.tsx with React Router

Set up React Router for screen navigation:

```tsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';
import LoginPage from './pages/LoginPage';
import DashboardPage from './pages/DashboardPage';
// Import other pages as needed

const routes = [
  { path: '/', label: 'Login', element: <LoginPage /> },
  { path: '/dashboard', label: 'Dashboard', element: <DashboardPage /> },
  // Add other routes here based on extracted screens
];

export default function App() {
  return (
    <BrowserRouter>
      <div className="flex h-screen">
        <nav className="w-48 bg-slate-900 text-white p-4 overflow-y-auto">
          <h2 className="text-lg font-bold mb-4">Screens</h2>
          {routes.map((route) => (
            <Link
              key={route.path}
              to={route.path}
              className="block py-2 px-4 rounded mb-2 hover:bg-slate-700 transition"
            >
              {route.label}
            </Link>
          ))}
        </nav>
        <main className="flex-1 bg-white p-8 overflow-y-auto">
          <Routes>
            {routes.map((route) => (
              <Route key={route.path} path={route.path} element={route.element} />
            ))}
          </Routes>
        </main>
      </div>
    </BrowserRouter>
  );
}
```

---

## Output — SCREENS.md (if requested)

Create `apps/mock-ui/SCREENS.md`:

```markdown
# Mock UI Screens — [Feature Name]

## Screen Inventory

| Screen | Purpose | AC Mapping | Status |
|--------|---------|-----------|--------|
| Login | User authentication | STORY-001 AC-001, AC-002 | ✅ Ready for review |
| Dashboard | Main user view | STORY-001 AC-003, AC-004 | ✅ Ready for review |
| [Screen] | [Purpose] | STORY-XXX AC-YYY | ✅ Ready for review |

---

## Screen Details

### Login
**User Story:** [Reference to docs/features/user-stories/story-XXX-...]
**Acceptance Criteria:**
- AC-001: User enters email and password
- AC-002: Form validates before submission
- AC-003: Error message shown on invalid credentials

**Mockable flows:**
- Valid login → success screen
- Invalid credentials → error message
- Empty fields → validation error

---

### Dashboard
**User Story:** [Reference]
**Acceptance Criteria:**
- AC-003: User sees dashboard after login
- AC-004: Dashboard displays user data
- AC-005: User can logout

**Mockable flows:**
- Load dashboard with sample data
- Click logout → return to login

---

## Future Enhancements

- [ ] Add API endpoint simulation (mock server)
- [ ] Add form state management (Zustand, Redux)
- [ ] Generate test scenarios per AC
- [ ] Export as Figma/design handoff
```

---

## README.md Content

Generate a README for `apps/mock-ui/README.md`:

```markdown
# Mock UI — [Feature Name]

**Purpose:** Interactive prototype for customer validation before development.

## Quick Start

```bash
cd apps/mock-ui
npm install
npm run dev
```

Visit **http://localhost:5678** in your browser.

## Structure

- `src/pages/` — One screen component per user story screen
- `src/components/` — Reusable shadcn/ui components
- `SCREENS.md` — Maps screens to acceptance criteria

## Acceptance Criteria Mapping

See the generated `SCREENS.md` file for screen-to-AC mapping.

## Next Steps

1. Share with customer for feedback
2. Iterate screens based on feedback
3. Handoff SCREENS.md to dev team
4. Dev team uses this as UI/UX reference during implementation

## Tools

- **Vite** — Fast dev server
- **React** — UI framework
- **Tailwind CSS** — Utility-first styling
- **shadcn/ui** — High-quality component library
```

---

## Save Output

**Path:** `apps/mock-ui/` (create or update entire folder)

After generation:
1. Confirm app structure is created
2. List generated screens
3. If SCREENS.md was requested, confirm mapping file was created
4. Provide the command to run: `cd apps/mock-ui && npm install && npm run dev`

---

## After Generating

Tell the user:

> **Mock UI is ready!**
>
> 1. Run the app locally: `cd apps/mock-ui && npm install && npm run dev`
> 2. Open http://localhost:5678 and test all screens
> 3. Share the mockable flows with the customer for feedback
> 4. When approved, this becomes the UI/UX spec for development
> 5. See [SCREENS.md](./apps/mock-ui/SCREENS.md) for AC mapping (if generated)
>
> **Next:** Run the dev server and validate with your customer.

---

## Rules

- Always generate runnable code — dev can test immediately
- Use only shadcn/ui components; do not build custom components from scratch
- Keep screens focused and minimal — mock, don't over-engineer
- Map every screen to at least one AC for traceability
- Extract screens automatically from user story AC descriptions; do not ask "which screens"
- Use Tailwind 4 for all styling
- Use React Router for navigation between screens
- Initialize project with `npm create vite@latest apps/mock-ui -- --template react-ts` first
