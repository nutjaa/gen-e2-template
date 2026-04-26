---
name: sprint-stakeholder-report
description: "Generate a stakeholder sprint report from a folder of completed user stories and work logs. Attach the sprint folder (e.g. docs/features/user-stories/week1/) before running."
---

You are a delivery lead writing a sprint completion report for non-technical stakeholders and product owners.

# Sprint Stakeholder Report Prompt

## Your input

Attach the following before running:

- A folder of completed sprint stories — e.g. `docs/features/user-stories/week1/`

The folder must contain at least one `*-story-*.md` file. Work logs (`*-work-log.md`) and plan files (`*-plan-*.md`) are used as supplementary evidence where present.

If nothing is attached, ask the user to attach the sprint folder before continuing.

**Also run the following git command** to collect unplanned work committed during the sprint period:

```bash
git log --oneline --since="7 days ago"
```

If the sprint date range is known, prefer the exact range:

```bash
git log --oneline --after="YYYY-MM-DD" --before="YYYY-MM-DD"
```

---

## Before generating — ask ONE question (if not already clear)

> **What is the sprint name or date range?**
>
> Used as the report title (e.g. "Week 1 — March 24–28 2026").
> Default: infer from folder name and work-log dates if available.

---

## How to read the folder

For every `*-story-*.md` file in the attached folder:

1. **Read the Story Overview block** — extract: Feature Name, Epic, Priority, Story Points
2. **Read the User Story statement** — extract the user role and the business value (the "So that" clause)
3. **Check the matching work log** (`same-number-work-log.md`) — extract:
   - Overall status (`COMPLETED` / `IN PROGRESS` / `BLOCKED`)
   - Step count and how many are ticked ✅
   - Date Started / Last Updated
   - Any Issues & Resolutions entries
4. **Scan Acceptance Criteria** — count total ACs; note any that have explicit failure notes in the work log
5. **Ignore plan files** (`*-plan-*.md`) — they are implementation detail, not stakeholder-facing

If no work log exists for a story, mark the story status as **No Work Log**.

---

## How to read the git log (unplanned work)

From the git log output, identify **hotfix / unplanned commits** — commits that are **not** linked to a story number.

A commit is considered **story-linked** if its message contains a story number pattern such as `040`, `041`, `042` … (3-digit prefix matching the story files in the sprint folder), or keywords like `story`, `feat:`, `feature`.

A commit is considered **unplanned / hotfix** if its message:

- starts with `fix:`, `hotfix`, `patch`, `bugfix`, or `chore:` **and** has no story number, OR
- has no story number reference at all and does not look like a merge or version bump commit

For each unplanned commit, record:

- Short commit hash
- Commit message (translated to plain English where possible)
- Whether it looks like a **hotfix** (production issue), **config change**, **dependency update**, or **other**

If there are no unplanned commits, omit the Unplanned Work section entirely.

---

## Task

Generate a **short** sprint completion report for non-technical stakeholders (product owner, client, agency lead). Target length: fits on one printed page.

- One-paragraph executive summary, then a delivery table, then next steps — that's it
- No per-story prose sections; the table is the only feature list
- Translate technical names into plain English outcomes
- Never use internal jargon (no "TypeScript", "MongoDB", "Bun", "Elysia", "bcrypt", etc.)
- Omit any optional section when it has nothing to say

---

## Output Format

```markdown
# Sprint Report: [Sprint Name / Date Range]

**Generated:** [today's date]
**Stories Delivered:** [count of COMPLETED] of [total]
**Story Points:** [delivered] of [total]

---

## Summary

[2–3 sentences max. What can the product do now that it couldn't before? One sentence on anything not delivered.]

---

## Delivered This Sprint

| #   | Feature                   | Business Value                         | Points | Status         |
| --- | ------------------------- | -------------------------------------- | ------ | -------------- |
| NNN | [Feature Name from story] | [Plain-English outcome from the story] | [SP]   | ✅ Done        |
| NNN | ...                       | ...                                    | ...    | 🔄 In Progress |
| NNN | ...                       | ...                                    | ...    | ⚠️ Blocked     |

---

## Carried Forward

[Only include if stories are incomplete. Skip entirely if all done.]

| #   | Feature        | Progress            | Notes               |
| --- | -------------- | ------------------- | ------------------- |
| NNN | [Feature Name] | [X of Y steps done] | [Reason or blocker] |

---

## Unplanned Work & Hotfixes

[Only include if unplanned commits were found. Skip entirely if none.]

| Commit   | Description                 | Type                                             |
| -------- | --------------------------- | ------------------------------------------------ |
| `abc123` | [Plain-English description] | 🔥 Hotfix / 🔧 Config / 📦 Dependency / 🔨 Other |

> These changes were not tracked in the sprint backlog. They are included here for full transparency.

---

## Issues

[Only include if work logs contain real blockers. Skip entirely if none.]

| Story | Issue | Resolution |
| ----- | ----- | ---------- |

---

## Quality

**Acceptance criteria covered:** [total AC count] across [N] stories — all passed. _(or note any deferrals)_

---

## Next Sprint

- [Action 1]
- [Action 2]
```

---

## Report Writing Rules

- **Length:** Aim for one printed page. If it's longer, cut it.
- **No per-story prose.** The delivery table is the only feature list — no subsections, no bullet lists of capabilities.
- **Tone:** Present tense, plain English. No code terms. No acronyms without expansion.
- **Status mapping:** COMPLETED → ✅ Done · IN PROGRESS → 🔄 In Progress · BLOCKED → ⚠️ Blocked · No Work Log → ❓ Unknown
- **Points:** Sum only ✅ Done stories. Show `delivered / total`.
- **Summary:** 2–3 sentences. What the product can do now. One mention (no blame) if anything slipped.
- **Carried Forward / Issues:** Omit entirely when empty.
- **Unplanned Work:** Omit entirely when no hotfix/unplanned commits exist. When present, keep descriptions plain English — no commit hashes or branch names in the prose summary.
- **Next Sprint:** At least one action item; lead with carried-forward stories if any.

---

## Save the output

**Path:** `docs/sprint-plan/sprint-report-{sprint-slug}.md`

- `{sprint-slug}` = kebab-case version of the sprint name (e.g. `week-1`, `sprint-3-april-2026`)
- If a file at that path already exists, ask the user before overwriting

Confirm the saved path to the user after writing.

---

## After generating

Tell the user:

> **Next steps:**
>
> 1. Review the Executive Summary — ensure it accurately reflects what stakeholders need to know
> 2. Share with your product owner or client for sprint sign-off
> 3. Use the **Carried Forward** section to seed the next sprint planning session
> 4. Archive this report alongside the sprint folder for future reference
