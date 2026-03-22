# PM Plugin

Your project lead for Claude Code. Plans projects, guides builds, troubleshoots problems, makes decisions, tracks progress, and ensures quality — everything except writing the code.

## Installation

Add to your shell config (`.zshrc` or `.bashrc`):

```bash
PLUGINS[pm]="$HOME/HelixisHQ/repos/github/helixisops/pm"
PROJECTS_DIR="$HOME/HelixisHQ/repos/github/helixisops"
```

Or load directly:

```bash
claude --plugin-dir /path/to/pm
```

## Commands

| Command | What It Does |
|---------|-------------|
| `/pm:plan` | Plan a new project, revise an existing plan, or handle post-completion |
| `/pm:status` | Full project status — done, in progress, pending, parked, decisions |
| `/pm:guide` | What to do next — instructions for Builder, mode, pre-requisites |
| `/pm:fix` | Diagnose and fix problems — exact commands, exact steps |
| `/pm:decide` | Structure a decision with options, trade-offs, and recommendation |
| `/pm:review` | Quality check — validate, test, check standards, pre-flight |
| `/pm:scaffold` | Create project files from approved plan (usually automatic) |

## How It Works

PM owns everything except writing code:

1. **Plan** — Interview, spec, build packages, scaffold
2. **Guide** — What to do next, what to tell Builder
3. **Fix** — Diagnose problems, give exact fixes
4. **Decide** — Structure decisions, log them
5. **Track** — Progress, parked items, decisions
6. **Review** — Quality checks before shipping

## Integration with Dev Plugin

PM and Builder share the same `.dev/` directory:

- PM writes the plan (PROJECT.md, packages, DECISIONS.md, STANDARDS.md)
- Builder executes it (updates STATUS.md, package progress)
- Same folder, different plugins, different capabilities

```
pm plan new-project     # PM plans the project
builder dev new-project # Builder builds the project
```

## Environment Variables

| Variable | Default | Purpose |
|----------|---------|---------|
| `PM_DEFAULT_ORG` | `helixisops` | Default GitHub org for new repos |
| `PM_DEFAULT_MODE` | `autopilot` | Default involvement mode for generated packages |
