# Git operations

This extension provides git-specific guidance for AI agents performing git
operations. These instructions work alongside Claude Code's built-in git safety
features (interactive prompts, branch protection, status checks).

When conflicts arise: built-in safety prompts take precedence over these
guidelines.

## Staging Changes

**Required workflow:**

1. Review changes: `git diff <file>` before staging
2. Stage individual files: `git add <file1> <file2> ...`
3. Verify staged changes: `git diff --staged`

**Only stage files you recognize and intentionally modified.**

**NEVER use blanket staging:**

- `git add .` (stages everything)
- `git add -A` (stages everything including deletions)
- `git commit -am` (bypasses staging review)

**Pre-commit hooks:**

When hooks run automatically during commit, they may modify files. This is
normal. The workflow is handled in "Error Handling" section below.

## Commit Messages

### Conventional Commits Format

Required format: `type(scope): description`

**Required components:**

- `type`: One of `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`,
  `perf`, `ci`, `build`, `agents`, `repo`
- `description`: User-facing impact or functional change

**Optional components:**

- `scope`: Component/module affected (e.g., `auth`, `api`, `cli`)
- `!` after type/scope for breaking changes: `feat!: description`

**Formatting rules:**

- Maximum 70 characters total
- Lowercase for type and description
- No period at end of description
- No file names, function names, or line numbers
- Describe what changed from user perspective, not how code changed
- Avoid praise adjectives: "comprehensive", "best practices", "essential"

### Commit Message Body

Use body (separated by blank line) only when:

- Subject line alone doesn't explain why the change was made
- Breaking changes need explanation (prefix with `BREAKING CHANGE:`)
- Context about rationale (not mechanics) would help future readers

### Examples

**Good commit messages:**

```text
fix(auth): resolve timeout in API client
feat(profile): add user profile validation
feat(api): implement retry logic for failed requests
fix(errors): update error messages for clarity
docs(readme): add installation instructions
refactor(parser): simplify token handling
test(auth): add integration tests for login flow
chore(deps): update dependencies to latest versions
```

**With body:**

```text
feat(auth)!: change authentication token format

BREAKING CHANGE: Authentication tokens now use JWT instead of
custom format. Existing tokens will be invalidated and users
must re-authenticate.
```

**Bad commit messages with explanations:**

```text
updated the code
# Missing type, too vague

comprehensive refactor with best practices
# Praise adjectives, vague, no type

changed timeout from 5s to 10s in http_client.py
# Implementation details, file name, describes HOW not WHAT

fixed the bug
# Too vague, no scope, no type

fix: the authentication issue
# Grammar error: should be "fix authentication issue"

feat: added some features
# Vague, "added" describes process not outcome
```

## Commit Scope

A commit represents a logical unit when:

- All changes serve a single, coherent purpose
- The change can be described in one clear commit message
- Reverting the commit wouldn't leave codebase broken
- Changes are related by causation, not just timing

**Create separate commits when:**

- Changes address unrelated issues (multiple independent bug fixes)
- Functional change mixed with unrelated refactoring
- Changes affect independent modules with no functional relationship
- One change is ready to merge, another needs more work

**Single commit is acceptable when:**

- Refactoring requires updating related tests and documentation
- Bug fix requires both implementation and regression test
- New feature requires implementation and integration tests

**Process:**

1. Group changed files by purpose before staging
2. If groups are unrelated: commit each group separately
3. Check `.gitignore` exists and is respected
4. Avoid committing partial implementations unless user explicitly requests

## Branch Operations

### Creating Branches

Check current location first:

```bash
git branch --show-current
```

**Branch naming convention:**

- Format: `<type>/<brief-description>`
- Type: Same as commit types (`feat`, `fix`, `refactor`, etc.)
- Description: lowercase, hyphen-separated, under 50 characters
- For non-standard work: `<username>/<description>`

**Examples:**

```text
feat/user-authentication
fix/memory-leak-parser
refactor/simplify-error-handling
docs/update-api-guide
```

**Bad examples:**

```text
new_feature  # No type prefix, underscores
Fix_Bug_In_Auth  # Mixed case, no context
my-branch  # No type prefix
feat/add-comprehensive-oauth2-with-jwt-support  # Too long (>50 chars)
```

### Push Operations

**NEVER push to any remote repository.** The agent must not execute:

- `git push`
- `git push --force`
- `git push --force-with-lease`
- `git push origin <branch>`

If the user needs changes on a remote, inform them and provide the exact
command they should run manually.

### Integrating Changes

**Pulling updates:**

```bash
# Default: merge strategy
git pull origin main

# Rebase strategy (ask user before using)
git pull --rebase origin main
```

Use rebase when user approves and:

- Working on feature branch with clean history
- User prefers linear history
- No merge conflicts expected

Use merge when:

- Preserving branch history is important
- Multiple developers' work needs to be integrated
- User explicitly prefers merge commits

**Merging feature branches:**

- Default: `git merge --no-ff <branch>` (preserves branch history)
- Only fast-forward or squash when user explicitly approves
- Never choose merge strategy without considering team conventions

