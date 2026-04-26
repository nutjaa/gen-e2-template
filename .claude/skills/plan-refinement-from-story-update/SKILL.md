---
name: plan-refinement-from-story-update
description: "Update an implementation plan when a user story is refined. Compares refined story vs existing plan, applies minimal step changes, and preserves in-progress execution by creating a new plan when work-log exists."
---

You are an experienced tech lead updating implementation plans after story refinement.

# Plan Refinement from Story Update Prompt

## Your inputs

Attach or provide all three:

1. **Refined story** from `docs/features/user-stories/` (e.g. `story-002-login-refined.md`)
2. **Current implementation plan** from `docs/technical-plans/` (e.g. `001-plan-login.md`)
3. **Optional execution context**: work log from `docs/technical-plans/` (e.g. `001-work-log.md`)

If refined story or plan is missing, ask user to provide both before continuing.

---

## Before updating — ask ONE question

Ask the user:

> I will check whether a work-log exists for the current plan.
>
> - If work-log exists (execution started), I will create a **new plan file** to preserve in-progress work.
> - If no work-log exists, I will **update the current plan in place**.
>
> **Proceed with this approach?**

Wait for confirmation before making changes.

---

## Task

Compare the refined story against the current implementation plan and update the plan with minimal, traceable changes.

### Auto-detection logic

1. Detect the plan number and story reference from inputs.
2. Check for matching work-log in `docs/technical-plans/`:
   - Primary: `{plan-number}-work-log.md`
   - Fallback: any work-log explicitly referencing the plan filename
3. If work-log found -> create a new plan file (do not overwrite active plan).
4. If no work-log found -> update plan in place.

### Comparison logic

Map story changes to plan impact:

- **AC unchanged** -> keep related plan steps
- **AC clarified** -> update wording/verification only
- **AC added** -> add new step(s)
- **AC removed** -> mark corresponding step as removed or out-of-scope
- **Flow changed** -> reorder affected steps and dependencies

Use smallest possible edits. Avoid rewriting unaffected sections.

---

## Output Format

Produce an updated plan in this structure:

```markdown
# Implementation Plan: [Feature Name]

## Overview

**User Story:** [Refined story path]
**Previous Plan:** [Old plan path]
**Plan Status:** Refined from story update — [date]

## Change Impact Summary

| Area | Previous | Updated | Impact |
|---|---|---|---|
| Acceptance Criteria | [count/list] | [count/list] | [Low/Med/High] |
| Step Count | [N] | [N] | [delta] |
| Dependencies | [summary] | [summary] | [changes] |
| Verification | [summary] | [summary] | [changes] |

## Implementation Steps

### Step 1: [Title] ⏱️ [Estimate]

**Goal:** [Goal]
**Depends on:** [None or step]
**Verification:** [How to verify]

#### Implementation Details:

- **Files to modify:** [paths]
- **Key changes:** [brief]
- **Methods/Functions:** [names]

#### Validation Commands:

```bash
[commands]
```

[Continue with updated + unchanged steps]

## Final Verification

### Acceptance Criteria Checklist

- [ ] [AC-001 text]
- [ ] [AC-002 text]
- [ ] [AC-XXX text]

### Integration Testing

```bash
[project-relevant commands]
```

### Manual Testing Steps

1. [step]
2. [step]

## Notes & Considerations

- [Scope or dependency notes]
- [Assumptions]
- [Known risks]
```

---

## Save Output

### Case A: Work-log exists (execution started)

Create a new plan file in `docs/technical-plans/`:

- Find highest numeric prefix among `*-plan-*.md`
- Save as: `docs/technical-plans/{N+1}-plan-{feature-name}-refined.md`
- Add references in Overview:
  - `Previous Plan: ...`
  - `Refined from story: ...`
- Keep old plan unchanged

### Case B: No work-log (planning phase)

Update current plan in place:

- Path: existing plan path
- Add `Plan Status: Refined from story update — [date]`
- Update only impacted sections

Always confirm saved path to the user.

---

## Output — Plan Refinement Summary

After saving, print:

```markdown
## Plan Refinement Summary

**Refined story:** [path]
**Previous plan:** [path]
**Updated plan:** [path]
**Work-log detected:** [Yes/No]

### Step-level changes

| Type | Count | Notes |
|---|---|---|
| Kept | [n] | Unchanged from previous plan |
| Updated | [n] | Verification/flow wording refined |
| Added | [n] | New AC coverage |
| Removed | [n] | Out-of-scope or obsolete |

### AC coverage

- AC kept: [list]
- AC added: [list]
- AC removed: [list]

### Recommendation

- [If new plan created] Execute new plan file and keep old work-log for audit.
- [If in-place update] Safe to continue planning/execution from updated file.
```

---

## Rules

- Never overwrite an active plan when work-log exists.
- Prefer minimal diffs: keep unaffected steps unchanged.
- Preserve traceability: every AC in refined story must map to at least one plan step.
- Keep step size practical (15-60 minutes each) unless existing plan style differs.
- Use project-appropriate validation commands (do not hardcode a tech stack).
- If refined story introduces major scope expansion, recommend splitting into multiple plans.
