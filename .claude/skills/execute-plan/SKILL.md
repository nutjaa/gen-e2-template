---
name: execute-plan
description: "Execute an implementation plan continuously from start to finish, all steps in sequence without manual intervention. Attach an implementation plan file before running."
---

You are an expert software developer executing an implementation plan autonomously, completing all steps in sequence with validation at each stage.

## Your input

Attach the following before running:

- An implementation plan file from `docs/technical-plans/` (e.g. `001-plan-feature-name.md`)

If no plan file is attached, respond with:

```
❌ No implementation plan found.
Expected: docs/technical-plans/{number}-plan-{name}.md
Please create an implementation plan first using the implement-plan-creation prompt.
```

---

# Execute Implementation Plan Prompt

## Task

Execute the entire implementation plan continuously from start to finish. All steps are executed in sequence without user confirmation between steps.

## Prerequisites

**REQUIRED**: An implementation plan file must exist in `docs/technical-plans/{number}-plan-{name}.md` before proceeding.

---

## Core Workflow

### 1. Initialize Session

- Locate implementation plan in `docs/technical-plans/`
- Parse plan to identify all steps
- Create/update work log with session start
- Begin execution immediately

### 2. Execute All Steps Continuously

- Execute each step in sequence
- Validate step completion after each step
- Update work log with step results
- Continue to next step automatically
- Only stop if critical error occurs

### 3. Session Management

- Track progress in work log as execution proceeds
- Handle step failures with automatic retry or recovery
- Complete all steps until plan is fully executed

## Execution Flow

---

### Phase 1: Session Setup

```
🔍 Looking for implementation plan...
📋 Found: docs/technical-plans/{number}-plan-{name}.md
📊 Total steps: X
📝 Work log: docs/technical-plans/{number}-work-log.md

🚀 Starting continuous execution of all steps...
```

### Phase 2: Continuous Execution

```
🚀 Executing Step 1: [Brief title]
⚙️  [What will be created/modified]
✅ Step 1 completed

🚀 Executing Step 2: [Brief title]
⚙️  [What will be created/modified]
✅ Step 2 completed

🚀 Executing Step 3: [Brief title]
⚙️  [What will be created/modified]
✅ Step 3 completed

[Continue for all remaining steps...]
```

### Phase 3: Final Validation

- Run all validation commands from plan
- Verify expected outputs across all steps
- Update work log with final status
- Report completion summary

## Work Log Format

Create/update `docs/technical-plans/{number}-work-log.md`:

```markdown
# Work Log: [Feature Name]

**Plan:** {number}-plan-{name}.md
**Date Started:** [ISO Date]
**Completed:** [ISO Date]
**Status:** COMPLETED
**Execution Type:** Continuous (All Steps)

## Step Progress

- Step 1: [Title] - ✅ COMPLETED ([timestamp])
- Step 2: [Title] - ✅ COMPLETED ([timestamp])
- Step 3: [Title] - ✅ COMPLETED ([timestamp])
- Step 4: [Title] - ✅ COMPLETED ([timestamp])
- Step 5: [Title] - ✅ COMPLETED ([timestamp])

## Step Details

### Step 1: [Step Title]

**Started:** [timestamp]
**Completed:** [timestamp]
**Files Modified:** [List of files changed]
**Validation:** [Results]

### Step 2: [Step Title]

**Started:** [timestamp]
**Completed:** [timestamp]
**Files Modified:** [List of files changed]
**Validation:** [Results]

[Continue for all steps...]

## Issues & Resolutions

[Any issues encountered and how they were resolved during execution]

## Final Validation

**Status:** ✅ All validation checks passed
**Timestamp:** [ISO timestamp]
**Results:** [Summary of validation results]

## Summary

- Total Steps: X
- All Completed: ✅
- Duration: [calculated from timestamps]
- Final Status: Ready for testing
```

## Error Handling

### Missing Plan

```
❌ No implementation plan found
Expected: docs/technical-plans/{number}-plan-{name}.md
Please create implementation plan first using implement-plan-creation.prompt.md
```

### Critical Step Failure

```
🛑 Critical failure at Step X: [Brief description]

❌ Issue: [Problem description]
🔧 Attempted: [What was tried]
📝 Steps completed before failure: X/Y

✅ Completed:
- Step 1: [Title]
- Step 2: [Title]
[...steps completed successfully]

❌ Failed at Step X: [Title]
[Detailed error information]

⏸️  Remaining steps not executed:
- Step X+1: [Title]
- Step X+2: [Title]
[...remaining steps]

🚀 Recovery Options:
1. Fix the issue and re-run this prompt (will resume from failed step)
2. Use execute-plan-step-by-step.prompt.md to continue carefully
3. Review and update the implementation plan
```

### Validation Failure

```
⚠️  All steps completed but final validation failed

✅ Implementation: All X steps completed
❌ Validation: Failed
