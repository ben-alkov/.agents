---
description: this command creates a commit and PR plan for any current changes
allowed-tools: Bash, Write, Read
---

# /commit-and-pr-plans

## Gather context

Run git commands to understand changes:

```bash
git status
git diff HEAD
git log --oneline -10
git log $(git merge-base HEAD origin/main 2>/dev/null || echo HEAD)..HEAD --oneline
```

If `nox` exists in current directory, run it. Stop if checks fail.

## Create plan

Write plan to `draft-commit-pr-plan.md` at repository root (use `git rev-parse
--show-toplevel` to resolve path).

Follow this structure:

```markdown
    # Branch: <type>/<brief-description>

    Format: feat|fix|refactor|docs|test|chore / 2-4 hyphenated words

    ## Commits

    ### 1. <type>(<scope>): <description>

    **Files:**
    - `path/to/file.py` (modified)
    - `tests/test_file.py` (added)

    **Rationale:** Why these files belong in one commit

    (Repeat for each proposed commit)

    ## PR Description

    **Summary:** 2-3 sentence overview of all changes

    **Rationale:** Why these changes were needed

    **Implementation:** Key technical decisions or approaches

    **Testing:** How to verify the changes work

    **Breaking Changes:** None / description of incompatible changes
```

## Guidelines

- Group changes by logical purpose, not file type
- Include tests with the code they verify
- Each commit must be revertable without breaking builds
- Use Conventional Commits format (see git_ops.md)
- If no uncommitted changes exist, report this without creating plan file
- If changes are committed but not pushed, plan only the PR description
