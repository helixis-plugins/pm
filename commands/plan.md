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

**Phase 5: Scaffold**

After all decisions resolved, use the **scaffold-creation** skill:

1. Check for existing `.dev/` — warn and ask confirmation if it exists
2. Create `.dev/` with `packages/` subdirectory
3. Write PROJECT.md from approved spec
4. Write all package files to `.dev/packages/`
5. Write STATUS.md with package overview table
6. Write DECISIONS.md with resolved decisions
7. Write STANDARDS.md based on tech stack
8. Write LEARNING.md and PARKED.md from templates
9. Write CLAUDE.md with session start protocol (append if exists)
10. Write README.md with project info
11. Initialize git, stage all files, commit
12. Create GitHub repo if `gh` available (silent, optional)
13. Present handoff confirmation:
    ```
    Project ready: [Name]
    Location: [pwd]
    Packages: [N] ([M] MVP + [K] post-MVP)
    First: BP-01 [name]

    To start building:
      builder dev [project-name]
    ```

### State: Planned

All packages exist but none have started.

1. Present the current plan: project name, package count, package list
2. Ask: "Want to revise the plan, or start building?"
3. If revise → enter revision mode (see below)
4. If build → suggest: "Run `/dev:package start BP-01` to begin."

### State: Mid-build

Some packages are done or in progress. Enter revision mode.

1. Read all package files and categorize:
   - **Done** — state `done` (protected, cannot modify)
   - **In Progress** — state `in_progress` (warn before modifying)
   - **Pending** — state `not_started` (freely modifiable)
2. Read PARKED.md for items that could become packages
3. Present the current state:
   ```
   Project: [Name]

   Done (locked):
     BP-01 Foundation ✓
     BP-02 Auth ✓

   In Progress:
     BP-03 Dashboard (3/6 tasks)

   Pending (editable):
     BP-04 Email Notifications
     BP-05 Integration Test

   Parked: [count] items
   ```
4. Ask: "What do you want to change?"
5. Handle the requested revision (see Revision Operations below)
6. After each change:
   - Refresh STATUS.md package overview table
   - Update any affected package files
   - Commit: `[PM] Revised: [description of change]`
7. Ask: "Anything else to change?" — repeat until user is done

#### Revision Operations

**Add a package:**
1. Ask what the new package should contain
2. Determine the correct position in the dependency chain
3. Generate a full package spec using the **package-generation** skill format
4. Assign the next available BP number
5. Write the package file to `.dev/packages/`
6. Update STATUS.md

**Remove a package:**
1. Verify the package state is `not_started` — refuse if `done`, warn if `in_progress`
2. Check if other packages depend on it
3. If dependencies exist → reassign them to the removed package's dependencies
4. Delete the package file from `.dev/packages/`
5. Update STATUS.md

**Split a package:**
1. Verify state is `not_started` or `in_progress` (warn for in_progress)
2. Ask how to divide the deliverables and tasks
3. Create two new packages from the original
4. Second part depends on first part
5. Reassign dependencies from the original to the appropriate new package
6. Remove the original, write two new files
7. Renumber subsequent packages if needed
8. Update STATUS.md

**Merge two packages:**
1. Verify both are `not_started` — refuse if either is `done`
2. Combine deliverables, tasks, and acceptance criteria
3. Use the broader goal
4. Dependencies = union of both packages' dependencies
5. Remove the second package file
6. Update the first package file with merged content
7. Reassign any packages that depended on the second to depend on the first
8. Update STATUS.md

**Re-plan pending packages:**
1. Collect all `not_started` packages
2. Present them and ask what should change
3. Regenerate using the **package-generation** skill
4. Preserve all `done` packages unchanged
5. Write updated package files
6. Update STATUS.md

**Promote parked item to package:**
1. Read PARKED.md, list items
2. User picks an item
3. Generate a package spec for it
4. Assign the next available BP number
5. Insert at the correct dependency position
6. Write the package file
7. Remove the item from PARKED.md
8. Update STATUS.md

#### Revision Rules

- State `done` → **NEVER** modify — "This package is complete and locked."
- State `in_progress` → **WARN** before modifying — "This package is in progress. Changing it may affect current work. Continue?"
- State `not_started` → modify freely
- After any revision, verify no orphaned dependencies (every dependency must point to an existing package)
- After any revision, verify no circular dependencies

### State: Complete (implemented in BP-14)

All packages are done.

1. Present completion summary
2. Offer three options: v2 planning, template saving, post-mortem
