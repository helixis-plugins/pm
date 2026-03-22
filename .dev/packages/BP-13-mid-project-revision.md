# BP-13: Mid-Project Revision

## Goal
When PM detects mid-build state, it lets the user modify the plan — add, remove, split, merge packages.

## Depends On
- BP-02 Project State Detection — must be complete
- BP-05 Build Package Generation — must be complete
- BP-08 Progress Tracking + Parked Items — must be complete

## Involvement Mode
autopilot

## Deliverables
- [ ] commands/plan.md — revision mode (triggered by state detection for mid-build)

## Tasks
1. Implement state reading and presentation (done/in-progress/pending)
2. Implement add package (with dependency insertion)
3. Implement remove package (with dependency updates)
4. Implement split package
5. Implement merge packages
6. Implement spec update with package cascade
7. Implement re-plan pending packages
8. Implement promote from PARKED.md to package
9. Implement protection for done/in-progress packages
10. Implement revision commit

## Specifications

When `/pm:plan` detects mid-build:
1. Read and present current state (done/in-progress/pending/parked)
2. "What do you want to change?"
3. Execute the requested change
4. Refresh STATUS.md
5. Commit: `[PM] Revised: [description]`

Rules:
- State `done` → NEVER modify
- State `in_progress` → warn before modifying
- State `not_started` → modify freely
- No orphaned dependencies after any revision
- PARKED.md items can be promoted to real packages

## Acceptance Criteria
- [ ] Mid-build detected and state presented correctly
- [ ] Add package inserts with correct dependencies
- [ ] Remove package updates dependent packages
- [ ] Split creates two valid packages
- [ ] Merge combines correctly
- [ ] Done packages never modified
- [ ] In-progress warning shown
- [ ] STATUS.md refreshed after every change
- [ ] Revisions committed to git
- [ ] Parked items can be promoted to packages

## Out of Scope
Modifying done packages, undoing builds.

## Status
- **State:** not_started
- **Progress:** 0/10 tasks
- **Branch:** —
- **Started:** —
- **Completed:** —
