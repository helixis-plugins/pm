---
description: Run quality checks — validate plugins, check packages, run tests, verify standards
---

# /pm:review

Runs pre-flight checks to validate the project. Use before shipping, before switching to Builder, or anytime you want to verify project health.

## Process

1. Use the **project-state** skill to read the current state
2. Use the **quality-assurance** skill to run all checks
3. Present results as a checklist

## Checks Run

| # | Check | What it does |
|---|-------|-------------|
| 1 | Plugin validation | Runs `claude plugin validate .` if `.claude-plugin/` exists |
| 2 | Package completion | Verifies all deliverable files exist for completed packages |
| 3 | Tests | Runs test command from STANDARDS.md if configured |
| 4 | Dependencies | Verifies all package dependencies are satisfied |
| 5 | Decisions | Flags unresolved decisions in DECISIONS.md |
| 6 | Standards | Runs linter if configured, checks conventions |
| 7 | README | Verifies README exists and has real content |

Checks that don't apply are skipped with "(N/A)" — not reported as failures.

## Output

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

## After the check

- **All pass** → "All checks passed. Ready to ship."
- **Some fail** → "[N] items need attention. Want to resolve them now?"
  - If yes → guide through each failing item
  - If no → user handles on their own

## When to use

- Before running `/dev:package done` — automatically called by the done flow
- Before deploying or shipping
- Before switching from PM to Builder (pre-handoff)
- Anytime you want a health check on the project
