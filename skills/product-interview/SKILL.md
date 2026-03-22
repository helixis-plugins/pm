---
name: product-interview
description: "Runs a structured product interview with 6 core questions and conditional follow-ups to capture everything needed for a product spec."
---

# Product Interview

This skill runs a structured interview to capture everything needed to write a product spec. It is invoked by `/pm:plan` when the project state is **empty**.

## Interview Rules

1. **One question at a time** — never batch multiple questions
2. **Acknowledge each answer** before asking the next question — show you understood
3. **Probe vague answers** — if the answer is too short, generic, or unclear, ask a follow-up before moving on
4. **Adapt to context** — fire conditional questions when relevant, skip them when not
5. **Hold everything in memory** — nothing is written to disk during the interview
6. **Present a summary** after all questions for user confirmation

## Core Questions

Ask these 6 questions in order. Every interview includes all of them.

### Question 1: The Elevator Pitch
> "What are we building? Give me the elevator pitch."

Capture:
- **Project name** — derive from the description if not stated explicitly
- **One-line description** — what it is in a single sentence
- **Category** — app, API, plugin, CLI tool, library, platform, etc.

Vague answer triggers:
- "Can you be more specific about what it does day-to-day?"
- "What's the one thing a user does with this that they can't do without it?"

### Question 2: The User
> "Who is this for? Describe the people who will use it."

Capture:
- **Primary persona** — role, context, what they care about
- **Secondary personas** — if mentioned
- **Scale hint** — one person, a team, public users

Vague answer triggers:
- "Is this just for you, or will others use it too?"
- "What does this person's day look like when they need this tool?"

### Question 3: Features
> "What does it need to do? List the main features."

Capture:
- **Feature list** — concrete capabilities, not abstract goals
- **Priority signals** — which features are must-have vs nice-to-have

Vague answer triggers:
- "Can you walk me through what a user does from start to finish?"
- "What are the three things it absolutely must do on day one?"

### Question 4: Tech Stack
> "What tech stack? Or should I recommend one?"

Capture:
- **Language(s)** — Python, TypeScript, Go, etc.
- **Framework(s)** — FastAPI, Next.js, Express, etc.
- **Database** — if applicable
- **Key dependencies** — external services, APIs, libraries
- **Deployment target** — if mentioned

If user says "recommend one":
- Ask what they're most comfortable with
- Consider the project type (web app → Next.js or FastAPI, CLI → Python or Node, plugin → markdown)
- Make a specific recommendation with brief rationale
- Confirm they're happy with it before proceeding

Vague answer triggers:
- "Any preference on language? What are you most comfortable building in?"
- "Will this need a database? A frontend?"

### Question 5: Scope
> "What's the scope? MVP, full product, or proof of concept?"

Capture:
- **Scope level** — MVP, full product, proof of concept, prototype
- **What "done" looks like** — when can they stop and use it

Vague answer triggers:
- "If you had to ship something usable in a week, what would it include?"
- "What's the difference between v1 and the full vision?"

### Question 6: Constraints
> "Any constraints? Deadlines, existing code, platform requirements?"

Capture:
- **Deadlines** — if any
- **Existing code** — starting from scratch or integrating with something
- **Platform requirements** — OS, browser, mobile, specific hosting
- **Budget constraints** — free tier only, specific providers
- **Other limitations** — compliance, accessibility, performance targets

If nothing: "None" is a valid answer — move on.

## Conditional Questions

After the core 6, evaluate the answers so far. Ask these **only when relevant**:

### Plugin Project
Trigger: answers mention "plugin", "Claude Code", "extension", "addon", or the project category is clearly a plugin.

> "Is this a Claude Code plugin, a Cowork plugin, or both? What namespace?"

### Auth / Billing
Trigger: answers mention "users", "accounts", "login", "sign up", "subscription", "payment", "billing", or the project involves multiple users with their own data.

> "Will it need authentication or billing? If billing, what provider — Stripe, Lemon Squeezy, or something else?"

Do NOT ask this for: CLI tools, local utilities, single-user tools, plugins.

### External APIs
Trigger: answers mention external data sources, third-party services, "API", "integration", or specific services like "Stripe", "OpenAI", "Twilio".

> "Which external APIs will it use? Do you already have API keys set up?"

### Magnus Black / Agent System
Trigger: answers mention "Magnus Black", "agent", "Amber", "Builder", "Dexter", "Atlas", "Sage", "Femi", "Sloane", "Lennox", or the Helixis agent ecosystem.

> "Which agent is this for? What folder does it live in?"

## Summary Presentation

After all questions (core + conditional), present a structured summary:

```
Here's what I've captured:

**Project:** [name]
**Description:** [one-line description]
**For:** [primary persona]
**Scope:** [MVP / full / PoC]

**Features:**
1. [feature]
2. [feature]
3. [feature]

**Tech Stack:** [language + framework + key deps]
**Constraints:** [deadlines, platforms, etc. or "None"]

Does this look right? Anything to add or correct?
```

## Confirmation Flow

- **User confirms** → proceed to spec writing (hand off to spec-writing skill)
- **User corrects** → update the relevant fields, re-present the summary
- **User adds** → incorporate the addition, re-present the summary
- **User says "looks good" / "yes" / "correct" / "approved"** → proceed

Re-present the summary after every correction until the user explicitly approves.

## Data Passed to Spec Writing

After approval, the following data is available in conversation memory for the spec-writing skill:

- project_name
- one_line_description
- primary_persona (and secondary if any)
- feature_list
- tech_stack (language, framework, database, dependencies, deployment)
- scope_level
- constraints
- conditional_answers (plugin details, auth/billing, APIs, agent info)
