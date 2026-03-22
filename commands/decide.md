---
description: Structure a decision with options, trade-offs, and a recommendation
---

# /pm:decide

Structures any decision with options, trade-offs, and a recommendation. Logs the result to DECISIONS.md.

## Process

1. Use the **decision-support** skill to run the decision flow

## Flow

### Step 1: Ask

"What do we need to decide?"

### Step 2: Structure options

For each viable option, present:
- Name and description
- Pros (2-4 concrete advantages)
- Cons (2-4 concrete disadvantages)
- Best when (the scenario where this option wins)

### Step 3: Recommend

Make a recommendation based on:
- Project scope (MVP vs full)
- Tech stack already chosen
- Known constraints
- Risk minimization

### Step 4: User chooses

Wait for the user to pick. Accept option name, number, or "go with your recommendation."

### Step 5: Log

Append to `.dev/DECISIONS.md` with auto-incrementing ID:

```markdown
## D-XXX: [Title]
- **Date:** [today]
- **Package:** BP-XX or "General"
- **Context:** [Why needed]
- **Decision:** [What was decided]
- **Rationale:** [Why this option]
- **Alternatives:** [What else considered]
- **Revisit when:** [Trigger or "N/A"]
```

### Step 6: Confirm

"Decision logged as D-XXX: [Title]"

## Auto-Detection

This skill also activates automatically during other PM commands when the user says things like:
- "should we use X or Y"
- "I'm not sure whether to..."
- "which is better"
- "X vs Y"

PM pauses, runs the decision flow, logs the result, then resumes the original command.

## Duplicate Check

Before structuring a new decision, check DECISIONS.md for related past decisions. If found, ask whether to revisit or keep the existing decision.

## Example

```
User: /pm:decide
PM: What do we need to decide?
User: Should we use SQLite or Postgres?

PM:
Option 1: SQLite
  File-based database, zero setup
  Pros:
    - No server to maintain
    - Perfect for MVP and single-user
    - Built into most frameworks
  Cons:
    - No concurrent write support
    - Hard to scale past ~10 simultaneous writers
  Best when: MVP, proof of concept, single-user tools

Option 2: Postgres
  Full relational database server
  Pros:
    - Production-grade, handles any scale
    - Rich query support, extensions
    - Standard for real deployments
  Cons:
    - Needs a server (Supabase free, Railway $5/mo)
    - More setup and config
  Best when: Multi-user apps, production deployment, complex queries

Recommendation: SQLite
Reason: This is an MVP — ship fast, switch to Postgres when you have users.

User: Go with SQLite

PM: Decision logged as D-001: Database choice
Recorded in .dev/DECISIONS.md.
```
