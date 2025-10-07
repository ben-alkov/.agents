# this command fixes GitHub issues. $ARGUMENTS is the GitHub issue to fix
<!-- markdownlint-disable ul-indent ol-prefix -->
Fix the GitHub issue specified in $ARGUMENTS by analyzing the problem,
implementing a solution, and creating a commit on a new branch.

## Pre-flight Checks

1. **Validate Issue**
   - Run `gh issue view $ARGUMENTS --json state,title,body,assignees,labels`
   - Verify issue is open (if closed, explain why you're stopping)
   - Check if PR already exists:
     `gh pr list --search "fixes #$ARGUMENTS" --json number,title`
   - If PR exists, report it and ask if user wants to continue

2. **Assess Feasibility**
   - Read the issue description completely
   - If unclear or missing critical info, list specific questions for the user
   - Identify if this is a bug fix, feature request, or other type
   - Confirm you understand what success looks like

## Context Gathering

3. **Understand Project Structure**
   - Use Grep to find relevant code files mentioned in the issue
   - Identify the testing framework (look for pyproject.toml, pytest.ini,
     Cargo.toml, etc.)
   - Check for linting/formatting tools (ruff, nox, rustfmt, etc.)
   - Look for CONTRIBUTING.md or similar docs for coding standards

4. **Locate Related Code**
   - Use Glob to find files matching patterns from the issue
   - Use Grep to search for functions, classes, or error messages mentioned
   - Read relevant files to understand current implementation
   - Identify dependencies and side effects

## Implementation

5. **Create Feature Branch**
   - Branch name format: `fix/issue-{number}-{short-description}`
   - Example: `fix/issue-42-null-pointer-bug`
   - Use `git checkout -b {branch-name}`

6. **Test Driven Development Workflow**

  6.1: Discover Testing Infrastructure

  - Identify test framework from step 3 (pytest, cargo test, etc.)
  - Use Glob to find test directories (tests/, `__tests__/`, spec/, etc.)
  - Examine existing test files to understand conventions:
    - Test file naming patterns
    - Assertion style
    - Fixture/mock patterns
  - Verify you can run tests: try the testing command from project docs or
    common patterns (pytest, npm test, cargo test)
  - Check for a venv, activate it if one exists, and only then try running the tests
  - If you cannot run the tests for *any* reason, report it and ask if user wants
    to continue

  6.2: RED - Write Failing Test

  - Create or modify test file following project conventions
  - Write a test that would pass if the issue were fixed
  - Test should be minimal and focused on the specific issue behavior
  - Run ONLY the new test to verify it fails for the expected reason
  - If test passes unexpectedly: re-examine your understanding of the issue
  - Commit the test with message: "Add failing test for #{issue_number}"

  6.3: GREEN - Minimal Implementation

  - Make the smallest code change that makes your new test pass
  - Do not fix other issues or improve unrelated code
  - Run the new test again to verify it now passes
  - Run the full test suite to ensure no regressions
  - If any tests fail: debug and fix before proceeding

  6.4: REFACTOR - Improve Code Quality

  - Review your implementation for:
    - Code duplication
    - Unclear variable names
    - Overly complex logic
  - Apply project coding standards from step 3
  - After each refactor: re-run tests to ensure they still pass
  - If no refactoring needed: proceed to step 7

7. **Quality Checks**
   - Run linter if project has one (from step 3)
   - Run type checker if applicable (TypeScript, mypy, etc.)
   - Use IDE diagnostics to verify no new errors introduced
   - If checks fail: Fix issues before proceeding

## Finalization

8. **Verify Fix**
   - Confirm changes actually solve the problem stated in the issue
   - Check for edge cases mentioned in issue comments
   - If fix is incomplete or creates new problems, report this to user

9. **Commit Changes**
    - Stage only files related to the fix
    - Commit message format:

      ```text
      Fix #{issue_number}: {brief description}

      {explanation of what was changed and why}
      {mention any trade-offs or limitations}

      Fixes #{issue_number}
      ```

    - Use atomic commits: each commit should be independently testable
    - DO NOT push or create PR (user will do this separately)

10. **Summary Report**
    - List files changed
    - Describe the fix implemented
    - Confirm tests pass (or explain testing approach if no tests)
    - Note the branch name for user reference
    - Mention any follow-up work needed

## Error Handling

- **Issue not found**: Report error and ask user to verify issue number
- **Insufficient permissions**: Ask user to run `gh auth status` and re-authenticate
  if needed
- **Cannot determine fix**: Explain what's unclear and ask for guidance
- **Tests fail after fix**: Report failures, ask if user wants to debug or
  revert
- **Multiple possible solutions**: Present options and ask user to choose
- **Breaking changes required**: Warn user and confirm before proceeding

## Success Criteria

The task is complete when:

- Issue is confirmed open and valid
- Root cause is identified
- Fix is implemented following project standards
- All existing tests pass (or manual verification confirms fix works)
- No new linter/type errors introduced
- Changes are committed on a feature branch
- User receives clear summary of what was done
