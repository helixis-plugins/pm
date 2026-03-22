---
name: project-state
description: "Detects the current project state (empty, planned, mid-build, complete) by reading .dev/ directory, STATUS.md, and package files."
---

This skill is not yet implemented. It will detect project state by:

1. Checking if `.dev/` directory exists
2. Scanning `.dev/packages/` for BP-*.md files
3. Reading the State field from each package
4. Determining overall state: empty, planned, mid-build, or complete
5. Handling edge cases: missing STATUS.md, malformed packages, empty packages/
