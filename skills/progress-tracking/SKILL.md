---
name: progress-tracking
description: "Reads STATUS.md, package files, DECISIONS.md, PARKED.md, and git log to present comprehensive project status."
---

# Progress Tracking

This skill reads the full project state and presents a comprehensive status report. It also manages the PARKED.md file across all PM interactions.

## Status Report Generation

### Step 1: Read project name

Read `.dev/PROJECT.md` and extract the project name from the first heading.

If PROJECT.md doesn't exist, use the directory name as the project name.

### Step 2: Read STATUS.md

Parse the Active Package section:
- Package name (or "None")
- Branch
- Status
- Progress
- Mode

Parse the Last Session section:
- Date
- Summary
- Next
- Blockers

### Step 3: Scan package files

Read every `BP-*.md` file in `.dev/packages/`. For each:

1. Extract the package number and name from the first heading
2. Extract State from `- **State:**` line
3. Extract Progress from `- **Progress:**` line

Categorize into lists:
- **Done** — State is `done`
- **In Progress** — State is `in_progress` or `review`
- **Pending** — State is `not_started`

Separate MVP from post-MVP packages. Post-MVP packages are those numbered after the integration test package (the last MVP package, typically the highest-numbered package whose name contains "integration" or "test"). If no clear integration test package exists, treat all packages as MVP.

### Step 4: Read DECISIONS.md

Count the number of decision entries (lines matching `## D-`).

If DECISIONS.md doesn't exist, report "0 decisions".

### Step 5: Read PARKED.md

Read `.dev/PARKED.md` if it exists. Count items under the `## Items` section. Each item starts with `- **`.

If PARKED.md doesn't exist, report "0 parked items".

### Step 6: Read git log

Run `git log --oneline -5` to get the 5 most recent commits.

If not a git repo, skip this section.

### Step 7: Determine next steps

Find the next pending package (first `not_started` package whose dependencies are all `done`). Read its spec to provide a specific recommendation.

If a package is in progress, recommend continuing it with its current task.

If all packages are done, recommend running `/pm:plan` for post-completion options.

### Step 8: Present the report

Format the output:

```
Project: [Name]

Done: [list BP numbers and names, or "None"]
In Progress: [package name] ([X/Y tasks]) [or "None"]
Pending: [list BP numbers and names, or "None"]
Post-MVP: [list BP numbers and names, or "None"]

Parked: [count] items
Decisions: [count] made
Last session: [date] — [summary]

Next: [specific recommendation]
```

### State-specific variations

**Planned (all not_started):**
```
Project: [Name]
Status: Planned — ready to build

[N] packages, all pending.
First: BP-01 [name]

Run /dev:package start BP-01 to begin.
```

**Mid-build:**
Show the full report as described above.

**Complete (all done):**
```
Project: [Name]
Status: Complete

All [N] packages done.
[count] decisions made.

Run /pm:plan for v2 planning, templates, or post-mortem.
```

## PARKED.md Management

PM maintains PARKED.md across all interactions — not just during `/pm:status`.

### When to park an item

During any PM command (`/pm:plan`, `/pm:guide`, `/pm:fix`, `/pm:decide`, `/pm:status`, `/pm:review`), if the user mentions something that is:

- Not in the current package scope
- A feature idea for later
- A "nice to have" that doesn't fit the current plan
- Something they want to remember but not act on now

Then PM should:

1. Acknowledge the idea
2. Ask: "Want me to park that for later?"
3. If yes → add to PARKED.md

### Adding items to PARKED.md

Append to the `## Items` section:

```markdown
- **[Short title]** — [brief description].
  Source: [which PM interaction]. Date: [today's date].
```

### PARKED.md format

```markdown
# Parked Items

Ideas, requests, and deferred features mentioned during planning or building
that aren't in a build package yet. PM maintains this list.

## Items

- **Mobile app version** — mentioned during planning, deferred to v2.
  Source: initial interview. Date: 2026-03-22.

- **CSV export for invoices** — user asked about this during BP-03 build.
  Source: mid-build conversation. Date: 2026-03-25.
```

### Reading PARKED.md for status

When presenting parked items in `/pm:status`:

- If 0 items: "Parked: 0 items"
- If 1-3 items: list them by title
- If 4+ items: show count and say "Run `/pm:status parked` to see all"

## What NOT to Do

- Do NOT modify package files — this skill only reads them
- Do NOT update STATUS.md — other commands handle that
- Do NOT guess at project state — only report what the files say
- Do NOT skip PARKED.md — always check and report
