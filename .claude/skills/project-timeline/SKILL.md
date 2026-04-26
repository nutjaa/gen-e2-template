---
name: project-timeline
description: "Generate a Mermaid Gantt chart from a sprint plan. Attach the sprint plan file before running. Outputs a stakeholder-ready timeline that renders in GitHub, VS Code, and Notion."
---

You are an experienced delivery lead creating a stakeholder-facing project timeline.

## Your input

The sprint plan is provided as an attached context file (`docs/features/sprint-plan/sprint-plan-*.md`).
If no sprint plan is attached, ask the user to attach it before continuing.

---

## What to generate

Produce a **Mermaid Gantt chart** that visualises the sprint plan week by week.

Why Mermaid: it renders natively in GitHub, VS Code preview, and Notion — stakeholders can open it in the browser immediately with no tools required.

---

## Before generating — extract from the sprint plan

Read the attached sprint plan and extract:

1. **Project name**
2. **Total weeks** and **start date** — if no start date is in the plan, ask: _"What is the project start date? (e.g. 2026-03-03)"_
3. **Developer names and roles** (from the Team line)
4. **Every story** — its ID, assigned developer, size, week number, and title
5. **Any dependency notes** (⚠️ lines) — these become `after` relationships in Mermaid
6. **Milestones** — end of each week theme, and project completion

---

## Gantt chart rules

- **One section per week** — label it `Week N — Theme`
- **One task per story** — use the story ID as the task label
- **Duration from size:**
  - `[S]` = 1d
  - `[M]` = 2d
  - `[L]` = 3d
- **Tasks within a week run in parallel** unless a dependency note exists
- **If a dependency note exists** between two stories → use `after storyId` syntax
- Add a **milestone** at the end of each week: `Week N complete : milestone, ...`
- Add a **project complete** milestone at the very end
- Use `dateFormat YYYY-MM-DD` and `axisFormat %d %b`
- Weeks are Mon–Fri (5 working days). Calculate actual dates from the start date.

---

## Output format

Produce the full markdown file content in this structure:

````
# Project Timeline — [Project Name]

**Generated from:** [sprint plan filename]
**Start date:** [YYYY-MM-DD]
**End date:** [YYYY-MM-DD]
**Team:** [Alice (Frontend) · Bob (Backend) · Carol (SRE)]

---

```mermaid
gantt
    title [Project Name] — Delivery Timeline
    dateFormat YYYY-MM-DD
    axisFormat %d %b

    section Week 1 — Foundation & Setup
    STORY-001 Set up repo & deploy skeleton    :done, s001, 2026-03-03, 1d
    STORY-002 Configure CI/CD pipeline         :done, s002, 2026-03-03, 2d
    STORY-003 Scaffold app shell               :done, s003, 2026-03-03, 2d
    Week 1 complete                            :milestone, 2026-03-07, 0d

    section Week 2 — [Theme]
    STORY-004 [Title]                          :s004, 2026-03-10, 2d
    STORY-005 [Title]                          :s005, after s004, 2d
    Week 2 complete                            :milestone, 2026-03-14, 0d

    section Week N — Buffer & Polish
    STORY-XXX Bug fixes & UAT support          :sxxx, 2026-MM-DD, 1d
    Project complete                           :milestone, crit, 2026-MM-DD, 0d
```

---

## Legend

| Symbol | Meaning |
|---|---|
| `done` | Story completed |
| `crit` | Critical milestone |
| `milestone` | End of phase / week |
| `after storyId` | Dependency — cannot start until the referenced story is done |

---

## Stakeholder summary

**[Project Name]** is a [N]-week engagement starting [Start date] and completing by [End date].

| Developer | Role | Stories assigned |
|---|---|---|
| Alice | Frontend | STORY-001, STORY-003, ... |
| Bob | Backend | STORY-002, STORY-005, ... |
| Carol | SRE | STORY-004, ... |

**Key milestones:**

| Milestone | Target date |
|---|---|
| Foundation complete | [date] |
| Core features live on staging | [date] |
| UAT ready | [date] |
| **Project handover** | **[date]** |
````

---

## Save the output

Save the file at:

**Path:** `docs/features/sprint-plan/timeline-[project-name-slug].md`

- Use the same slug as the sprint plan file (e.g. `water-billing-system`)
- Create the directory if it does not exist
- If a file at that path already exists, ask the user before overwriting

Confirm the saved path to the user after writing.

---

## After generating

Tell the user:

> **How to view this timeline:**
>
> - **GitHub:** push to any branch — Mermaid renders automatically in the file preview
> - **VS Code:** open the file and use `Markdown: Open Preview` (or press `Ctrl+Shift+V`)
> - **Notion:** paste the mermaid code block into a Notion code block set to `mermaid`
> - **Mermaid Live Editor:** paste at [mermaid.live](https://mermaid.live) to share a live link with stakeholders
>
> To update the timeline after plan changes, re-attach the updated sprint plan and re-run this prompt.
