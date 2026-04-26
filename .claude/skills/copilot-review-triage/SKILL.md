---
name: copilot-review-triage
description: "Triage a batch of GitHub Copilot code-review comments: verify each finding against the actual code, classify it, and produce a verdict table + fix plan. Paste the Copilot comments before running."
---

You are an expert software engineer performing a second-opinion review of GitHub Copilot code-review comments.

## Your input

Paste the raw Copilot review comments below (copy from the GitLab / GitHub MR diff view).

If no comments are provided, ask the user to paste them before continuing.

---

## Task

For each Copilot comment:

1. **Read the actual file** at the cited location — do not trust the comment alone.
2. **Classify** the finding using the verdict taxonomy below.
3. **Decide the action** (Fix / Document / Skip).
4. **Execute all Fixes** in a single pass after the table is agreed.

---

## Verdict Taxonomy

| Emoji | Label                     | Meaning                                                                                              |
| ----- | ------------------------- | ---------------------------------------------------------------------------------------------------- |
| ✅    | Real bug                  | Confirmed defect — must fix                                                                          |
| ⚠️    | Defensive gap             | Not a bug today but will bite later — fix if low effort                                              |
| ℹ️    | Known trade-off           | Intentional design decision — document only                                                          |
| 🏗️    | Architectural suggestion  | Valid long-term refactor — skip for POC, log as tech-debt                                            |
| 🗓️    | Out of scope              | Correct observation but belongs in a future story                                                    |
| ❌    | Copilot wrong             | Finding is incorrect or inaccurate — no action                                                       |
| 🚨    | **Business logic change** | Suggestion would alter existing product behaviour — **must highlight to customer before any action** |

---

## Output — Part 1: Verdict Table

Print this table immediately after reading the code. Do not fix anything yet.

```
| # | Copilot Finding | Verdict | Action |
|---|---|---|---|
| 1 | <short summary> | ✅ Real bug | Fix — <one line description> |
| 2 | <short summary> | ℹ️ Known trade-off — <reason> | Document only |
| 3 | <short summary> | 🏗️ Architectural suggestion, fine for POC | Skip for now |
| 4 | <short summary> | 🚨 Business logic change — <what behaviour changes> | **Highlight to customer — await decision** |
...
```

After printing the table, ask:

> **Proceed with all fixes listed above?**
> Reply `yes` to apply all, or list the numbers you want to skip.

---

## Output — Part 2: Apply Fixes

After confirmation, apply every item marked **Fix** or **Document only**:

- **Fix**: edit the file directly using the correct tool.
- **Document only**: add a `// TODO:` comment or update the work log note — do not change logic.
- **Skip**: no action.
- **Copilot wrong**: no action, but note why in a brief comment to the user.
- **Business logic change**: do NOT apply the change. Instead, present a clear explanation to the customer:
  > 🚨 **Business logic change detected**
  > Copilot suggests: `<what it recommends>`
  > Current behaviour: `<what the code does today>`
  > Impact if applied: `<what would change for users/data>`
  > **Do you want to apply this change? (yes / no / discuss)**
  > Only proceed after explicit customer approval.

After all edits, run the repository's standard validation commands relevant to the changed files (typecheck/build/test/lint).

- Detect the correct toolchain from the project itself (for example: npm, pnpm, yarn, bun, gradle, maven, dotnet, python, go, etc.).
- If multiple packages/services were changed, validate each affected package/service.
- Report concise pass/fail status per package/service in the final summary.

---

## Output — Part 3: Fix Summary

Print a final summary:

```
## Fix Summary

| # | Finding | Result |
|---|---|---|
| 1 | <finding> | ✅ Fixed — <file changed, brief description> |
| 2 | <finding> | 📝 Documented — added TODO comment |
| 3 | <finding> | ⏭️ Skipped — architectural, post-POC |
| 4 | <finding> | ❌ Not fixed — Copilot incorrect because <reason> |
| 5 | <finding> | 🚨 Highlighted to customer — awaiting decision |
```

Validation checks: **[package/service]: OK/FAILED** (list any errors and resolution).

---

## Rules

- Always read the actual code before forming a verdict — Copilot can be wrong.
- Never fix architectural suggestions without explicit user approval.
- **Never apply a business logic change without explicit customer approval** — always classify as 🚨 and present the impact clearly before acting.
- Keep fixes minimal and scoped — do not refactor unrelated code.
- If a fix introduces new complexity, flag it and ask before proceeding.
- Document only = one-line `// TODO (tech-debt):` comment at the relevant site.
