# BP-04: Spec Writing

## Goal
After interview confirmation, PM writes a full product spec and presents it for review.

## Depends On
- BP-03 Product Interview — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/plan.md — spec writing phase added after interview
- [x] skills/spec-writing/SKILL.md — full implementation

## Tasks
1. Implement spec generation from interview answers
2. Implement spec presentation in conversation
3. Implement revision flow — user requests changes, PM rewrites
4. Implement approval flow — user approves, proceed to packages

## Specifications

Spec follows the template format from `templates/product-spec.md`. Every section populated from interview — no invented content. Technical architecture names specific tools. "Does NOT do" section is mandatory.

Presented in conversation. User can request changes — PM revises and re-presents. On approval → proceed to package generation.

Spec held in memory — becomes `.dev/PROJECT.md` during scaffolding.

## Acceptance Criteria
- [ ] Spec follows template format exactly
- [ ] All sections populated from interview — nothing invented
- [ ] Technical architecture is specific (framework names, not generic)
- [ ] "Does NOT do" section exists and is meaningful
- [ ] User can request changes and PM revises
- [ ] Approved spec retained for package generation

## Out of Scope
Packages, scaffolding, decisions.

## Status
- **State:** done
- **Progress:** 4/4 tasks
- **Branch:** merged
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #7
