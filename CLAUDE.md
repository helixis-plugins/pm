# PM Plugin

## Session Start Protocol
1. Read .dev/STATUS.md for current project state
2. Read the active build package spec in .dev/packages/ (if one is active)
3. Run git status to check for uncommitted work
4. Report a brief status to the user before doing anything else

## Project Quick Reference
- **Project:** PM Plugin — Claude Code project lead plugin
- **Stack:** Markdown-based Claude Code plugin (commands, skills, templates, plugin.json)
- **Standards:** See .dev/STANDARDS.md

## Rules
- NEVER work on anything not defined in the active build package
- ALWAYS check .dev/STATUS.md before starting work
- ALWAYS update .dev/STATUS.md when completing tasks
- ALWAYS commit working code before ending a session
- If asked to do something out of scope, flag it and offer to: add to current package, create a new package, or skip
- When no build package is active, prompt the user to start one before writing any code
