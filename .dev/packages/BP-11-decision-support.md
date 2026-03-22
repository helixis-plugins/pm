# BP-11: Decision Support (Standalone)

## Goal
`/pm:decide` structures any decision with options, trade-offs, and a recommendation — independent of the planning flow.

## Depends On
- BP-06 Decision Points — must be complete

## Involvement Mode
autopilot

## Deliverables
- [ ] commands/decide.md — full implementation
- [ ] skills/decision-support/SKILL.md — full implementation

## Tasks
1. Implement decision intake — "What do we need to decide?"
2. Implement option generation with pros/cons
3. Implement recommendation logic
4. Implement DECISIONS.md append with correct format and auto-incrementing ID
5. Implement auto-detection of decisions during other PM interactions

## Specifications

Explicit: User runs `/pm:decide` → PM asks what needs deciding → structures options → recommends → user chooses → logged.

Auto-detected: During any PM interaction, when user says something like "should we use X or Y" or "I'm not sure whether to..." → PM pauses, runs decision flow, then continues.

Decision format in DECISIONS.md:
```markdown
## D-XXX: [Title]
- **Date:** [today]
- **Package:** BP-XX (if applicable)
- **Context:** [Why]
- **Decision:** [What]
- **Rationale:** [Why this option]
- **Alternatives:** [What else considered]
- **Revisit when:** [Trigger or "N/A"]
```

ID auto-increments by reading existing entries in DECISIONS.md.

## Acceptance Criteria
- [ ] `/pm:decide` runs structured decision flow
- [ ] Options presented with pros/cons
- [ ] Recommendation made when appropriate
- [ ] Decision logged to DECISIONS.md with correct format
- [ ] ID auto-increments correctly
- [ ] Auto-detection works during other PM commands
- [ ] Works when DECISIONS.md doesn't exist yet (creates it)

## Out of Scope
Decision dependencies, reversal tracking.

## Status
- **State:** not_started
- **Progress:** 0/5 tasks
- **Branch:** —
- **Started:** —
- **Completed:** —
