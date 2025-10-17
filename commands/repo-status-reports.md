---
description: this command generates a repository health report with structure,
quality, and coverage analysis
allowed-tools: [Read, Glob, Grep, Write]
argument-hint: [quick|full]
---

# /repo-status-reports

Generate a health report analyzing structure, code quality, and test coverage.

Use `quick` for structure-only analysis, `full` (default) for comprehensive review.

## Analysis Areas

**Repository Structure**

- List project components (apps, packages, modules, services)
- Identify configuration files and tooling setup

**Code Quality**

- Search for uncommitted TODO/FIXME/HACK markers (should not exist in committed code)
- Identify source files without corresponding test files
- Note files exceeding reasonable size thresholds (>500 lines)

**Test Coverage**

- Map source files to test files (by naming convention)
- Flag untested modules

**Dependency Health** (quick mode: skip)

- Provide commands for user to check outdated dependencies:
  - Node: `npm outdated` or `yarn outdated`
  - Python: `pip list --outdated`
  - Other: Tool-specific commands

**Linting/Type Checking** (quick mode: skip)

- Provide commands for user to run:
  - Linters (eslint, pylint, clippy, etc.)
  - Type checkers (tsc, mypy, etc.)

## Deliverable

Write `repo_health.md` to repository root containing:

1. **Executive Summary:** 3-5 bullet points highlighting critical findings
2. **Detailed Findings:** Organized by category above
3. **Action Items:** Prioritized list (HIGH/MEDIUM/LOW) with specific next steps
4. **Commands to Run:** List heavyweight commands for user to execute separately

Format action items as checkboxes for easy tracking.
