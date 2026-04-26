---
name: execute-plan-step-by-step
description: "Execute an implementation plan one step at a time with user confirmation between steps. Attach an implementation plan file before running."
---

You are an expert software developer executing an implementation plan one step at a time, with careful validation and user control at each stage.

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

# Execute Implementation Plan Step-by-Step Prompt

## Task

Execute implementation plans one step at a time with user confirmation between steps. Focus on careful, validated progress with detailed logging.

## Prerequisites

**REQUIRED**: An implementation plan file must exist in `docs/technical-plans/{number}-plan-{name}.md` before proceeding.

---

## Core Workflow

### 1. Initialize Session

- Locate implementation plan in `docs/technical-plans/`
- Parse plan to identify all steps
- Create/update work log with session start
- Show plan overview and wait for user confirmation

### 2. Execute Single Step

- Execute ONLY the next pending step
- Validate step completion thoroughly
- Update work log with step results
- Stop and wait for user to run prompt again

### 3. Session Management

- Track current step in work log
- Resume from last completed step
- Handle step failures with clear recovery options

## Execution Flow

---

### Phase 1: Session Setup

```
🔍 Looking for implementation plan...
📋 Found: docs/technical-plans/{number}-plan-{name}.md
📊 Total steps: X
📝 Work log: docs/technical-plans/{number}-work-log.md

Current Status: Step X of Y
Next Step: [Step Title]

Ready to execute? Type 'yes' to proceed with this step only.
```

### Phase 2: Single Step Execution

```
🚀 Executing Step X: [Brief title]

⚙️  Implementation details:
- [What will be created/modified]
- [Key changes to make]

✅ Step X completed successfully!
📝 Updated work log

🔄 To continue: Re-run this prompt to execute Step X+1
```

### Phase 3: Step Validation

- Run validation commands specified in plan
- Verify expected outputs
- Update work log with validation results
- Prompt user for next step execution

### Phase 4: Final Validation (last step only)

When all steps are complete, **you MUST**:

1. Read every Acceptance Criteria (AC-001, AC-002, …) from the plan file
2. Run the corresponding validation command or check for each AC
3. Record results in a `**Validation Results:**` markdown table in the work log
4. Only mark the work log `Status: COMPLETED` after this table is written

```
**Validation Results:**

| AC     | Check                          | Result    |
| ------ | ------------------------------ | --------- |
| AC-001 | [description from plan]        | ✅ PASSED |
| AC-002 | [description from plan]        | ✅ PASSED |
| AC-00N | [description from plan]        | ❌ FAILED |
```

## Work Log Format

Create/update `docs/technical-plans/{number}-work-log.md`:

```markdown
# Work Log: [Feature Name]

**Plan:** {number}-plan-{name}.md
**Date Started:** [ISO Date]
**Last Updated:** [ISO Date]
**Status:** IN_PROGRESS
**Current Step:** X of Y

## Step Progress

- Step 1: [Title] - ✅ COMPLETED ([timestamp])
- Step 2: [Title] - ✅ COMPLETED ([timestamp])
- Step 3: [Title] - 🔄 IN_PROGRESS
- Step 4: [Title] - ⏸️ PENDING
- Step 5: [Title] - ⏸️ PENDING

## Current Step Details

**Step 3:** [Step Title]
**Started:** [timestamp]
**Goal:** [What this step accomplishes]
**Files Modified:** [List of files changed]
**Validation:** [Results of validation commands]

## Issues & Resolutions

(None yet)

## Next Action

Re-run execute-plan-step-by-step.prompt.md to continue with Step 4
```

When ALL steps are done, the final work log MUST also include:

```markdown
**Validation Results:**

| AC     | Check                          | Result    |
| ------ | ------------------------------ | --------- |
| AC-001 | [check description]            | ✅ PASSED |
| AC-002 | [check description]            | ✅ PASSED |
```

## Error Handling

### Missing Plan

```
❌ No implementation plan found
Expected: docs/technical-plans/{number}-plan-{name}.md
Please create implementation plan first using implement-plan-creation.prompt.md
```

### Step Failure

```
🛑 Step X failed: [Brief description]

❌ Issue: [Problem description]
🔧 Attempted: [What was tried]
📝 Logged to work log

🚀 Recovery Options:
1. Fix the issue manually and re-run this prompt
2. Skip this step (not recommended)
3. Restart from Step 1

Choose recovery option and re-run prompt.
```

### Validation Failure

```
⚠️  Step X completed but validation failed

✅ Implementation: Done
❌ Validation: Failed
📋 Expected: [What should happen]
