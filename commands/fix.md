---
description: Diagnose problems and get exact commands to fix them
---

# /pm:fix

Diagnoses problems and gives you exact commands to fix them. Works for build errors, test failures, plugin issues, git problems, and environment issues.

## Process

1. Use the **project-state** skill to read the current project context
2. Use the **troubleshooting** skill to diagnose and prescribe

## Flow

### Step 1: Ask what's wrong

"What's happening? Paste the error or describe the problem."

### Step 2: Gather context

Read automatically (user doesn't need to provide these):
- Active package from STATUS.md
- Tech stack from STANDARDS.md / PROJECT.md
- Recent git changes via `git diff --stat` and `git log --oneline -5`
- Relevant decisions from DECISIONS.md

### Step 3: Diagnose

Match against common failure patterns:
- **Plugin issues** — validation errors, missing commands, broken frontmatter
- **Build errors** — missing deps, import errors, config issues
- **Test failures** — missing fixtures, environment issues, wrong assertions
- **Git problems** — conflicts, detached HEAD, push rejections
- **Environment** — services not running, permissions, ports in use
- **Hangs** — blocking calls, infinite loops, missing responses

If the description is too vague → ask a clarifying question before diagnosing.

If the problem doesn't match any pattern → say "I need more information" and ask specific questions.

### Step 4: Prescribe

Give exact fixes:
- Exact commands to run
- Exact file paths and edits
- Exact sequence of steps

Never give vague suggestions. Every fix is actionable.

### Step 5: Verify

Tell the user how to confirm it worked:

```
To verify:
  Run: [command]
  Expected: [result]
```

## Example

```
User: "Builder says the tests are failing on BP-03"

PM: Let me see the test output. Run:
  /dev:review

[User shares output]

PM: The auth middleware test is failing because the database
isn't initialized. The test setup is missing the schema creation step.

Tell Builder:
  "The test_auth.py file needs a setup fixture that creates the
  database tables before tests run. Add a conftest.py with a
  fixture that runs schema.sql."

To verify:
  Run: pytest tests/test_auth.py
  Expected: All tests pass
```
