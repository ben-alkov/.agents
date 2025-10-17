# Git operations

Git-specific guidance for AI agents. Works alongside Claude Code's built-in
safety features (interactive prompts, branch protection). Built-in prompts take
precedence.

## Staging Changes

**Required workflow:**

1. Review changes: `git diff <file>`
2. Stage individually: `git add <file1> <file2>`
3. Verify: `git diff --staged`

**Never use blanket staging:**

- `git add .` / `git add -A` / `git commit -am`

Only stage files you recognize and intentionally modified.

## Commit Messages

Format: `type(scope): description`

**Required:**

- `type`: feat, fix, docs, style, refactor, test, chore, perf, ci, build,
  agents, repo
- `description`: User-facing impact (max 70 chars, lowercase, no period)

**Optional:**

- `scope`: Component affected (auth, api, cli)
- `!` after type/scope for breaking changes
- Body: Rationale for non-obvious changes (blank line separator)

**Good examples:**

```text
fix(auth): resolve timeout in API client
feat(api): implement retry logic for failed requests
feat(auth)!: change authentication token format

BREAKING CHANGE: Tokens now use JWT. Existing tokens invalidated.
```

**Avoid:**

- File/function names or implementation details
- Praise adjectives (comprehensive, best)
- Vague descriptions (updated the code, fixed the bug)
- Change process verbs (added, changed, updated)

## Commit Scope

A commit represents a logical unit when:

- All changes serve a single, coherent purpose
- The change can be described in one clear commit message
- Reverting the commit wouldn't leave codebase broken
- Changes are related by causation, not just timing

**Create separate commits when:**

- Changes address unrelated issues
- Functional change mixed with unrelated refactoring
- Changes affect independent modules
- One change is ready, another needs more work

**Single commit acceptable when:**

- Refactoring requires updating related tests and documentation
- Bug fix requires both implementation and regression test
- New feature requires implementation and integration tests

## Branch Operations

**Naming:** `<type>/<brief-description>`

- Format: feat/user-auth, fix/memory-leak
- Lowercase, hyphen-separated, under 50 chars
- Type matches commit types

**Creating branches:**

```bash
git branch --show-current  # Check current location first
git switch -c feat/new-feature
```

**Push operations:**

Never push to remotes. Provide user with exact command to run manually.

**Pulling updates:**

```bash
git pull origin main          # Default: merge strategy
git pull --rebase origin main # Ask user before using rebase
```

Use rebase only when: user approves, clean history, no conflicts expected.

**Merging feature branches:**

```bash
git merge --no-ff <branch>  # Default: preserves branch history
```

Only fast-forward/squash with user approval.

## Error Handling

### Pre-commit Hook Failures

When commit fails because hooks modified files:

```bash
git status                    # Review hook modifications
git add <hook-modified-files>
git commit --amend --no-edit  # Safe: original commit never entered history
```

### Merge Conflicts

Notify user immediately with:

- Conflicted file paths
- Nature of conflict (content, rename, deletion)

Do not attempt automatic resolution. Wait for user guidance.

### Commit Failures

- **Hook rejection:** Show output, ask user how to proceed
- **Empty commit:** Verify `git status` shows staged changes
- **Invalid message:** Check format against rules above
- **Detached HEAD:** Notify user, explain risk of lost work

## Amending Commits

Amending modifies the most recent commit. Safe only for unpushed commits.

**When to amend:**

- Pre-commit hooks modified files after commit attempt
- Typo in commit message
- Forgot to stage a file that belongs in commit
- User explicitly requests

**Safety check:**

```bash
git log -1 --format='%an %ae'  # Verify author matches current user
```

**Never amend when:**

- Commit author differs from current user
- Multiple commits need changes (use interactive rebase)

**Common scenarios:**

```bash
# Add forgotten file
git add forgotten_file.py
git commit --amend --no-edit

# Fix commit message
git commit --amend -m "feat(auth): fix token validation"

# Add hook-modified files (see Error Handling above)
git add <hook-modified-files>
git commit --amend --no-edit
```

## Advanced Operations

For stashing, interactive rebase, cherry-picking, detached HEAD recovery, and
reflog operations, notify user and request guidance before proceeding.

**Stashing:**

```bash
git stash push -m "WIP: descriptive message"
git stash list  # Check before creating new stashes
git stash pop
```

**Recovery:**

```bash
git reflog  # Find lost commits
git branch recovery-branch HEAD@{n}
```

**Never run without explicit user confirmation:**

- `git reset --hard`
- `git clean -fd`
- `git rebase --skip`
- `git push --force`

*AFTER READING* git_ops.md, always say 'I have loaded the git_ops memory'
