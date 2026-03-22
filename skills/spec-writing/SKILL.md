---
name: spec-writing
description: "Generates a full product spec from interview answers using the product-spec template, with revision and approval flow."
---

# Spec Writing

This skill generates a full product spec from the interview answers captured by the **product-interview** skill. It is invoked by `/pm:plan` after the interview summary is approved.

## Input

The following data must be available in conversation memory from the completed interview:

- project_name
- one_line_description
- primary_persona (and secondary if any)
- feature_list
- tech_stack (language, framework, database, dependencies, deployment)
- scope_level
- constraints
- conditional_answers (plugin details, auth/billing, APIs, agent info)

## Spec Generation Process

### Step 1: Generate the spec

Use the template format from `templates/product-spec.md`. Fill every section from the interview data. Follow these rules:

**Section: What This Is**
- One paragraph
- State what the product is, what it does, and its primary value
- Derived from: project_name, one_line_description, feature_list

**Section: Who It's For**
- List each persona with a description of their role and what they care about
- Derived from: primary_persona, secondary personas

**Section: The Core Problem**
- What problem this product solves
- Why the current situation is painful or insufficient
- Derived from: one_line_description, feature_list, constraints
- Do NOT invent problems the user didn't describe — infer from what they said

**Section: The Solution**
- How the product works from the user's perspective
- Walk through the primary user flow
- Derived from: feature_list, tech_stack

**Section: Core Capabilities**
- Numbered list with details for each capability
- Each capability should map to one or more features from the interview
- Include enough detail that a builder knows what to implement
- Derived from: feature_list

**Section: Technical Architecture**
- **Be specific** — name exact frameworks, libraries, databases, deployment targets
- Never write generic statements like "modern web framework" or "scalable database"
- Include: language, framework, database, key dependencies, deployment target
- Derived from: tech_stack, conditional_answers

**Section: What This Does NOT Do**
- This section is **mandatory** — never skip it
- List explicit boundaries to prevent scope creep
- Include things the user might assume are included but aren't
- Include things deferred to v2
- Derive from: scope_level, constraints, feature_list (what's missing)
- Minimum 3 items

**Section: Future (v2+)**
- Things intentionally deferred
- Ideas mentioned during the interview that were out of scope
- Derive from: scope_level, constraints, conditional_answers, any "nice to have" mentions

### Step 2: Present the spec

Present the complete spec in the conversation. Format it exactly as it will appear in `.dev/PROJECT.md`.

After presenting, ask:

> "Review the spec above. Want to change anything, or does it look good?"

### Step 3: Handle revisions

If the user requests changes:

1. Acknowledge the requested change
2. Revise the affected section(s)
3. Re-present the **full spec** (not just the changed section)
4. Ask again: "How does this look now?"

Repeat until the user approves.

Common revision requests:
- "Add [feature] to capabilities" → add to Core Capabilities and update Solution if needed
- "Change the stack to [X]" → update Technical Architecture and any affected capabilities
- "Remove [item]" → remove from relevant section, consider adding to "Does NOT Do"
- "The problem isn't quite right" → rewrite The Core Problem based on clarification
- "Add [thing] to future" → add to Future (v2+)

### Step 4: Approval

The user approves when they say any of:
- "looks good" / "approved" / "yes" / "ship it" / "go ahead"
- Any clear affirmative without a change request

On approval:
1. Confirm: "Spec approved. Moving to package generation."
2. Hold the approved spec in memory — it becomes `.dev/PROJECT.md` during scaffolding
3. Hand off to the **package-generation** skill (BP-05)

## Quality Checks

Before presenting the spec, verify:

- [ ] Every section has content — no empty sections, no placeholder text
- [ ] Technical Architecture names specific tools — no generic language
- [ ] "What This Does NOT Do" has at least 3 items
- [ ] Features in Core Capabilities match what the user described — nothing invented
- [ ] The spec is self-consistent — Solution matches Capabilities, Architecture supports both

If any check fails, fix the issue before presenting.

## What NOT to Do

- Do NOT invent features the user didn't mention
- Do NOT add technical components the user didn't ask for
- Do NOT use generic architecture descriptions ("scalable", "modern", "cloud-native")
- Do NOT skip the "Does NOT Do" section
- Do NOT write to disk — the spec stays in memory until scaffolding
- Do NOT proceed to packages without explicit user approval
