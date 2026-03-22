---
name: project-state
description: "Detects the current project state (empty, planned, mid-build, complete) by reading .dev/ directory, STATUS.md, and package files."
---

# Project State Detection

This skill detects the current project state and provides structured context for other PM commands. Run this skill at the start of every PM session and before any command that needs to know the project state.

## Detection Procedure

Follow these steps in order. Stop at the first match.

### Step 1: Check for .dev/ directory

Look for a `.dev/` directory in the current working directory.

- If `.dev/` does **not** exist → state is **empty**
- If `.dev/` exists → continue to Step 2

### Step 2: Check for package files

Look for `BP-*.md` files in `.dev/packages/`.

- If `.dev/packages/` does not exist → state is **empty**
- If `.dev/packages/` exists but contains no `BP-*.md` files → state is **empty**
- If `BP-*.md` files exist → continue to Step 3

### Step 3: Read package states

For every `BP-*.md` file in `.dev/packages/`:

1. Read the file
2. Find the line matching `- **State:**` in the Status section
3. Extract the state value (one of: `not_started`, `in_progress`, `review`, `done`)
4. If no `- **State:**` line is found → treat that package as `not_started`

Collect all states into a list.

### Step 4: Determine overall project state

Apply these rules to the collected states:

| Condition | Project State |
|-----------|--------------|
| All states are `not_started` | **planned** |
| All states are `done` | **complete** |
| Any other combination | **mid-build** |

## Edge Case Handling

### Missing STATUS.md

If `.dev/` and package files exist but `.dev/STATUS.md` is missing:

1. Report: "STATUS.md is missing. Rebuilding from package files."
2. Read all package files to determine their states and progress
3. Generate a new STATUS.md using the `templates/STATUS.md` template:
   - Set Active Package to "None" (unless a package has state `in_progress` — then set that as active)
   - Set Last Session date to today
   - Set Summary to "STATUS.md rebuilt from package files"
   - Build the Package Overview table from all package files
4. Write the rebuilt STATUS.md to `.dev/STATUS.md`
5. Continue with normal state detection

### Malformed package file

If a package file exists but cannot be parsed (no Status section, garbled content, empty file):

- Treat that package as `not_started`
- Do not crash or stop — continue reading other packages
- Report which file was malformed: "Warning: [filename] has no Status section — treating as not_started."

### Empty .dev/packages/ directory

If `.dev/packages/` exists but contains no `BP-*.md` files:

- State is **empty** — the project has a partial setup but no plan
- Report: "No packages found. The project needs planning."

## Output Messages

After detecting the state, report one of these messages:

### Empty
```
New project. No plan found.

Run /pm:plan to start planning, or create packages with /dev:package new.
```

### Planned
```
Project planned — [N] packages, all not started.

Packages:
[list package numbers and names]

Run /pm:guide for what to do next, or start building with /dev:package start BP-01.
```

### Mid-build
```
Project in progress.

Done: [list or count]
In Progress: [package name] ([X/Y tasks])
Pending: [list or count]

Run /pm:status for full details, or /pm:guide for what to do next.
```

### Complete
```
All [N] packages complete.

Run /pm:plan for v2 planning, templates, or post-mortem.
```

## Reading STATUS.md

When STATUS.md exists, parse these fields from the Active Package section:

- **Package:** — the active package name, or "None"
- **Branch:** — the current git branch
- **Status:** — the active package status
- **Progress:** — task progress (e.g., "3/7 tasks")
- **Mode:** — involvement mode

And from the Last Session section:

- **Date:** — when the last session occurred
- **Summary:** — what happened
- **Next:** — what was planned next
- **Blockers:** — any blockers

And from the Package Overview table:

- Parse each row to get package number, name, state, and progress
- Use this as a quick reference instead of reading every package file individually

## Data Returned

After detection, make the following information available to the calling command:

1. **project_state** — one of: `empty`, `planned`, `mid-build`, `complete`
2. **package_count** — total number of packages
3. **packages_done** — list of done package numbers and names
4. **packages_in_progress** — list of in-progress package numbers and names
5. **packages_pending** — list of not_started package numbers and names
6. **active_package** — the currently active package (from STATUS.md), or null
7. **warnings** — any edge case warnings encountered during detection
