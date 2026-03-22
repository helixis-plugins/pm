# BP-02: Project State Detection

## Goal
PM can detect whether the current folder is empty, planned, mid-build, or complete — and routes to the correct behaviour.

## Depends On
- BP-01 Plugin Skeleton + Templates — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] skills/project-state/SKILL.md — full implementation

## Tasks
1. Implement four-state detection logic (empty, planned, mid-build, complete)
2. Implement STATUS.md parsing — extract active package, progress, package table
3. Implement package file scanning — read State from every `.dev/packages/BP-*.md`
4. Implement corrupted state handling — missing STATUS.md, empty packages/, malformed files
5. Define the exact output message for each state

## Specifications

Detection logic:
1. Check if `.dev/` directory exists → if no: state is **empty**
2. Check if `.dev/packages/` has any `BP-*.md` files → if no: treat as **empty** (partial setup)
3. Read every `BP-*.md` file, extract the `- **State:**` line
4. If all states are `not_started` → state is **planned**
5. If all states are `done` → state is **complete**
6. Otherwise → state is **mid-build**

Edge cases:
- `.dev/` exists but STATUS.md missing → rebuild STATUS.md from package files, then continue
- Package file has no Status section → treat as `not_started`
- `.dev/packages/` exists but is empty → treat as **empty**

## Acceptance Criteria
- [ ] Empty folder → correctly detected
- [ ] Planned project (all not_started) → correctly detected
- [ ] Mid-build (mixed states) → correctly detected
- [ ] Complete (all done) → correctly detected
- [ ] Missing STATUS.md → rebuilt from packages without crashing
- [ ] Malformed package file → treated as not_started, no crash

## Out of Scope
The actual flows for each state — those are separate packages.

## Status
- **State:** review
- **Progress:** 5/5 tasks
- **Branch:** bp/02-project-state-detection
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #3
