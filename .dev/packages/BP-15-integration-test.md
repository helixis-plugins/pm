# BP-15: Integration Test

## Goal
Verify the full PM workflow end-to-end — plan a project, hand off to Builder, troubleshoot, revise, review, complete.

## Depends On
- All previous packages (BP-01 through BP-14)

## Involvement Mode
pair

## This is a manual test protocol, not a coding package.

## Test Script

```
TEST 1: Validation + Install
1. Run: claude plugin validate . (from pm directory)
2. Verify: passes with no errors
3. Run: claude --plugin-dir ./pm
4. Verify: all 7 /pm: commands appear, no errors

TEST 2: Full Planning Flow
1. Create: mkdir ~/test-pm-project && cd ~/test-pm-project
2. Launch: claude --plugin-dir ~/HelixisHQ/repos/github/helixisops/pm/
3. Run /pm:plan
4. Describe: "A FastAPI backend that processes invoice PDFs and stores
   line items in a database with a dashboard."
5. Complete the interview
6. Verify: spec generated, all sections populated
7. Request one spec change → verify revision
8. Approve spec
9. Verify: packages generated with correct format
10. Approve packages
11. Verify: decisions identified and resolved
12. Verify: project scaffolded in current folder
13. Verify: git initialized, .dev/ complete

TEST 3: Handoff to Builder
1. Exit PM
2. Launch: claude --plugin-dir ~/HelixisHQ/repos/github/helixisops/software-developer/
3. Run /dev:status → verify correct status
4. Run /dev:package list → verify all packages
5. Run /dev:package start BP-01 → verify Builder starts immediately

TEST 4: Progress Tracking
1. Exit Builder (after some work on BP-01)
2. Launch PM again
3. Run /pm:status
4. Verify: shows BP-01 in progress, pending packages, parked items (if any)

TEST 5: Build Guidance
1. Run /pm:guide
2. Verify: tells you what to do next, what to tell Builder
3. Verify: pre-requisites listed for next package
4. Verify: mode recommendation given

TEST 6: Troubleshooting
1. Run /pm:fix
2. Describe a fake problem: "Builder says plugin.json is invalid"
3. Verify: PM asks for details, reads context, suggests diagnosis
4. Verify: exact fix commands provided

TEST 7: Decision Support
1. Run /pm:decide
2. Present: "Should we use SQLite or Postgres?"
3. Verify: options with pros/cons presented
4. Verify: recommendation made
5. Choose one → verify logged in DECISIONS.md

TEST 8: Quality Assurance
1. Run /pm:review
2. Verify: checks run (validation, deliverables, tests, decisions)
3. Verify: results presented as checklist

TEST 9: Mid-Project Revision
1. Run /pm:plan (mid-build)
2. Verify: current state shown
3. Say: "Add email notifications"
4. Verify: new package created, dependencies correct
5. Say: "Remove BP-06"
6. Verify: removed, dependencies updated
7. Try to modify a done package → verify refused

TEST 10: Post-Completion
1. Manually mark all packages done
2. Run /pm:plan
3. Verify: complete state, three options shown
4. Choose "Save as template"
5. Verify: saved to ~/.dev-templates/

TEST 11: Parked Items
1. During any PM interaction, mention "we should add mobile later"
2. Verify: PM offers to park it
3. Run /pm:status → verify parked item appears
4. Run /pm:plan (revision mode) → verify can promote parked item to package
```

## Pass/Fail
- All 11 tests pass → PM plugin ready for daily use
- Any test fails → identify which BP needs a fix, patch, re-test

## Status
- **State:** in_progress
- **Progress:** 1/11 tests
- **Branch:** bp/15-integration-test
- **Started:** 2026-03-22
- **Completed:** —
- **Issue:** #29
