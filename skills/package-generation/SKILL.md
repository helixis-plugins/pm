---
name: package-generation
description: "Breaks an approved product spec into build packages with correct dependencies, matching the dev plugin's exact format."
---

# Build Package Generation

This skill breaks an approved product spec into build packages that match the `dev` plugin's exact format. It is invoked by `/pm:plan` after the spec is approved.

## Input

The approved product spec must be available in conversation memory. The skill reads:

- Core Capabilities — each capability becomes one or more packages
- Technical Architecture — informs deliverables and tech-specific tasks
- What This Does NOT Do — helps define Out of Scope per package
- Future (v2+) — becomes post-MVP packages if appropriate

## Generation Process

### Step 1: Analyze the spec

Read the approved spec and identify discrete units of work:

1. **List every capability** from Core Capabilities
2. **Group related capabilities** that share code or must be built together
3. **Identify infrastructure needs** — database, auth, API setup, config
4. **Identify integration points** — where capabilities connect
5. **Note post-MVP items** from the Future section

### Step 2: Create the package list

Apply these structural rules:

| Rule | Detail |
|------|--------|
| First package | Always **Foundation / Skeleton** — project setup, config, base structure |
| Last MVP package | Always **Integration Test** — end-to-end verification |
| Post-MVP | Clearly separated, numbered after the integration test |
| Package count | 5-15 for typical projects |
| Package size | Each completable in 1-2 sessions |
| Dependencies | Explicit, acyclic — no circular references |

### Step 3: Order by dependencies

Build a dependency graph:

1. Foundation has no dependencies
2. Each subsequent package depends on what it needs to exist first
3. Independent packages can share the same dependency level
4. Integration test depends on all MVP packages
5. Verify: no circular dependencies exist

### Step 4: Generate the package map

Create a visual dependency map:

```
BP-01  Foundation                    ← No dependencies
BP-02  [Capability A]               ← Depends on BP-01
BP-03  [Capability B]               ← Depends on BP-01
BP-04  [Capability C]               ← Depends on BP-02
BP-05  [Integration]                ← Depends on BP-02, BP-03, BP-04
BP-06  [Post-MVP Feature]           ← Depends on BP-05
```

### Step 5: Generate each package

For every package, use the `templates/build-package.md` template and fill:

**Name:** Short, descriptive — what it builds, not what it does

**Goal:** One paragraph explaining what this package produces and why

**Depends On:** List of BP-XX references with names, or "None"

**Involvement Mode:** Recommend based on complexity:
- `autopilot` — straightforward plumbing, config, CRUD, boilerplate
- `narrator` — standard features with some decisions (default)
- `guided` — complex logic, unfamiliar tech, important architecture
- `pair` — critical systems, security, novel algorithms

**Deliverables:** Specific file paths with descriptions
- Use realistic paths matching the tech stack conventions
- Every file the package creates or modifies
- Format: `- [ ] path/to/file — what it is`

**Tasks:** Auto-generated from deliverables and goal
- Typically one task per deliverable, plus setup and testing
- Each task completable in one sitting
- Numbered sequentially
- Include a verification/testing task at the end

**Acceptance Criteria:** Testable conditions
- Specific and verifiable — not vague
- Include both positive cases ("X works") and edge cases where relevant
- Format: `- [ ] criterion`

**Out of Scope:** What this package explicitly does NOT do
- Features that belong in later packages
- Edge cases deferred to future work
- Format: `- item`

**Status:** Always initialized as:
```
- **State:** not_started
- **Progress:** 0/[N] tasks
- **Branch:** —
- **Started:** —
- **Completed:** —
```

### Step 6: Present to user

Present the packages in two parts:

**Part 1: Package Map**
Show the visual dependency map from Step 4.

**Part 2: Package Summaries**
For each package, show:
```
BP-XX: [Name]
  Goal: [one sentence]
  Depends on: [list]
  Mode: [mode]
  Deliverables: [count] files
  Tasks: [count]
```

Then ask:

