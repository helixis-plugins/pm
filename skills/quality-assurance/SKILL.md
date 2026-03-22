---
name: quality-assurance
description: "Runs pre-flight checks including plugin validation, package completion, tests, dependency satisfaction, decision resolution, and standards compliance."
---

# Quality Assurance

This skill runs pre-flight checks to validate a project before shipping or before marking a package complete. It checks multiple dimensions and presents results as a clear checklist.

## Checks

Run all applicable checks. Skip checks that don't apply with "(N/A)".

### Check 1: Plugin Validation

**Applies when:** the project has a `.claude-plugin/` directory.

**How to check:**
1. Look for `.claude-plugin/plugin.json`
2. If found, run `claude plugin validate .`
3. Report pass or fail with details

**Result:**
- Pass: "Plugin validation: passed"
- Fail: "Plugin validation: FAILED — [error details]"
- N/A: "Plugin validation: N/A (not a plugin project)"

### Check 2: Package Completion

**Applies when:** any packages have state `done` or `review`.

**How to check:**
For each completed package:
1. Read the deliverables list
2. Verify each deliverable file exists on disk (use the file path from the `- [x]` line)
3. Count how many exist vs how many are listed

**Result:**
- Pass: "BP-01 through BP-XX: all deliverables present"
- Fail: "BP-XX: missing deliverables — [list missing files]"

### Check 3: Test Runner

**Applies when:** STANDARDS.md specifies a test command.

**How to check:**
1. Read `.dev/STANDARDS.md`
2. Look for a "Testing" section with a "Run:" command (e.g., `pytest`, `npm test`, `vitest`)
3. If found, run the test command
4. Report results

**Result:**
- Pass: "Tests: [X]/[X] passing"
- Fail: "Tests: [X]/[Y] passing — [failure details]"
- N/A: "Tests: N/A (no test command configured)"

### Check 4: Dependency Satisfaction

**Applies when:** any packages exist.

**How to check:**
For each package that is `in_progress` or `not_started`:
1. Read its "Depends On" section
2. For each dependency, check that the dependency package has state `done`
3. Flag any unsatisfied dependencies

**Result:**
- Pass: "Dependencies: all satisfied"
- Fail: "Dependencies: BP-XX depends on BP-YY which is not done"

### Check 5: Decision Resolution

**Applies when:** `.dev/DECISIONS.md` exists.

**How to check:**
1. Read DECISIONS.md
2. Look for any entries that have "UNRESOLVED" or "TBD" or empty Decision fields
3. Also scan package specs for mentions of decisions that need to be made but aren't in DECISIONS.md

**Result:**
- Pass: "Decisions: all resolved ([N] total)"
- Fail: "Decision D-XXX unresolved: [title]"
- N/A: "Decisions: N/A (no decisions logged)"

### Check 6: Standards Compliance

**Applies when:** `.dev/STANDARDS.md` exists.

**How to check:**
1. Read STANDARDS.md for configured linter
2. If a linter is configured (e.g., `ruff`, `eslint`, `prettier`):
   - Check if the linter is installed
   - If installed, run it
   - Report results
3. If no linter configured, check basic conventions:
   - File naming matches STANDARDS.md patterns
   - Directory structure matches expectations

**Result:**
- Pass: "Standards: code follows STANDARDS.md"
- Fail: "Standards: [N] issues — [summary]"
- N/A: "Standards: N/A (no linter configured, manual review recommended)"

### Check 7: README Check

**Applies when:** always.

**How to check:**
1. Look for README.md in the project root
2. If it exists, check it's not a placeholder (has more than 5 lines of real content)
3. Check for key sections: project name, setup/installation, usage

**Result:**
- Pass: "README: exists and documented"
- Fail: "README: missing" or "README: placeholder only — needs real content"

## Presentation

Present all results as a checklist:

```
Pre-flight check for [Project Name]:

✅ Plugin validation: passed
✅ BP-01 through BP-04: all deliverables present
✅ Tests: 24/24 passing
✅ Dependencies: all satisfied
⚠️  Decision D-005 unresolved: deployment target
✅ Standards: code follows STANDARDS.md
⚠️  README: missing deployment instructions

2 items need attention. Want to resolve them now?
```

### Icons

- ✅ — check passed
- ⚠️ — check failed or needs attention
- ➖ — check not applicable (N/A)

### Summary

After the checklist:
- If all pass: "All checks passed. Ready to ship."
- If any fail: "[N] items need attention. Want to resolve them now?"
- If user wants to resolve: guide them through each failing item

## Context-specific checks

### Pre-handoff check (before switching from PM to Builder)

Run these additional checks:
- Is the project in a clean git state? (`git status`)
- Are all pre-requisites for the next package in place?
- Does Builder have everything it needs?

### Pre-ship check (before deployment)

Run these additional checks:
- All MVP packages marked done
- Integration test passed (if applicable)
- No unresolved TODOs in the codebase (`grep -r "TODO" --include="*.py" --include="*.ts" --include="*.js"`)
- All decision points resolved
- README documents setup and usage

## What NOT to Do

- Do NOT fix issues automatically — report them, let the user or Builder fix
- Do NOT fail on N/A checks — skip them gracefully
- Do NOT block on warnings — report and let the user decide
- Do NOT run destructive commands — only read and validate
- Do NOT skip any applicable check — run everything that applies
