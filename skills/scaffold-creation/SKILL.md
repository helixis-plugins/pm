---
name: scaffold-creation
description: "Creates the .dev/ project structure, writes all project files, initializes git, and optionally creates a GitHub repo."
---

# Scaffold Creation

This skill populates the current working directory with a complete `.dev/` project structure, ready for Builder. It is invoked by `/pm:plan` after all decisions are resolved, or standalone via `/pm:scaffold`.

## Input

The following must be available in conversation memory:

- **Approved spec** — the full product spec (becomes PROJECT.md)
- **Approved packages** — all generated build packages
- **Resolved decisions** — all decision point resolutions (if any)
- **Tech stack** — from the interview (determines STANDARDS.md content)
- **Project name** — from the interview

## Pre-flight Check

Before creating anything:

### Check for existing .dev/

If `.dev/` already exists in the current directory:

1. Warn: "This folder already has a `.dev/` directory. Scaffolding will overwrite it."
2. Ask: "Continue? (yes/no)"
3. If no → stop
4. If yes → proceed (existing files will be overwritten)

### Verify data completeness

Confirm all required data is in memory:
- [ ] Spec is approved
- [ ] Packages are approved
- [ ] Decisions are resolved (or none needed)

If anything is missing → report what's missing and stop.

## Scaffold Process

### Step 1: Create directory structure

```
[current directory]/
├── .dev/
│   ├── packages/
│   ├── PROJECT.md
│   ├── STATUS.md
│   ├── DECISIONS.md
│   ├── STANDARDS.md
│   ├── LEARNING.md
│   └── PARKED.md
├── CLAUDE.md
└── README.md
```

Create `.dev/` and `.dev/packages/` directories.

### Step 2: Write PROJECT.md

Write the approved spec to `.dev/PROJECT.md`. Use the exact content approved by the user — no modifications.

### Step 3: Write package files

For each approved package:
1. Derive filename: `BP-XX-short-name.md`
2. Write the full package spec to `.dev/packages/[filename]`
3. All packages start with State: `not_started`

### Step 4: Write STATUS.md

Copy `templates/STATUS.md` VERBATIM and only replace the placeholder variables. Do NOT simplify, restructure, or omit any sections. The dev plugin depends on this exact format. Template:

```markdown
# Project Status

## Active Package
- **Package:** None
- **Branch:** main
- **Status:** initialized
- **Progress:** —
- **Mode:** —

## Last Session
- **Date:** [today]
- **Summary:** Project scaffolded by PM plugin
- **Next:** Start building with /dev:package start BP-01
- **Blockers:** None

## Package Overview

| Package | Name | State | Progress |
|---------|------|-------|----------|
| BP-01 | [name] | not_started | 0/[N] tasks |
| BP-02 | [name] | not_started | 0/[N] tasks |
...
```

Build the table from all package files.

### Step 5: Write DECISIONS.md

If decisions were resolved during planning:

```markdown
# Decision Log

## D-001: [Title]
- **Date:** [today]
- **Package:** BP-XX (if applicable)
- **Context:** [Why this decision was needed]
- **Decision:** [What was decided]
- **Rationale:** [Why this option was chosen]
- **Alternatives:** [What else was considered]
- **Revisit when:** [Trigger or "N/A"]

## D-002: [Title]
...
```

If no decisions were needed:

```markdown
# Decision Log

No decisions recorded yet. Use /pm:decide to log architectural and technical decisions.
```

### Step 6: Write STANDARDS.md

Generate based on the tech stack from the interview. Use these defaults:

**Python projects:**
```markdown
# Coding Standards

## Formatting
- Formatter: black
- Line length: 88 characters
- Indent: 4 spaces

## Linting
- Linter: ruff
- Type hints: encouraged on all public functions

## Testing
- Framework: pytest
- Test files: test_[name].py in tests/ directory
- Run: pytest

## Documentation
- Docstrings: Google style on public functions and classes
```

**Node/TypeScript projects:**
```markdown
# Coding Standards

## Formatting
- Formatter: prettier
- Indent: 2 spaces
- Semicolons: yes
- Quotes: single

## Linting
- Linter: eslint
- Strict mode: enabled

## Testing
- Framework: vitest (or jest)
- Test files: [name].test.ts in __tests__/ or alongside source
- Run: npm test

## TypeScript
- Strict mode: enabled
- No any: enforced
```

