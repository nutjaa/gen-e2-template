---
name: implement-plan-creation
description: "Generate a step-by-step implementation plan from a user story with AC-linked test cases. Attach a user story file (docs/features/user-stories/*.md) before running."
---

You are an experienced software architect and tech lead breaking down user stories into actionable implementation plans for developers.

# Implementation Plan Generation Prompt

## Your input

Attach the following before running:

- A user story file from `docs/features/user-stories/` (e.g. `story-001-feature-name.md`)

If no file is attached, ask the user to attach the user story before continuing.

---

## Task

Generate a concise, step-by-step implementation plan based on a provided user story. The plan should break down the development work into verifiable steps with focused implementation guidance.

Include explicit test-case design mapped to Acceptance Criteria so QA and engineering can validate behavior consistently.

---

## Prerequisites

**CRITICAL**: A user story file must be provided in the context before proceeding. If no user story is available, respond with:

```
❌ ERROR: No user story provided. Please include a user story file (docs/features/user-stories/story-{number}-{story-name}.md) in the context before generating an implementation plan.
```

**CRITICAL**: The user story must contain Acceptance Criteria (AC-001, AC-002, ...). If no Acceptance Criteria are present, respond with:

```
❌ ERROR: The provided user story has no Acceptance Criteria. Acceptance Criteria are required before generating an implementation plan.
```

## Context Input

You will receive:

- A user story markdown file from `docs/features/user-stories/`
- The user story's acceptance criteria and technical requirements
- Any existing project structure or constraints

---

## Output Format

Generate an implementation plan following this structure and save it as `docs/technical-plans/{number}-plan-{story-name}.md`:

````markdown
# Implementation Plan: [Feature Name from User Story]

## Overview

**User Story:** [Reference to the source user story file]
**Estimated Time:** [Total development time estimate]
**Prerequisites:** [Any setup requirements before starting]

## Implementation Steps

### Step 1: [Step Title] ⏱️ [Time Estimate]

**Goal:** [What this step accomplishes]
**Depends on:** None
**Verification:** [How to verify this step is complete]

#### Implementation Details:

- **Files to modify:** `path/to/file1.ext`, `path/to/file2.ext`
- **Key changes:** [Brief description of main changes needed]
- **Methods/Functions:** [List of main methods to implement/modify]

#### Validation Commands:

```bash
[Commands to test this step]
```
````

---

### Step 2: [Step Title] ⏱️ [Time Estimate]

**Goal:** [What this step accomplishes]
**Depends on:** [Step N must be completed first, or "None"]
**Verification:** [How to verify this step is complete]

#### Implementation Details:

- **Files to create:** `path/to/new-file.ext`
- **Key changes:** [Brief description of implementation approach]
- **Methods/Functions:** [List of main methods to implement]

#### Validation Commands:

```bash
[Commands to test this step]
```

---

[Continue for all steps...]

## Test Cases by Acceptance Criteria

| TC ID | Linked AC | Test Type | Scenario | Preconditions | Steps | Expected Result | Automation Target |
|---|---|---|---|---|---|---|---|
| TC-001 | AC-001 | Functional | [Happy path scenario] | [State/data required] | [Short step list] | [Observable outcome] | [Unit/Integration/E2E/Manual] |
| TC-002 | AC-002 | Validation | [Invalid/edge scenario] | [State/data required] | [Short step list] | [Exact error/fallback] | [Unit/Integration/E2E/Manual] |

Rules:
- Every AC must map to at least one test case.
- Include at least one negative/edge test case.
- Prefer automation targets where practical; mark manual-only cases explicitly.

## Final Verification

### Acceptance Criteria Checklist

- [ ] [Criterion 1 from user story]
- [ ] [Criterion 2 from user story]
- [ ] [Additional criteria...]

### Integration Testing

```bash
[Commands to run full integration tests]
```

### Automated Test Execution

```bash
[Commands to run test suite that covers mapped AC test cases]
```

### Manual Testing Steps

1. [Step-by-step manual testing instructions]
2. [Expected results for each test]

## Notes & Considerations

- [Any important technical decisions]
- [Potential challenges and solutions]
- [Future enhancement considerations]

---

## Step Guidelines

### Step Size & Granularity
- Each step should take **15-60 minutes** to complete
- Break larger tasks into smaller, verifiable chunks
- Each step should have a clear, testable outcome
- Steps should build incrementally on previous work

### Implementation Details Format
Keep implementation details **concise and focused**:
- **Files to create/modify:** List specific file paths with brief purpose
- **Key changes:** High-level description of what needs to be implemented
- **Methods/Functions:** Names of main methods/functions to create or modify
- **Brief description:** Short explanation of the approach, not full code

### Verification Methods
Each step should include:
- **Validation Commands**: Actual commands to test the implementation
- **Expected Results**: What success looks like
- **Clear Success Criteria**: How to know the step is complete
- **AC/Test Traceability**: Reference which AC IDs and TC IDs the step enables

---

## Save the output

**Path:** `docs/technical-plans/{number}-plan-{story-name}.md`

- Use the same `{number}` as the corresponding user story
- `{story-name}` = kebab-case feature name matching the user story
- Example: if user story is `story-001-user-login.md`, plan should be `001-plan-user-login.md`
- Create the `docs/technical-plans/` directory if it does not exist
- If a file at that path already exists, ask the user before overwriting

Confirm the saved path to the user after writing.

## Quality Standards

### Developer Alignment
- Use specific technical terminology developers understand
- Reference exact file paths and command syntax
- Provide context for technical decisions without overwhelming detail
- Focus on **what** needs to be done rather than **how** in detail

### Incrementality
- Each step should work independently
- Developer can stop/start at any step boundary
- Previous steps remain functional when adding new steps
- Clear dependencies between steps

### Testability
- Every step must be verifiable before moving to next
- Provide clear validation commands
- Clear success/failure criteria
- Include both functional and integration testing
- Ensure full AC coverage with linked test cases (TC IDs)
- Prefer automated test coverage for stable critical paths

## Example Step Structure

```markdown
### Step 3: Implement Health Check Endpoint ⏱️ 30 mins
**Goal:** Create a /health endpoint that returns service status and timestamp
**Verification:** GET request to /health returns 200 with JSON response

#### Implementation Details:
- **Files to modify:** `backendv2/main.py`
- **Key changes:** Add health check endpoint with status, timestamp, service info
- **Methods/Functions:** `health_check()` async function
```
