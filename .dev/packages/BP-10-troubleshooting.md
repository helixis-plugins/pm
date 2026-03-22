# BP-10: Troubleshooting

## Goal
`/pm:fix` diagnoses problems and gives exact commands to resolve them.

## Depends On
- BP-02 Project State Detection — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/fix.md — full implementation
- [x] skills/troubleshooting/SKILL.md — full implementation

## Tasks
1. Implement problem intake — ask what's happening, accept error output
2. Implement context reading — active package, tech stack, recent changes
3. Implement diagnosis patterns for common failure modes
4. Implement fix prescription — exact commands, exact file edits
5. Implement verification steps — how to confirm the fix worked

## Specifications

`/pm:fix` flow:
1. "What's happening? Paste the error or describe the problem."
2. Read context: active package spec, STANDARDS.md, recent `git diff --stat`
3. Diagnose based on: error message, tech stack, what's being built, common patterns
4. Prescribe: exact commands to run, exact file changes, exact sequence
5. Verify: "Run [command] to confirm it's fixed."

Common diagnosis patterns:
- Plugin validation errors → check plugin.json schema, skill frontmatter YAML
- Import errors → missing dependency, wrong path
- Test failures → missing setup, wrong assertion, environment issue
- Git conflicts → which files, how to resolve
- Build failures → missing config, wrong version, dependency mismatch
- API errors → auth issue, wrong endpoint, missing env var
- "It just hangs" → check for blocking calls, infinite loops, missing responses

PM should ask clarifying questions if the problem description is too vague before diagnosing.

## Acceptance Criteria
- [ ] `/pm:fix` asks for problem description
- [ ] Reads project context (active package, tech stack)
- [ ] Diagnoses common failure patterns
- [ ] Gives exact commands/fixes — not vague suggestions
- [ ] Includes verification step
- [ ] Asks clarifying questions for vague problem descriptions
- [ ] Doesn't guess — says "I need more info" when insufficient context

## Out of Scope
Automatically fixing code, running Builder.

## Status
- **State:** review
- **Progress:** 5/5 tasks
- **Branch:** bp/10-troubleshooting
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #19
