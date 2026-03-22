# BP-01: Plugin Skeleton + Templates

## Goal
A valid Claude Code plugin that installs, passes validation, and has all slash commands and skills defined with placeholder responses. All templates exist.

## Depends On
- None

## Involvement Mode
autopilot

## Deliverables
- [x].claude-plugin/plugin.json — plugin manifest
- [x]commands/plan.md — placeholder
- [x]commands/status.md — placeholder
- [x]commands/guide.md — placeholder
- [x]commands/fix.md — placeholder
- [x]commands/decide.md — placeholder
- [x]commands/review.md — placeholder
- [x]commands/scaffold.md — placeholder
- [x]skills/project-state/SKILL.md — placeholder
- [x]skills/product-interview/SKILL.md — placeholder
- [x]skills/spec-writing/SKILL.md — placeholder
- [x]skills/package-generation/SKILL.md — placeholder
- [x]skills/scaffold-creation/SKILL.md — placeholder
- [x]skills/build-guidance/SKILL.md — placeholder
- [x]skills/troubleshooting/SKILL.md — placeholder
- [x]skills/decision-support/SKILL.md — placeholder
- [x]skills/progress-tracking/SKILL.md — placeholder
- [x]skills/quality-assurance/SKILL.md — placeholder
- [x]templates/product-spec.md
- [x]templates/build-package.md
- [x]templates/STATUS.md
- [x]templates/DECISIONS.md
- [x]templates/LEARNING.md
- [x]templates/PARKED.md
- [x]templates/CLAUDE.md
- [x]README.md

## Tasks
1. Create plugin directory structure with all folders
2. Write plugin.json: name "pm", author object, description
3. Write 7 placeholder command files with YAML frontmatter (description only)
4. Write 10 placeholder skill files with valid YAML frontmatter (name and description only)
5. Write all 7 template files with {PLACEHOLDER} variables
6. Write README with installation, commands list, integration info
7. Run `claude plugin validate .` and fix any errors

## Specifications

plugin.json:
```json
{
  "name": "pm",
  "version": "0.1.0",
  "description": "Your project lead. Plans projects, guides builds, troubleshoots problems, makes decisions, tracks progress, and ensures quality — everything except writing the code.",
  "author": {
    "name": "Helixis"
  }
}
```

Command frontmatter format — description only, no custom fields:
```yaml
---
description: Short description of what this command does
---
```

Skill frontmatter format — name and description only, no custom fields:
```yaml
---
name: skill-name
description: "Description of when this skill activates and what it does."
---
```

templates/PARKED.md:
```markdown
# Parked Items

Ideas, requests, and deferred features mentioned during planning or building
that aren't in a build package yet. PM maintains this list.

## Items

{PARKED_ITEMS}
```

All other templates follow the formats specified in the product spec.

## Acceptance Criteria
- [x]`claude plugin validate .` passes with no errors
- [x]Plugin loads via `--plugin-dir` with no errors
- [x]All 7 slash commands appear when typing `/pm:`
- [x]Each command responds with its placeholder message
- [x]All 10 skills have valid SKILL.md with proper frontmatter
- [x]All 7 templates exist and contain correct placeholder variables
- [x]README explains what PM does, how to install, and lists all commands

## Out of Scope
Any real functionality. Shell only.

## Status
- **State:** done
- **Progress:** 7/7 tasks
- **Branch:** merged
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #1
