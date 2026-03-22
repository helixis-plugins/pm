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

**Phase 2: Spec Writing**

After interview approval, use the **spec-writing** skill:

1. Generate the full product spec from interview answers using `templates/product-spec.md`
2. Fill every section — no empty sections, no placeholders, no invented content
3. Technical Architecture must name specific tools (not "modern framework")
4. "What This Does NOT Do" section is mandatory — minimum 3 items
5. Present the complete spec in conversation
6. Ask: "Review the spec above. Want to change anything, or does it look good?"
7. If user requests changes → revise, re-present the full spec, ask again
8. Repeat until user approves
9. On approval: "Spec approved. Moving to package generation."
10. Hold approved spec in memory — it becomes `.dev/PROJECT.md` during scaffolding

**Phase 3: Package Generation**

After spec approval, use the **package-generation** skill:

1. Analyze spec capabilities → group into packages
2. Always start with a Foundation/Skeleton package (BP-01)
3. Always end MVP with an Integration Test package
4. Separate post-MVP packages clearly
5. Order by dependencies — no circular references
6. Generate a visual package map showing the dependency tree
7. Generate each package from `templates/build-package.md` with:
   - Specific file paths in deliverables
   - Auto-generated tasks (one per deliverable + setup/testing)
   - Testable acceptance criteria
   - Recommended involvement mode
8. Present the map + package summaries
9. User can add/remove/split/merge packages → revise and re-present
10. Repeat until user approves
11. On approval: "Packages approved. [N] MVP + [M] post-MVP."
12. Hold packages in memory — written to `.dev/packages/` during scaffolding

**Phase 4: Decision Points**

After packages are approved, scan the spec and packages for unresolved decisions:

1. Scan for decision triggers:
   - Multiple database options viable (e.g., "SQLite or Postgres")
   - Deployment target unspecified
   - Auth approach unclear
   - Email/notification provider needed
   - Domain name needed
   - API alternatives exist
   - User said "recommend one" during interview
2. If no decisions needed → skip to Phase 5
3. If decisions found, present them as a table:
   ```
   Decisions to resolve before building:

   | # | Decision | Needed By | Recommendation |
   |---|----------|-----------|----------------|
   | 1 | Database: SQLite vs Postgres | BP-03 | SQLite (MVP) |
   | 2 | Deployment target | BP-06 | Railway |
   ```
4. Resolve one at a time:
   - Present options with pros/cons
   - Make a recommendation when appropriate
   - User chooses → store in memory
5. After all resolved: "All decisions resolved. Scaffolding the project."
6. Hold resolved decisions in memory — written to `.dev/DECISIONS.md` during scaffolding

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
