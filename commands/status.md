---
description: Show full project status — done, in progress, pending, parked, blocked, decisions
---

# /pm:status

Shows the complete project state at a glance.

## Process

1. Use the **project-state** skill to detect the current state (empty, planned, mid-build, complete)
2. Use the **progress-tracking** skill to generate the full status report

## State: Empty

If no `.dev/` directory or no packages:

```
No project found in this directory.
Run /pm:plan to start planning.
```

## State: Planned / Mid-build / Complete

Use the **progress-tracking** skill to:

1. Read project name from PROJECT.md
2. Parse STATUS.md for active package and last session
3. Scan all package files for done/in-progress/pending counts
4. Read DECISIONS.md for decision count
5. Read PARKED.md for parked item count
6. Read git log for recent activity
7. Determine next steps based on project state
8. Present the formatted status report

## Output Format

```
Project: [Name]

Done: BP-01 Foundation, BP-02 Auth, BP-03 Database
In Progress: BP-04 Dashboard (3/6 tasks)
Pending: BP-05 Email, BP-06 Integration Test
Post-MVP: BP-07 Batch Processing

Parked: 2 items
  - Mobile app version
  - CSV export
Decisions: 4 made
Last session: 2026-03-22 — Built dashboard layout and data fetching

Next: Finish BP-04 tasks 4-6 (chart rendering, filtering, responsive layout)
```

## Parked Items Behavior

- 0 items → "Parked: 0 items"
- 1-3 items → list by title inline
- 4+ items → show count

## Cross-command Parking

This command also reminds about the parking capability. If during any PM interaction the user mentions an out-of-scope idea, PM should offer to park it. The **progress-tracking** skill handles the PARKED.md write operations.
