# BP-12: Quality Assurance

## Goal
`/pm:review` runs pre-flight checks — validates plugins, checks packages, runs tests, verifies standards compliance.

## Depends On
- BP-02 Project State Detection — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/review.md — full implementation
- [x] skills/quality-assurance/SKILL.md — full implementation

## Tasks
1. Implement plugin validation check (if project is a plugin)
2. Implement package completion check — deliverables exist, tasks done
3. Implement test runner — execute test command from STANDARDS.md
4. Implement dependency check — all package dependencies satisfied
5. Implement decision check — all flagged decisions resolved
6. Implement standards check — code follows STANDARDS.md
7. Implement README check — exists and not placeholder
8. Implement results presentation as checklist

## Specifications

`/pm:review` runs all applicable checks and presents:
```
Pre-flight check for [Project Name]:

✅ Plugin validation: passed (or N/A if not a plugin)
✅ BP-01 through BP-04: all deliverables present
✅ Tests: 24/24 passing (or "no tests configured")
✅ Dependencies: all satisfied
⚠️  Decision D-005 unresolved
✅ Standards: code follows STANDARDS.md
⚠️  README: missing deployment instructions

2 items need attention. Want to resolve them now?
```

Checks that don't apply are skipped with "(N/A)" — not reported as failures.

## Acceptance Criteria
- [ ] Plugin validation runs if project has `.claude-plugin/`
- [ ] Package completion checked for all done packages
- [ ] Tests run if test command configured in STANDARDS.md
- [ ] Unresolved decisions flagged
- [ ] Results presented as clear checklist with checkmarks/warnings
- [ ] N/A checks skipped gracefully
- [ ] Offers to resolve flagged items

## Out of Scope
Fixing issues automatically — PM reports, user/Builder fixes.

## Status
- **State:** review
- **Progress:** 8/8 tasks
- **Branch:** bp/12-quality-assurance
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #23
