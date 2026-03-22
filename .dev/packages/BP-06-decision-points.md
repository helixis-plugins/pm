# BP-06: Decision Points

## Goal
PM identifies decisions that must be resolved before building and walks the user through them.

## Depends On
- BP-04 Spec Writing — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/plan.md — decision point phase added after packages
- [x] Decision detection logic in skills/package-generation/SKILL.md

## Tasks
1. Implement decision detection from spec and packages
2. Implement decision presentation with options and recommendations
3. Implement one-at-a-time resolution flow
4. Store resolved decisions in memory for DECISIONS.md

## Specifications

Detect when: multiple DB options viable, deployment unspecified, auth approach unclear, email provider needed, domain name needed, API alternatives exist, user said "recommend one."

Present as table with options and which package needs it. Resolve one at a time — options, pros/cons, recommendation, user chooses. Stored in memory → written to DECISIONS.md during scaffolding.

## Acceptance Criteria
- [ ] Decision points detected from spec and packages
- [ ] Options presented with pros/cons
- [ ] Recommendations made when appropriate
- [ ] Resolved one at a time
- [ ] Zero decisions when everything was specified in interview

## Out of Scope
Decision tracking over project lifetime (that's `/pm:decide`).

## Status
- **State:** review
- **Progress:** 4/4 tasks
- **Branch:** bp/06-decision-points
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #11
