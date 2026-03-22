# Coding Standards

## Project Type
Claude Code plugin — primarily Markdown files with structured frontmatter and templates. No compiled code.

## File Naming Conventions
- Plugin commands: `lowercase.md` in `commands/`
- Skills: `kebab-case/` directories in `skills/`, each containing `SKILL.md`
- Templates: `kebab-case.md` or `UPPERCASE.md` in `templates/`
- Build packages: `BP-XX-short-name.md` in `.dev/packages/`
- Plugin config: `plugin.json` in `.claude-plugin/`

## Directory Structure
```
pm/
├── .claude-plugin/plugin.json
├── commands/           ← Slash command definitions
├── skills/             ← Reusable capability modules
│   └── skill-name/SKILL.md
├── templates/          ← File templates for scaffolding
├── CLAUDE.md           ← Agent instructions
└── README.md
```

## Markdown Formatting
- Use ATX-style headers (`#`, `##`, `###`)
- One blank line between sections
- Use fenced code blocks with language hints where applicable
- Tables: use pipes with header separator row
- Line length: no hard wrap — one sentence per line or natural wrapping

## Template Conventions
- Use `[placeholder]` for values the user provides
- Use `<!-- comments -->` for instructions that should not appear in output
- Templates must be valid markdown when placeholders are filled

## Skill Writing
- Each skill gets its own directory under `skills/`
- `SKILL.md` is the entry point — contains full instructions for the capability
- Skills should be self-contained: reading the SKILL.md alone should be enough
- Reference templates by relative path when a skill needs to generate files

## Command Writing
- Each command is a single `.md` file in `commands/`
- Commands should reference skills rather than duplicating logic
- Keep command files focused on the interaction flow; delegate knowledge to skills

## Git Conventions
- Branch naming: bp/XX-short-name (for build packages), feature/name, fix/name
- Commit format: [BP-XX] description (when working in a build package)
- Commit after each completed task, not at the end of a session
- Never commit directly to main — always use branches and PRs

## Documentation
- README.md: installation, usage, slash command reference
- Each command and skill should be self-documenting via its markdown content
- CLAUDE.md: agent-facing instructions, not user-facing docs
