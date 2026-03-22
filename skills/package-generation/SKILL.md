---
name: package-generation
description: "Breaks an approved product spec into build packages with correct dependencies, matching the dev plugin's exact format."
---

This skill is not yet implemented. It will:

1. Analyze spec capabilities and group into packages
2. Order by dependencies with foundation first and integration test last
3. Generate each package from the build-package template
4. Present package map for user review and revision
5. Detect decision points that need resolution
