# BP-03: Product Interview

## Goal
`/pm:plan` in an empty folder runs a structured interview capturing everything needed for a product spec.

## Depends On
- BP-01 Plugin Skeleton + Templates — must be complete

## Involvement Mode
autopilot

## Deliverables
- [x] commands/plan.md — interview phase (replaces placeholder)
- [x] skills/product-interview/SKILL.md — full implementation

## Tasks
1. Implement 6 core interview questions in plan.md
2. Implement conditional question logic in the interview skill
3. Implement interview summary presentation
4. Implement summary confirmation and correction flow
5. Implement vague-answer follow-up probes

## Specifications

Core questions (always asked, one at a time):
1. "What are we building? Give me the elevator pitch."
2. "Who is this for? Describe the people who will use it."
3. "What does it need to do? List the main features."
4. "What tech stack? Or should I recommend one?"
5. "What's the scope? MVP, full product, or proof of concept?"
6. "Any constraints? Deadlines, existing code, platform requirements?"

Conditional questions (skill decides when to ask):
- Sounds like a plugin → "Claude Code, Cowork, or both? Namespace?"
- Mentions users/accounts → "Auth or billing needed?"
- Mentions external data → "Which APIs? Keys available?"
- Mentions Magnus Black/agents → "Which agent? What folder?"

Rules:
- One question at a time — never batch
- Acknowledge each answer before the next question
- Vague answers → follow-up probe before moving on
- After all questions → present summary
- User can correct summary → PM updates and re-presents
- User approves → proceed to spec writing

Answers held in conversation memory — nothing written to disk yet.

## Acceptance Criteria
- [ ] `/pm:plan` in empty folder starts interview with first question
- [ ] Questions asked one at a time
- [ ] Conditional questions fire when relevant
- [ ] Vague answers get follow-up probes
- [ ] Summary presented after all questions
- [ ] User can correct summary before proceeding
- [ ] Interview adapts — doesn't ask about billing for a CLI tool

## Out of Scope
Spec writing, packages, scaffolding.

## Status
- **State:** done
- **Progress:** 5/5 tasks
- **Branch:** merged
- **Started:** 2026-03-22
- **Completed:** 2026-03-22
- **Issue:** #5
