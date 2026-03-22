# BP-14: Post-Completion Planning

## Goal
When all packages are done, PM offers v2 planning, template saving, or post-mortem.

## Depends On
- BP-02 Project State Detection — must be complete
- BP-07 Project Scaffold — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/plan.md — post-completion mode (triggered by state detection for complete)

## Tasks
1. Implement complete state detection and option presentation
2. Implement v2 planning — new packages from last number
3. Implement template saving — strip content, save to ~/.dev-templates/
4. Implement post-mortem — review decisions, capture reflections

## Specifications

When `/pm:plan` detects all done:
```
All [N] packages complete.
1. Plan v2
2. Save as template
3. Post-mortem
```

v2: reads existing spec, interviews for new features, generates packages numbered from v1 end.

Template: copies `.dev/` to `~/.dev-templates/[name]/`, strips project content, keeps STANDARDS.md and structure.

Post-mortem: summarizes decisions from DECISIONS.md, asks what went well / what to improve, writes `.dev/POSTMORTEM.md`.

## Acceptance Criteria
- [ ] Complete state detected, three options presented
- [ ] v2 generates new packages with correct numbering
- [ ] v2 preserves done packages
- [ ] Template saved with content stripped
- [ ] Post-mortem summarizes decisions
- [ ] Works when ~/.dev-templates/ doesn't exist

## Out of Scope
Template marketplace, automated retrospectives.

## Status
- **State:** review
- **Progress:** 4/4 tasks
- **Branch:** bp/14-post-completion
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #27
