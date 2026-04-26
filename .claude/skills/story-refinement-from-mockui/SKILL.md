---
name: story-refinement-from-mockui
description: "Refine a user story based on mock UI feedback and validated screens. Compares original AC with actual UI behavior, updates acceptance criteria, and saves the refined story."
---

You are an experienced product manager analyzing mock UI feedback to refine user stories and acceptance criteria.

# Story Refinement from Mock UI Prompt

## Your inputs

Attach or provide all three:

1. **Original user story** from `docs/features/user-stories/` (e.g. `story-001-feature-name.md`)
2. **Mock UI code/folder** at `apps/mock-ui/` OR screenshots/demo notes
3. **Feedback notes** (optional) — what changed, what users/stakeholders said, what was validated

If any of these are missing, ask the user to provide them before continuing.

---

## Before refining — ask ONE question

Ask the user:

> **After checking for active work-log:** If a work-log exists (meaning dev team has already started), we'll create a new numbered story to preserve their original commitment. If no work-log, we'll update the story in place.
>
> **Do you want me to proceed with this approach, or would you prefer something different?**

Wait for confirmation before refining.

---

## Task

Compare the original user story acceptance criteria against what the mock UI actually demonstrates, then produce a refined story that reflects validated design decisions and feedback.

**Auto-detection logic:**
1. Check if a work-log exists: `docs/technical-plans/[story-number]-work-log.md`
2. If work-log found → dev work has started → **create new numbered story** (next available number)
3. If NO work-log → still in design phase → **update original story in place**
4. Always preserve original story with clear reference

**Analysis steps:**

1. **Read the original story** — capture all AC-XXX criteria and scope
2. **Review the mock UI** — understand what screens exist, what flows work, what's interactive
3. **Identify deltas:**
   - AC that is now clearer based on UI mockup
   - AC that needs reordering or rephrasing for clarity
   - AC that was removed or combined (scope narrowing)
   - New AC discovered during feedback (scope expansion)
   - Error states, edge cases, or validations shown in UI
4. **Document feedback impact** — what changed and why
5. **Generate refined AC** — same BDD Given/When/Then format, but updated for reality

---

## Output Format

### Case 1: Work-log exists (create new numbered story)

Produce a new story as `docs/features/user-stories/story-[NEXT-NUMBER]-[feature-name]-refined.md`

Example: Original `story-001-login.md` + work-log found → Create `story-002-login-refined.md`

### Case 2: No work-log (update in place)

Update the original story in place: `docs/features/user-stories/story-[NUMBER]-[feature-name].md`

Add a refinement timestamp and change log section.

---

## Story Template

Both cases use this structure:

```markdown
# User Story: [Feature Name]

## Story Overview

**Feature Name:** [Same or refined]
**Epic:** [Same or updated]
**Priority:** [Same or adjusted based on feedback]
**Story Points:** [Same or re-estimated if scope changed]
**Status:** Refined from mock UI feedback — [date]

## 📌 REFINEMENT REFERENCE (if new story)

**Refined from:** story-001-login.md
**Reason:** Mock UI feedback + customer validation
**Original story:** [Link to original for dev team reference]
**Work preserved in:** story-001 (original commitment remains)

---

## ⚠️ REFINEMENT NOTES

**Changes from original:**
- [What was clarified by mock UI testing]
- [What was added/removed/reordered]
- [Scope changes, if any]

**Feedback source:**
- [Who validated: customer, stakeholder, internal team, etc.]
- [Key insights from testing]
- [Decisions made based on UI prototype]

---

## User Story

**As a** [user role/persona],
**I want to** [capability/functionality — refined if needed],
**So that** [business value/benefit — same or clarified].

## Description

[Updated 2-3 sentences based on mock UI learnings. Include integration points, technical considerations, and any scope clarifications.]

## Pre-conditions

- [ ] [Condition 1]
- [ ] [Condition 2 — updated if mock UI revealed new dependencies]
- [ ] [Additional conditions]

## Acceptance Criteria

> AC IDs are stable identifiers for traceability.
> 
> **Changes from original:**  
> - Removed AC-XXX (reason: out of scope / covered by other AC / not validated)
> - Updated AC-YYY (reason: UI testing showed different flow)
> - Added AC-ZZZ (reason: discovered during feedback)

### AC-001: [Functional Area 1 — potentially reworded from original]

**Given** [the starting context or system state]  
**When** [the user performs an action]  
**Then** [the observable outcome]  
**And** [additional outcome if needed] _(optional)_

### AC-002: [Functional Area 2 — potentially reordered or refined]

**Given** [the starting context or system state]  
**When** [the user performs an action]  
**Then** [the observable outcome]

### AC-003: [Error / Edge Case — potentially reworded based on UI validation]

**Given** [invalid input or failure condition]  
**When** [the user attempts the action]  
**Then** [the system responds with error/fallback — as shown in mock UI]

---

## Notes & Considerations

- [Technical decisions confirmed/changed by mock UI]
- [Scope refinements and rationale]
- [Open questions or future enhancements]
- [Handoff notes for development team]
```