**React/Next.js projects:**
Add to Node/TypeScript standards:
```markdown
## Components
- Functional components only
- One component per file
- File naming: PascalCase.tsx
- CSS: Tailwind utility classes
```

**FastAPI projects:**
Add to Python standards:
```markdown
## API Conventions
- Endpoint naming: lowercase with hyphens
- Request/response models: Pydantic BaseModel
- Async handlers: use async def for all endpoints
- Error responses: HTTPException with appropriate status codes
```

**All projects include:**
```markdown
## Git Conventions
- Branch naming: bp/XX-short-name (for build packages), feature/name, fix/name
- Commit format: [BP-XX] description (when working in a build package)
- Commit after each completed task, not at the end of a session
- Never commit directly to main — always use branches and PRs
```

**Mixed/other stacks:** Combine relevant sections. If the stack is unrecognized, provide generic conventions and note they should be customized.

### Step 7: Write LEARNING.md

Copy from `templates/LEARNING.md`:

```markdown
# Learning Progress

## Concepts Encountered

| Concept | First Seen | Times Seen | Comfort Level |
|---------|-----------|------------|---------------|

No concepts tracked yet.

## Comfort Levels
- **new** — just introduced, needs full explanation
- **learning** — seen a few times, understands basics
- **familiar** — comfortable, brief reminders only
- **confident** — can do independently, no explanation needed
```

### Step 8: Write PARKED.md

Copy from `templates/PARKED.md` with empty items:

```markdown
# Parked Items

Ideas, requests, and deferred features mentioned during planning or building
that aren't in a build package yet. PM maintains this list.

## Items

No items parked yet.
```

### Step 9: Write CLAUDE.md

Generate using `templates/CLAUDE.md` with actual values:

```markdown
# [Project Name]

## Session Start Protocol
1. Read .dev/STATUS.md for current project state
2. Read the active build package spec in .dev/packages/ (if one is active)
3. Run git status to check for uncommitted work
4. Report a brief status to the user before doing anything else

## Project Quick Reference
- **Project:** [project name]
- **Stack:** [tech stack summary]
- **Standards:** See .dev/STANDARDS.md

## Rules
- NEVER work on anything not defined in the active build package
- ALWAYS check .dev/STATUS.md before starting work
- ALWAYS update .dev/STATUS.md when completing tasks
- ALWAYS commit working code before ending a session
- If asked to do something out of scope, flag it and offer to: add to current package, create a new package, or skip
- When no build package is active, prompt the user to start one before writing any code
```

If a CLAUDE.md already exists, **append** to it rather than overwriting.

### Step 10: Write README.md

Generate a project README:

```markdown
# [Project Name]

[One-line description from spec]

## Setup

[Tech-stack-specific setup instructions — e.g., npm install, pip install, etc.]

## Development

This project uses build packages managed by the `dev` plugin.

- Check status: `/dev:status`
- Start a package: `/dev:package start BP-XX`
- Complete a package: `/dev:package done`

See `.dev/STATUS.md` for current progress.

## Project Planning

Planned and managed by the PM plugin.

- View status: `/pm:status`
- Get guidance: `/pm:guide`
- Make decisions: `/pm:decide`
```

### Step 11: Initialize git

If the current directory is not already a git repo:

```bash
git init
git add .
git commit -m "Project setup: [name] — planned by PM plugin"
```

If it is already a git repo:

```bash
git add .
git commit -m "Project setup: [name] — planned by PM plugin"
```

### Step 12: Create GitHub repo (optional)

Silently check if `gh` CLI is available.

If available:
```bash
gh repo create $PM_DEFAULT_ORG/[project-name] --private --source=. --push
```

Use `PM_DEFAULT_ORG` environment variable (default: `helixisops`).

If `gh` is not available → skip silently. Do not warn or error.

### Step 13: Overwrite warning

Already handled in the Pre-flight Check section above. If `.dev/` exists, warn before proceeding.

### Step 14: Handoff confirmation

After everything is written and committed, present:

```
Project ready: [Name]
Location: [current directory path]
Packages: [N] ([M] MVP + [K] post-MVP)
First: BP-01 [first package name]

To start building:
  builder dev [project-name]
```

## What NOT to Do

- Do NOT create a new folder — write into the current directory
- Do NOT modify the approved spec — write it exactly as approved
- Do NOT skip any files — all 8 `.dev/` files must be created
- Do NOT write project source code — only project management files
- Do NOT fail silently — report what was created
- Do NOT modify .zshrc or shell config — only suggest commands
