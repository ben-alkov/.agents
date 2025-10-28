---
description: this command resolves the GitHub issue specified in $ARGUMENTS
allowed-tools: Bash, Read, Grep, Glob
argument-hint: <issue-number>
---

# /github-issue-fixes

Fix the GitHub issue in $ARGUMENTS by analyzing the problem, implementing a
solution, and committing changes on a feature branch.

## Validate Issue

Fetch issue: `gh issue view $ARGUMENTS --json state,title,body`

Stop if: issue is closed, requirements are unclear, or PR already exists (`gh pr
list --search "fixes #$ARGUMENTS"`). Report the blocking condition and ask how
to proceed.

## Understand Context

Use Grep/Glob to locate code referenced in the issue. Identify test framework
(pytest, cargo, jest), linting tools (ruff, eslint), and coding standards
(CONTRIBUTING.md).

If issue mentions specific errors, files, or functions: search for exact matches
first. Read related files to understand current implementation.

## Branch and Commit

Create branch: `fix/issue-{number}-{brief-description}`

Commit message: `fix: resolve #{issue_number} - {description}` with body
explaining changes. Include `Fixes #{issue_number}` footer.

Follow git_ops.md for staging and commit conventions.

## Test-Driven Development

Write a failing test that would pass if the issue were fixed. Run only this test
to verify failure. Implement the minimal fix to make it pass. Run the full test
suite to check for regressions. Refactor if needed while keeping tests green.

For Python projects: check for venv and activate before running tests.

Skip TDD only if: no test infrastructure exists, issue is documentation-only, or
testing is genuinely impractical. Explain why in these cases.

## Quality Verification

Run linters and type checkers discovered during context gathering. Use IDE
diagnostics to verify no new errors.

Confirm fix addresses the issue's specific problem, including edge cases
mentioned in comments.

## Summary

Report: files changed, fix description, test results, branch name, and any
follow-up work needed.

Do not push changes or create PR. User will handle this separately.

## Error Handling

- **Issue not found**: Verify issue number with user
- **Insufficient gh permissions**: Ask user to run `gh auth status`
- **Cannot determine fix**: Explain what's unclear, ask for guidance
- **Tests fail after implementation**: Report failures, offer to debug or revert
- **Multiple valid approaches**: Present options, let user choose
