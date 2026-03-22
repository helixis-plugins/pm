# PM Plugin

## Description
A Claude Code plugin that serves as a project lead — plans projects, guides builds, troubleshoots failures, makes technical decisions, tracks progress, and ensures quality. Everything that surrounds coding, handled by one PM agent.

## Target User
Primary: Helixis — anyone running named agents (Builder, Amber) through Claude Code who needs a project lead to plan work, monitor progress, troubleshoot blockers, and ensure quality without manually managing specs, build packages, and agent coordination.

Broader: Technical founders and product builders who use Claude Code's `dev` plugin and want a single entry point for planning, decisions, QA, troubleshooting, and guidance.

## Tech Stack
- Language: Markdown (Claude Code plugin — no traditional runtime)
- Framework: Claude Code plugin system (`.claude-plugin/plugin.json`, commands/, skills/)
- Key dependencies: Claude Code CLI, `dev` plugin (shared `.dev/` directory), `gh` CLI (optional, for GitHub repo creation)

## Starting Point
New project — starting from scratch

## Additional Context
- PM and Builder share the same `.dev/` directory as source of truth
- Launch system: `pm plan <project>` opens Claude Code with PM plugin in the project folder
- Integrates with existing `dev` plugin — PM writes plans, Builder executes them
- Environment variables: `PM_DEFAULT_ORG` (default: helixisops), `PM_DEFAULT_MODE` (default: autopilot)
- Plugin structure: commands (slash commands), skills (reusable capabilities), templates (file templates)

## Created
2026-03-22
