# this command plans a commit and PR task

Plan commits and PR description for current changes.

## Prerequisites

### Verify checks pass

Run `nox` to verify all checks pass:

- If checks fail: stop and report failures - do not proceed with planning
- If nox is not available: skip this step and note it in the plan output

### Gather context

Run these commands to understand the changes:

- `git status` - identify staged/unstaged/untracked files
- `git diff HEAD` - see all uncommitted changes
- `git log --oneline -10` - review recent commit message style
- `git log $(git merge-base HEAD origin/main)..HEAD --oneline` - see commits
  since diverging from main (if applicable)

## Commit Requirements

Each commit must:

- Address one specific purpose (feature/fix/refactor/docs/test)
- Be atomic: if reverted, doesn't break builds or tests
- Include related test changes with the code they test
- Follow Conventional Commits format: `<type>(<scope>): <description>`

## Deliverable: Structured Plan

Write complete plan to `draft-commit-pr-plan.md` in git repository root
(resolve via `git rev-parse --show-toplevel`).

### Section 1: Branch Name

Propose single branch name for all changes using format:
`<type>/<brief-description>`

- type: feat|fix|refactor|docs|test|chore
- brief-description: 2-4 words, kebab-case
- Example: `feat/add-user-auth`

### Section 2: Commit Plan

List all proposed commits in order. For each commit show:

- Complete commit message (Conventional Commits format)
- File changes included (list files with change type: added/modified/deleted)
- Rationale for grouping these changes together

### Section 3: PR Description Draft

Include:

- **Summary**: 2-3 sentence overview of all changes
- **Rationale**: Why these changes were needed
- **Implementation notes**: Key technical decisions or approaches
- **Test plan**: How to verify changes work
- **Breaking changes**: Any backward-incompatible changes (if none, state "None")

## Edge Cases

- If no changes exist: report this and exit without creating plan file
- If changes are already committed but not pushed: plan only the PR description
- If multiple unrelated change types exist: propose multiple commits with clear
  separation