---

## Save Output

**Auto-detect logic determines path:**

**If work-log exists:**
- Find the highest story number in `docs/features/user-stories/`
- Create new story: `docs/features/user-stories/story-[N+1]-[feature-name]-refined.md`
- Add reference back to original (story number, link to original file)
- Confirm: "Work-log found. Created new story story-002 to preserve original commitment."

**If NO work-log:**
- Update original in place: `docs/features/user-stories/story-[N]-[feature-name].md`
- Add refinement timestamp and change log
- Note: "No active work-log. Updated story-001 in place (planning phase)."

---

## Refinement Rules

- **Auto-detect work-log first** — if exists, always create new story; never overwrite active work
- **Never remove AC without explaining why** — add a note like "(Removed: redundant with AC-003, covered by UI validation)"
- **Preserve original AC IDs where possible** — AC-001 stays AC-001; only reorder if essential
- **Add new AC at the end** — don't renumber the entire list
- **Quote exact UI behavior** — "user sees error toast 'Email already exists'" not "error is shown"
- **Reference mock UI directly** — "As shown in LoginPage.tsx mock, user can edit profile inline"
- **Include scope decisions** — "Out of scope per feedback: export to PDF (future story)"
- **Validate all AC** — every AC must be visually or logically confirmed by the mock UI
- **If creating new story, reference the original** — Dev team needs to know what commitment they're replacing or informing

---

## Output — Refinement Summary

After saving, print:

```markdown
## Refinement Summary

**Original story:** [Original file path + date created]
**Refined story:** [New/updated file path + date refined]

### Changes Summary

| Aspect | Before | After | Reason |
|--------|--------|-------|--------|
| AC count | 3 | 4 | Added error handling AC discovered in UI testing |
| Scope | [original] | [refined] | [scope change or clarification] |
| Priority | High | High | [same or updated] |
| Story Points | 5 | 5 | [same or re-estimated] |

### AC Changes

- ✅ AC-001: Kept (clarified wording)
- ✅ AC-002: Kept (reordered, no logic change)
- ✨ AC-003: Added (new error state found in UI)
- ✅ AC-004: Kept (now AC-003 after reorder)
- ❌ AC-004 (original): Removed (out of scope per feedback)

---

## Next Steps

1. Dev team reviews refined story and SCREENS.md mapping
2. Update implementation plan if scope changed
3. If new stories added, generate stories for those separately
```

---

## After Refining

Tell the user (tailored to outcome):

**If work-log was found (new story created):**

> **Story refined and ready!**
>
> 1. Original story preserved: `story-001-login.md` (dev team's original commitment)
> 2. New refined story created: `story-002-login-refined.md`
> 3. New story references original for context
> 4. Changes documented in "Refinement Reference" section
> 5. Dev team can now plan/execute story-002, or review feedback and decide
>
> **Next:** Share new story + SCREENS.md with dev team for planning, or iterate further if feedback continues.

**If no work-log (story updated in place):**

> **Story refined and ready!**
>
> 1. Original story updated: `story-001-login.md`
> 2. Refinement timestamp added
> 3. Changes documented in "Refinement Notes" section
> 4. Ready for dev team to execute
>
> **Next:** Create implementation plan or begin development based on refined story.

---

## Rules

- Always compare original AC vs. mock UI side-by-side before writing refined AC
- Never assume — ask for clarification if feedback is vague or contradicts AC
- Keep Given/When/Then format strict — all AC must be executable
- If scope expanded significantly, consider breaking into separate stories instead of bloating one
- Archive or date-stamp original story for audit trail
- Reference mock UI file paths in AC text for developer clarity
