# BP-05: Build Package Generation

## Goal
After spec approval, PM breaks it into build packages matching the `dev` plugin's exact format.

## Depends On
- BP-04 Spec Writing — must be complete

## Involvement Mode
autopilot

## Deliverables
- [ ] commands/plan.md — package generation phase added after spec
- [ ] skills/package-generation/SKILL.md — full implementation

## Tasks
1. Implement spec analysis — identify features to group into packages
2. Implement dependency ordering
3. Implement package map generation (visual)
4. Implement individual package generation using template
5. Implement auto-generation of tasks from deliverables
6. Implement user review and revision of packages

## Specifications

Generation process:
1. Analyze spec capabilities → group into packages
2. Add foundation package first (BP-01)
3. Add integration test last (last MVP package)
4. Separate post-MVP packages
5. Order by dependencies
6. Generate package map
7. Generate each package from `templates/build-package.md`
8. Present map + descriptions
9. User can add/remove/split/merge → revise and re-present
10. User approves → proceed to decisions

Package format matches `dev` plugin exactly. File naming: `BP-XX-short-name.md`. Package count: 5-15 typical. Held in memory until scaffolding.

## Acceptance Criteria
- [ ] Package map generated with correct dependencies
- [ ] Each package matches `dev` plugin format exactly
- [ ] No circular dependencies
- [ ] Foundation first, integration test last
- [ ] Post-MVP clearly separated
- [ ] Realistic file paths in deliverables
- [ ] Testable acceptance criteria
- [ ] User can revise before finalizing

## Out of Scope
Creating files on disk, decision resolution.

## Status
- **State:** not_started
- **Progress:** 0/6 tasks
- **Branch:** —
- **Started:** —
- **Completed:** —
