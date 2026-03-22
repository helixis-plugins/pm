# BP-08: Progress Tracking + Parked Items

## Goal
`/pm:status` shows complete project state including parked items. PM maintains PARKED.md.

## Depends On
- BP-02 Project State Detection — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/status.md — full implementation
- [x] skills/progress-tracking/SKILL.md — full implementation

## Tasks
1. Implement STATUS.md parsing for active package, progress, last session
2. Implement package file scanning for done/in-progress/pending counts
3. Implement DECISIONS.md parsing for decision count
4. Implement PARKED.md reading and display
5. Implement git log reading for recent activity
6. Implement "next steps" recommendation from next pending package
7. Implement PARKED.md write capability (add items during any PM interaction)

## Specifications

`/pm:status` output:
```
Project: [Name]

Done: BP-01, BP-02, BP-03
In Progress: BP-04 (3/6 tasks)
Pending: BP-05, BP-06
Post-MVP: BP-07, BP-08
Parked: [count] items
Decisions: [count] made
Last session: [date] — [summary]
Next: [specific next step]
```

PARKED.md is maintained by PM across all interactions. When user mentions something that isn't in scope → PM asks "Want me to park that for later?" → adds to PARKED.md with source and date.

## Acceptance Criteria
- [ ] `/pm:status` shows accurate state for all packages
- [ ] Parked items displayed in status
- [ ] Decisions count shown
- [ ] Last session info from STATUS.md
- [ ] Next step recommendation based on next pending package
- [ ] PM can add items to PARKED.md during any interaction
- [ ] Works in all project states (planned, mid-build, complete)

## Out of Scope
Build guidance (BP-09), troubleshooting (BP-10).

## Status
- **State:** review
- **Progress:** 7/7 tasks
- **Branch:** bp/08-progress-tracking
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #15
