---
description: Create project files from an approved plan — .dev/ structure, packages, CLAUDE.md, git
---

# /pm:scaffold

Creates the full project structure from an approved plan. Usually called automatically by `/pm:plan` after all decisions are resolved. Can be run standalone for edge cases.

## When to Use Standalone

- The planning flow was interrupted after approval but before scaffolding
- You want to re-scaffold a project (e.g., after correcting the plan manually)
- You have a spec and packages in memory but haven't scaffolded yet

## Prerequisites

The following must be available in conversation memory:

1. **Approved spec** — a complete product spec
2. **Approved packages** — all build packages with dependencies
3. **Resolved decisions** — all decision points resolved (or none needed)

If any prerequisite is missing, report what's missing and stop.

## Process

Use the **scaffold-creation** skill to execute the full scaffold:

1. Check for existing `.dev/` directory — warn if it exists, ask to confirm overwrite
2. Create `.dev/` directory structure with `packages/` subdirectory
3. Write `.dev/PROJECT.md` from approved spec
4. Write all package files to `.dev/packages/`
5. Write `.dev/STATUS.md` with package overview table
6. Write `.dev/DECISIONS.md` with resolved decisions (or empty template)
7. Write `.dev/STANDARDS.md` based on tech stack
8. Write `.dev/LEARNING.md` from template
9. Write `.dev/PARKED.md` from template (empty)
10. Write `CLAUDE.md` with session start protocol (append if exists)
11. Write `README.md` with project info and setup instructions
12. Initialize git if needed, stage all files, commit
13. Create GitHub repo if `gh` CLI is available (optional, silent)
14. Present handoff confirmation with next steps

## Output

On success, the current directory will contain:

```
[current directory]/
├── .dev/
│   ├── PROJECT.md          ← approved spec
│   ├── STATUS.md           ← package tracking
│   ├── DECISIONS.md        ← resolved decisions
│   ├── STANDARDS.md        ← tech stack conventions
│   ├── LEARNING.md         ← concept tracker
│   ├── PARKED.md           ← deferred items
│   └── packages/
│       ├── BP-01-[name].md
│       ├── BP-02-[name].md
│       └── ...
├── CLAUDE.md               ← agent instructions
└── README.md               ← project documentation
```

Git will be initialized with a first commit. The project is ready for Builder.