> "Here's the package breakdown. Want to adjust anything? You can:
> - Add a package
> - Remove a package
> - Split a package into two
> - Merge two packages
> - Change dependencies
> - Change the order"

### Step 7: Handle revisions

If the user requests changes:

**Add a package:**
1. Ask what it should contain
2. Determine where it fits in the dependency chain
3. Generate the full package spec
4. Update the map
5. Re-present

**Remove a package:**
1. Check if other packages depend on it
2. If yes — reassign those dependencies
3. Remove the package
4. Renumber if needed
5. Re-present

**Split a package:**
1. Ask how to divide the deliverables
2. Create two packages from one
3. Update dependencies — second part depends on first
4. Renumber subsequent packages
5. Re-present

**Merge two packages:**
1. Combine deliverables, tasks, acceptance criteria
2. Use the broader goal
3. Dependencies = union of both
4. Remove the second package
5. Renumber if needed
6. Re-present

**Change dependencies:**
1. Update the specified dependency
2. Verify no circular dependencies
3. Re-present

Re-present the full map and summaries after every change. Repeat until user approves.

### Step 8: Approval

On approval:
1. Confirm: "Packages approved. [N] MVP packages + [M] post-MVP."
2. Hold all package specs in memory — they become `.dev/packages/BP-XX-name.md` during scaffolding
3. Hand off to decision point detection (BP-06)

## Decision Point Detection

After packages are approved, scan the spec and packages for decisions that must be resolved before building begins.

### Detection Triggers

Scan the approved spec and generated packages for these patterns:

| Trigger | Example | What to ask |
|---------|---------|-------------|
| Multiple DB options | "SQLite or Postgres", "needs a database" without specifying | "Which database?" |
| Deployment unspecified | No deployment target in Technical Architecture | "Where will this be deployed?" |
| Auth approach unclear | "needs auth" without specifying method | "What auth approach?" |
| Email/notification provider needed | Features mention emails, notifications | "Which email provider?" |
| Domain name needed | Web app without domain specified | "Do you have a domain?" |
| API alternatives | Multiple services could fulfill a need | "Which service?" |
| User said "recommend one" | During interview, deferred a choice | Surface the deferred choice now |

### Detection Process

1. Read through the approved spec section by section
2. Read through each generated package's deliverables and tasks
3. Flag anything that requires a choice between alternatives
4. Flag anything that is underspecified and will block a builder
5. If zero triggers found → report "No decisions needed" and skip to scaffolding

### Resolution Flow

If decisions are found:

1. Present all decisions as a table:
   ```
   Decisions to resolve before building:

   | # | Decision | Needed By | Recommendation |
   |---|----------|-----------|----------------|
   | 1 | [what] | BP-XX | [recommendation] |
   ```

2. Resolve one at a time:
   - State the decision clearly
   - Present 2-4 options with pros and cons
   - Make a recommendation with rationale when possible
   - Wait for the user to choose
   - Acknowledge the choice

3. After all decisions are resolved:
   - Confirm: "All decisions resolved."
   - Store all resolved decisions in memory
   - They become entries in `.dev/DECISIONS.md` during scaffolding
   - Format for each stored decision:
     ```
     title: [Decision Title]
     date: [today]
     package: BP-XX
     context: [Why this decision was needed]
     decision: [What was decided]
     rationale: [Why this option]
     alternatives: [What else was considered]
     revisit_when: [Trigger or "N/A"]
     ```

4. Hand off to scaffolding

## File Naming

Derive the filename from the package name:
- `BP-XX-short-name.md`
- Lowercase, hyphens, max 30 characters for the short-name portion
- Examples: `BP-01-foundation.md`, `BP-04-stripe-integration.md`, `BP-08-integration-test.md`

## What NOT to Do

- Do NOT create packages smaller than 2 tasks — merge with an adjacent package
- Do NOT create packages larger than ~15 tasks — split into two
- Do NOT create circular dependencies
- Do NOT write files to disk — packages stay in memory until scaffolding
- Do NOT skip the integration test package
- Do NOT mix MVP and post-MVP work in the same package
- Do NOT proceed without explicit user approval
