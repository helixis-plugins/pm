---
description: Get guidance on what to do next — what to tell Builder, what mode to use, what to prepare
---

# /pm:guide

Tells you exactly what to do next. Reads the project state and gives specific, actionable instructions.

## Process

1. Use the **project-state** skill to detect the current state
2. Use the **build-guidance** skill to generate guidance

## State: Empty

```
No project found. Run /pm:plan to start planning.
```

## State: Planned

No packages started yet. Guide to the first package:

1. Read BP-01 spec
2. Identify any pre-requisites
3. Present the full next-package briefing:
   - What BP-01 builds
   - Pre-requisites to prepare
   - Launch command for Builder
   - Mode recommendation
   - Anticipated Builder questions

## State: Mid-build — No active package

A package was recently completed but the next one hasn't started:

1. Identify the next eligible package (dependencies met)
2. Present the full next-package briefing
3. Include launch command: `/dev:package start BP-XX`

## State: Mid-build — Package active

Builder is working on a package:

1. Read the active package from STATUS.md
2. Determine current task from progress count
3. Present in-progress guidance:
   - Current package and progress
   - Current task description
   - What to tell Builder to continue
   - Any blockers or decisions needed

## State: Complete

```
All packages complete. Nothing to build.
Run /pm:plan for v2 planning, templates, or post-mortem.
```

## Output Format

### Next package briefing:
```
Next up: BP-XX ([Name])

[Goal summary]

Pre-requisites:
  - [item] or "None — Builder can start immediately"

Tell Builder:
  /dev:package start BP-XX

Mode: [mode] — [why]

Builder will likely ask:
  Q: [question]
  A: [suggested answer]

Watch for:
  - [red flag]
```

### In-progress guidance:
```
Currently working on: BP-XX ([Name])
Progress: [X]/[Y] tasks

Current task: [N] — [description]

Tell Builder:
  "Continue with task [N]: [description]"
```
