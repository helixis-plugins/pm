# BP-09: Build Guidance

## Goal
`/pm:guide` tells the user exactly what to do next — what to tell Builder, what mode to use, what pre-requisites to prepare.

## Depends On
- BP-02 Project State Detection — must be complete
- BP-08 Progress Tracking + Parked Items — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/guide.md — full implementation
- [x] skills/build-guidance/SKILL.md — full implementation

## Tasks
1. Implement next-package detection from STATUS.md
2. Implement package spec analysis — what Builder will need
3. Implement pre-requisite identification (API keys, accounts, config)
4. Implement Builder-interaction scripting (what to say, how to answer)
5. Implement involvement mode recommendation
6. Implement in-progress guidance (what's the next task within active package)

## Specifications

`/pm:guide` reads STATUS.md and the next package spec, then produces:

1. **What's next** — which package, what it builds
2. **Pre-requisites** — things to prepare before Builder starts (API keys, accounts, config, decisions)
3. **Launch command** — exact command to tell Builder
4. **Mode recommendation** — which involvement mode and why
5. **Anticipated questions** — what Builder will likely ask and how to answer
6. **Red flags** — anything to watch for during the build

If a package is already in progress:
- Shows current task within the active package
- Suggests what to tell Builder to continue
- Flags any blockers or decisions needed

## Acceptance Criteria
- [ ] `/pm:guide` identifies the next package correctly
- [ ] Pre-requisites listed (API keys, config, etc.)
- [ ] Exact launch command provided
- [ ] Mode recommendation with reasoning
- [ ] Anticipated Builder questions with suggested answers
- [ ] Works when no package is active (recommends starting next)
- [ ] Works when package is in progress (shows current task)

## Out of Scope
Actually launching Builder, writing code.

## Status
- **State:** done
- **Progress:** 6/6 tasks
- **Branch:** merged
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #17
