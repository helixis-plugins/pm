# BP-07: Project Scaffold

## Goal
Populate the current folder with `.dev/` structure, packages, CLAUDE.md, git — ready for Builder.

## Depends On
- BP-02 Project State Detection — must be complete
- BP-05 Build Package Generation — must be complete
- BP-06 Decision Points — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/scaffold.md — full implementation
- [x] skills/scaffold-creation/SKILL.md — full implementation
- [x] commands/plan.md — auto-scaffold after approval

## Tasks
1. Implement `.dev/` directory creation
2. Implement PROJECT.md writing from approved spec
3. Implement package file writing from approved packages
4. Implement STATUS.md generation with package table
5. Implement DECISIONS.md from resolved decisions
6. Implement STANDARDS.md generation by tech stack
7. Implement LEARNING.md from template
8. Implement PARKED.md from template (empty)
9. Implement CLAUDE.md generation
10. Implement README.md generation
11. Implement git init + first commit
12. Implement GitHub repo creation (optional, if `gh` available)
13. Implement overwrite warning if `.dev/` already exists
14. Implement handoff confirmation message

## Specifications

Writes into current working directory. Does NOT create a new folder. If `.dev/` exists → warn and ask confirmation.

STANDARDS.md by tech stack:
- Python → Black, type hints, docstrings, pytest
- Node/TypeScript → Prettier, ESLint, strict
- React/Next.js → Component conventions, Tailwind
- FastAPI → Endpoint naming, Pydantic, async
- Mixed → combine relevant standards

Git: `git init && git add . && git commit -m "Project setup: [name] — planned by PM plugin"`

GitHub: if `gh` available → `gh repo create $PM_DEFAULT_ORG/[name] --private --source=. && git push -u origin main`. If not → skip silently.

Handoff:
```
Project ready: [Name]
Location: [pwd]
Packages: [N] ([M] MVP + [K] post-MVP)
First: BP-01 [name]

To start building:
  builder dev [project-name]
```

## Acceptance Criteria
- [ ] `.dev/` created with all expected files
- [ ] All package files in `.dev/packages/`
- [ ] PROJECT.md contains approved spec
- [ ] STATUS.md has package table
- [ ] DECISIONS.md has resolved decisions
- [ ] STANDARDS.md appropriate for tech stack
- [ ] PARKED.md exists (empty)
- [ ] CLAUDE.md has session start protocol
- [ ] Git initialized with first commit
- [ ] GitHub repo created if `gh` available
- [ ] Warning if `.dev/` already exists
- [ ] Builder can launch and read the plan immediately

## Out of Scope
Writing any project code.

## Status
- **State:** review
- **Progress:** 14/14 tasks
- **Branch:** bp/07-project-scaffold
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #13
