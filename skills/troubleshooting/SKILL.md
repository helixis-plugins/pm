---
name: troubleshooting
description: "Diagnoses build failures and problems by reading project context, matching common failure patterns, and prescribing exact fixes."
---

# Troubleshooting

This skill diagnoses problems and prescribes exact fixes. It reads project context, matches against common failure patterns, and gives the user specific commands and file edits to resolve the issue.

## Troubleshooting Flow

### Step 1: Problem intake

Ask: "What's happening? Paste the error or describe the problem."

Wait for the user to describe the issue. Accept:
- Error messages (pasted output)
- Symptom descriptions ("it hangs", "nothing happens", "wrong output")
- Behavioral issues ("Builder won't start", "tests fail")

If the description is too vague to diagnose:
- Ask a clarifying question before proceeding
- "Can you paste the exact error message?"
- "What command did you run?"
- "When does this happen — on startup, during build, or when testing?"

Do NOT guess. If you don't have enough information, say so.

### Step 2: Read project context

Gather context from the project to inform diagnosis:

1. **Active package** — read STATUS.md for the current package
2. **Package spec** — read the active package's spec to understand what's being built
3. **Tech stack** — read STANDARDS.md or PROJECT.md for the technology being used
4. **Recent changes** — run `git diff --stat` and `git log --oneline -5` to see what changed recently
5. **DECISIONS.md** — check for relevant past decisions that might affect the issue

### Step 3: Diagnose

Match the problem against common failure patterns. Work through the most likely causes first.

#### Plugin issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| "plugin.json is invalid" | Schema error in plugin.json | Check required fields: name, version, description, author (must be object) |
| "command not found" | Missing or misnamed command file | Verify file exists in commands/ with correct .md extension |
| "skill not loading" | Invalid YAML frontmatter | Check frontmatter has `---` delimiters, valid name and description fields |
| "plugin won't load" | Directory structure wrong | Verify `.claude-plugin/plugin.json` path is correct |

#### Build/code issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Import/module not found | Missing dependency or wrong path | Check package.json/requirements.txt, verify import path |
| Type errors | Wrong types in function call | Check function signature, verify argument types |
| "command not found" (shell) | Tool not installed or not in PATH | Check installation, verify PATH includes tool directory |
| Build fails silently | Missing config file | Check for .env, config files referenced in code |
| Compilation errors | Syntax error or version mismatch | Check error line number, verify language/framework version |

#### Test issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Tests fail on setup | Missing test fixtures or database | Check test setup/conftest, verify test database exists |
| Tests pass locally, fail in CI | Environment difference | Check env vars, dependencies, OS differences |
| Tests hang | Blocking call, infinite loop, or missing mock | Check for unmocked network calls, async issues |
| "No tests found" | Wrong test file naming or directory | Check test discovery pattern in config |

#### Git issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Merge conflicts | Divergent branches | List conflicting files, suggest resolution strategy |
| "detached HEAD" | Checked out a commit instead of branch | `git checkout [branch-name]` |
| "uncommitted changes" blocking operation | Dirty working tree | `git stash` or commit changes first |
| Push rejected | Remote has new commits | `git pull --rebase origin [branch]` then push |

#### Environment issues

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| "connection refused" | Service not running | Check if database/server is started |
| "permission denied" | File permissions or auth | Check file permissions, verify credentials |
| "out of memory" | Process too large | Check for memory leaks, increase limits |
| "port already in use" | Another process on the port | Find and kill: `lsof -i :[port]` |

#### "It just hangs"

1. Check for blocking network calls without timeouts
2. Check for infinite loops (missing break condition)
3. Check for missing response/return in request handlers
4. Check for deadlocks in concurrent code
5. Check for interactive prompts waiting for input

### Step 4: Prescribe the fix

Give **exact** commands and file edits. Not vague suggestions.

**Good:**
```
Run this command:
  npm install express

Then add this to line 3 of src/server.ts:
  import express from 'express';
```

**Bad:**
```
You might need to install the missing dependency and update your imports.
```

Rules for prescriptions:
- Give exact commands to run
- Give exact file paths and line numbers when suggesting edits
- Give the exact sequence of steps
- If multiple fixes are possible, recommend the most likely one first
- If unsure, say "Try this first — if it doesn't work, let me know what happens"

### Step 5: Verification

Tell the user how to confirm the fix worked:

```
To verify:
  Run: [exact command]
  Expected: [what they should see]
```

Always include a verification step. The user should know immediately if the fix worked.

## When You Don't Know

If the problem doesn't match any pattern and you can't diagnose it:

1. Say: "I need more information to diagnose this."
2. Ask for:
   - The full error output (not just the last line)
   - The exact command that triggered it
   - Whether this worked before and what changed
3. Do NOT guess or suggest generic fixes

## Cross-command Integration

During any PM command, if the user mentions a problem:
- Pause the current flow
- Run the troubleshooting flow
- After the fix, resume the original command

## What NOT to Do

- Do NOT write code fixes yourself — prescribe them for the user or Builder
- Do NOT run Builder — tell the user what to tell Builder
- Do NOT suggest "try restarting" as a first step — diagnose the root cause
- Do NOT give vague advice — every fix must be exact and actionable
- Do NOT ignore the project context — the tech stack and active package matter
- Do NOT assume — if you need more info, ask
