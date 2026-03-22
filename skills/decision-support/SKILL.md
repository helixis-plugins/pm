---
name: decision-support
description: "Structures decisions with options, trade-offs, and recommendations. Logs resolved decisions to DECISIONS.md with auto-incrementing IDs."
---

# Decision Support

This skill structures any decision with options, trade-offs, and a recommendation. It logs resolved decisions to DECISIONS.md with auto-incrementing IDs. It works both explicitly (via `/pm:decide`) and via auto-detection during other PM interactions.

## Explicit Decision Flow

### Step 1: Decision intake

Ask: "What do we need to decide?"

Wait for the user to describe the decision. Accept:
- Direct questions ("Should we use SQLite or Postgres?")
- Ambiguous situations ("I'm not sure how to handle auth")
- Trade-off descriptions ("We could go with X but Y is also an option")

### Step 2: Structure the options

For each viable option:

1. **Name** — clear, short label
2. **Description** — one sentence on what this option means
3. **Pros** — concrete advantages (2-4 items)
4. **Cons** — concrete disadvantages (2-4 items)
5. **Best when** — the scenario where this option wins

Present as:

```
Option 1: [Name]
  [Description]
  Pros:
    - [advantage]
    - [advantage]
  Cons:
    - [disadvantage]
    - [disadvantage]
  Best when: [scenario]

Option 2: [Name]
  ...
```

Typically 2-4 options. Never more than 5.

### Step 3: Make a recommendation

If you can recommend one option:

```
Recommendation: [Option name]
Reason: [one-sentence rationale tied to this project's context]
```

Base recommendations on:
- The project's scope (MVP vs full product)
- The tech stack already chosen
- Constraints from the interview
- What minimizes risk for the current phase

If you genuinely cannot recommend (truly equal options, depends entirely on preference):

```
No strong recommendation — this depends on your preference for [X vs Y].
```

This should be rare. Most decisions have a best option given the context.

### Step 4: User chooses

Wait for the user to pick an option. Accept:
- Option name or number
- "Go with your recommendation"
- "Let's do [X]"
- A modified version of an option

If the user picks a modified version, confirm the exact decision before logging.

### Step 5: Log to DECISIONS.md

#### Determine the next ID

Read `.dev/DECISIONS.md`. Find the highest existing `## D-XXX` entry. Increment by 1.

If no entries exist (file is empty or has only the template), start with D-001.

If DECISIONS.md doesn't exist, create it first.

#### Write the entry

Append to DECISIONS.md:

```markdown
## D-XXX: [Title]
- **Date:** [today's date]
- **Package:** BP-XX (if the decision relates to a specific package, otherwise "General")
- **Context:** [Why this decision was needed — one sentence]
- **Decision:** [What was decided — the chosen option]
- **Rationale:** [Why this option was chosen — one sentence]
- **Alternatives:** [What else was considered — comma-separated list]
- **Revisit when:** [Trigger for reconsidering, or "N/A"]
```

#### Determine the package

- If a package is currently active (from STATUS.md) → use that package
- If the decision was triggered during planning → use the relevant package from the discussion
- If general/not package-specific → use "General"

#### Determine revisit trigger

Common triggers:
- "When we have paying users" (for MVP shortcuts)
- "When performance becomes an issue" (for simple-first choices)
- "When requirements change" (for scope-dependent choices)
- "N/A" (for permanent decisions)

### Step 6: Confirm

```
Decision logged as D-XXX: [Title]
Recorded in .dev/DECISIONS.md.
```

## Auto-Detection

During any PM command, watch for decision signals in user messages:

**Trigger phrases:**
- "should we use X or Y"
- "I'm not sure whether to"
- "which is better"
- "what do you think about"
- "X vs Y"
- "do we need"
- "is it worth"

**When detected:**

1. Pause the current flow
2. Acknowledge: "That's a decision point. Let me structure it."
3. Run the decision flow (Steps 2-6)
4. After logging, resume the original command flow

**When NOT to trigger:**
- Rhetorical questions
- Questions about how PM works
- Requests for information (not choices)
- Questions already answered in DECISIONS.md

## Reading Existing Decisions

Before structuring a new decision, check DECISIONS.md for related past decisions. If a similar decision was already made:

```
This was already decided in D-XXX: [Title]
Decision: [what was decided]

Want to revisit it, or keep the existing decision?
```

## What NOT to Do

- Do NOT make decisions without user input — always present options and let them choose
- Do NOT log decisions the user didn't explicitly approve
- Do NOT present more than 5 options — narrow it down
- Do NOT give vague pros/cons — be specific and concrete
- Do NOT skip the recommendation — always have an opinion (unless truly equal)
- Do NOT duplicate existing decisions — check DECISIONS.md first
