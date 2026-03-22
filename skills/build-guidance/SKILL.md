---
name: build-guidance
description: "Analyzes the next package spec and provides specific guidance on what to do next, what to tell Builder, and what to prepare."
---

# Build Guidance

This skill analyzes the project state and the next package spec to provide specific, actionable guidance. It tells the user exactly what to do, what to tell Builder, and what to prepare.

## Two Modes

### Mode 1: No package active — guide to next package

When STATUS.md shows no active package, identify the next package to start and provide a full briefing.

### Mode 2: Package in progress — guide within active package

When a package is already active, show where things stand and what to do next within it.

---

## Mode 1: Next Package Briefing

### Step 1: Identify the next package

1. Read STATUS.md and all package files
2. Find the first `not_started` package whose dependencies are all `done`
3. If no package is eligible (dependencies not met), report which dependencies are blocking

If all packages are done → "All packages complete. Run `/pm:plan` for post-completion options."

### Step 2: Read the package spec

Read the full spec of the next package. Extract:
- Goal
- Deliverables
- Tasks
- Acceptance criteria
- Involvement mode
- Dependencies (what was already built)
- Out of scope

### Step 3: Identify pre-requisites

Scan the package spec for things the user needs to prepare before Builder starts:

**External accounts/services:**
- API keys mentioned → list which ones and where to get them
- Third-party accounts → list which ones to create
- Database setup → note if a database needs to be created or configured

**Environment variables:**
- Scan deliverables and tasks for mentions of env vars, config files, secrets
- List each one with what value it needs

**Decisions needed:**
- Check DECISIONS.md — are there unresolved decisions that affect this package?
- If yes, suggest resolving them first via `/pm:decide`

**Local setup:**
- Dependencies to install
- Tools to configure
- Files to create before Builder starts

If no pre-requisites → "No preparation needed. Builder can start immediately."

### Step 4: Generate the launch command

```
Tell Builder:
  /dev:package start BP-XX
```

### Step 5: Recommend involvement mode

Read the mode from the package spec and explain why:

| Mode | When to recommend | Explanation |
|------|------------------|-------------|
| `autopilot` | Straightforward plumbing, CRUD, config, boilerplate | "This is standard implementation work. Let Builder run and check the result." |
| `narrator` | Standard features with some judgment calls | "Builder will explain what it's doing. Step in if something looks off." |
| `guided` | Complex logic, unfamiliar tech, important architecture | "You should review each step. Builder will ask before making key decisions." |
| `pair` | Critical systems, security, novel algorithms | "Work alongside Builder on this one. It needs your input throughout." |

### Step 6: Anticipate Builder questions

Based on the package spec, predict what Builder will likely ask:

1. Read the tasks and deliverables
2. Identify ambiguous points, missing details, or choices Builder might face
3. For each anticipated question, provide a suggested answer

Format:
```
Builder will likely ask:

Q: "Should I use [X] or [Y] for [feature]?"
A: "[Recommended answer and why]"

Q: "Where should [file] go?"
A: "[Specific path based on project conventions]"

Q: "What about [edge case]?"
A: "Out of scope for this package — skip it."
```

If nothing is ambiguous → "This package is well-specified. Builder shouldn't need much input."

### Step 7: Identify red flags

Things to watch for during the build:

- Tasks that might take longer than expected
- Dependencies on external services that might be down
- Common failure modes for this type of work
- Things that could go out of scope

If nothing notable → skip this section.

### Step 8: Present the briefing

```
Next up: BP-XX ([Package Name])

[One-sentence goal summary]

Pre-requisites:
  [list, or "None — Builder can start immediately"]

Tell Builder:
  /dev:package start BP-XX

Mode: [mode] — [brief explanation]

Builder will likely ask:
  Q: [question]
  A: [answer]

Watch for:
  [red flags, or omit if none]
```

---

## Mode 2: In-Progress Guidance

### Step 1: Read active package state

From STATUS.md:
- Which package is active
- Current progress (X/Y tasks)

From the package spec:
- Full task list
- Which tasks are done (infer from progress count — first X tasks are done)

### Step 2: Identify the current task

The next task is task number (progress + 1).

Read the task description and the relevant deliverables.

### Step 3: Check for blockers

- Are there unresolved decisions in DECISIONS.md that affect the current task?
- Are there missing pre-requisites?
- Has the user mentioned any issues?

### Step 4: Present in-progress guidance

```
Currently working on: BP-XX ([Package Name])
Progress: [X]/[Y] tasks

Current task: [task number] — [task description]

Tell Builder:
  "Continue with task [N]: [description]"

[Any blockers or notes]
```

---

## What NOT to Do

- Do NOT tell the user to write code — that's Builder's job
- Do NOT launch Builder — just tell the user what to type
- Do NOT make decisions for the user — present options, let them choose
- Do NOT skip the pre-requisites check — missing API keys waste Builder's time
- Do NOT guess about task completion — only report what STATUS.md and the package spec say