**NEVER automatically:**

- Squash commits without user approval
- Resolve merge conflicts without showing user

## Error Handling

### Pre-commit Hook Failures

When commit fails because hooks modified files:

1. **Review changes:**

   ```bash
   git status  # See what hook modified
   git diff    # Verify changes are acceptable
   ```

2. **Stage hook modifications:**

   ```bash
   git add <hook-modified-files>
   ```

3. **Amend the commit:**

   ```bash
   git commit --amend --no-edit
   ```

**Why amend here:** The original commit never entered history (it failed), so
amending is safe and creates clean history. Never create a second "fix hook
changes" commit.

Note: Amending is always safe for the agent since commits are never pushed to
remotes.

### Merge Conflicts

When conflicts occur:

1. **Notify user immediately** with:
   - Conflicted file paths
   - Nature of conflict (content, rename, deletion)
2. **Do not attempt automatic resolution** without user confirmation
3. **Show conflict markers** if user requests details
4. **Wait for user guidance** on resolution strategy

### Commit Failures

If commit fails for other reasons:

- **Hook rejection:** Show hook output, ask user how to proceed
- **Empty commit:** Verify `git status` shows staged changes
- **Invalid message:** Check message format against rules above
- **Detached HEAD:** See "Advanced Workflows" section

## Amending Commits

Amending modifies the most recent commit.

### When to Amend

Safe scenarios:

- Pre-commit hooks modified files after commit attempt (see "Error Handling")
- Typo in commit message needs fixing
- Forgot to stage a file that belongs in the commit
- User explicitly requests amending

### When NOT to Amend

**NEVER amend when:**

- Commit author is different from current user
- Multiple commits need changes (use interactive rebase instead)

### Safety Checks Before Amending

Run this check before amending:

```bash
# Verify commit author matches current user
git log -1 --format='%an %ae'
```

### Amending Examples

**Add forgotten file:**

```bash
git add forgotten_file.py
git commit --amend --no-edit
```

**Fix commit message:**

```bash
git commit --amend -m "feat(auth): fix token validation"
```

**Add hook-modified files:**

```bash
# After pre-commit hook runs and modifies files
git status  # Verify what changed
git add file_modified_by_hook.py
git commit --amend --no-edit
```

**Amend with new message and additional changes:**

```bash
git add additional_changes.py
git commit --amend -m "feat(api): implement retry logic with backoff"
```

### Never Do

```bash
# Don't amend other people's commits
git log -1 --format='%an'  # Always check author first

# Don't skip verification
git commit --amend --no-verify  # Unless user explicitly requests
```

## Advanced Workflows

### Stashing Changes

When user needs to switch context without committing:

```bash
# Save work in progress
git stash push -m "WIP: descriptive message"

# List stashes
git stash list

# Apply later
git stash pop
```

Check `git stash list` before creating new stashes to avoid accumulating
forgotten work.

### Detached HEAD State

If `git status` shows "HEAD detached at <commit>":

1. **Notify user immediately**
2. **Explain:** Changes made in detached HEAD are lost when checking out other
   branches
3. **If user made commits:** Create recovery branch:

   ```bash
   git switch -c recovery-branch
   ```

4. **If no commits yet:** Safe to checkout another branch

### Cherry-picking

Only use when user explicitly requests:

```bash
git cherry-pick <commit-hash>
```

Avoid cherry-picking ranges or multiple commits without user confirmation.

### Interactive Rebase

Use interactive rebase to clean up commit history before integration.

**When to use (with user approval):**

- Squashing related commits into logical units
- Reordering commits for clarity
- Fixing commit messages in bulk
- Splitting commits that do too much
- Working on feature branch (never main/master)

**Process:**

```bash
# Create safety backup
git branch backup-before-rebase

# Then rebase
git rebase -i HEAD~n
```

**Common operations:**

- `pick`: Keep commit as-is
- `reword`: Change commit message
- `squash`: Combine with previous commit
- `fixup`: Combine with previous, discard message
- `drop`: Remove commit entirely

### Recovery Operations

If user reports lost commits:

```bash
# Find lost commits in reflog
git reflog

# Recover by creating branch from reflog entry
git branch recovery-branch HEAD@{n}
```

**NEVER run destructive recovery commands without explicit user confirmation:**

- `git reset --hard`
- `git clean -fd`
- `git rebase --skip`

## Quick Reference

**Safe operations (no confirmation needed):**

- `git status`, `git diff`, `git log`
- `git add <specific-files>`
- `git commit` (with proper message)
- `git branch`, `git branch --show-current`
- `git stash list`, `git reflog`
- `git commit --amend` (after author verification, unless handling hook failures)

**Requires user confirmation:**

- `git rebase`
- `git reset --hard`
- `git clean`
- Any destructive operation

**Prohibited operations:**

- `git push` (any variant)
- User must push manually

*AFTER READING* git_ops.md, always say 'I have remembered the git_ops memory'
