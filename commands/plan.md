---
description: Plan a new project, revise an existing plan, or handle post-completion options
---

# /pm:plan

This is the main planning command. Its behaviour depends on the project state.

## Step 1: Detect Project State

Use the **project-state** skill to determine the current state of the project folder.

## Step 2: Route by State

### State: Empty

The folder has no `.dev/` directory or no package files. Run the full planning flow.

**Phase 1: Interview**

Use the **product-interview** skill to run the structured interview.

1. Greet the user briefly: "New project. Let's plan it."
2. Ask the 6 core questions **one at a time** — wait for each answer before asking the next:
   1. "What are we building? Give me the elevator pitch."
   2. "Who is this for? Describe the people who will use it."
   3. "What does it need to do? List the main features."
   4. "What tech stack? Or should I recommend one?"
   5. "What's the scope? MVP, full product, or proof of concept?"
   6. "Any constraints? Deadlines, existing code, platform requirements?"
3. After each answer, acknowledge it briefly before the next question
4. If an answer is vague, probe with a follow-up before moving on
5. After the core questions, check if conditional questions apply:
   - Plugin project → "Is this a Claude Code plugin, a Cowork plugin, or both? What namespace?"
   - Users/accounts mentioned → "Will it need authentication or billing?"
   - External APIs mentioned → "Which external APIs? Do you have keys set up?"
   - Magnus Black/agents mentioned → "Which agent? What folder?"
6. Present the interview summary
7. User confirms or corrects — re-present after corrections until approved

**Phase 2: Spec Writing** (implemented in BP-04)

After interview approval, hand off to the **spec-writing** skill.

**Phase 3: Package Generation** (implemented in BP-05)

After spec approval, hand off to the **package-generation** skill.

**Phase 4: Decision Points** (implemented in BP-06)

After packages are approved, identify and resolve decision points.

**Phase 5: Scaffold** (implemented in BP-07)

After all decisions resolved, scaffold the project using the **scaffold-creation** skill.

### State: Planned

All packages exist but none have started.

1. Present the current plan: project name, package count, package list
2. Ask: "Want to revise the plan, or start building?"
3. If revise → enter revision mode (implemented in BP-13)
4. If build → suggest: "Run `/dev:package start BP-01` to begin."

### State: Mid-build (implemented in BP-13)

Some packages are done or in progress.

1. Present current state (done / in-progress / pending)
2. Ask: "What do you want to change?"
3. Enter revision mode

### State: Complete (implemented in BP-14)

All packages are done.

1. Present completion summary
2. Offer three options: v2 planning, template saving, post-mortem
