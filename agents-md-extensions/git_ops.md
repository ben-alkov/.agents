# Git operations

This extension provides git-specific guidance that complements the system-level
git safety protocol. Always follow system git protocols first.

## Staging Changes

- Always stage individual files using `git add <file1> <file2> ...`
- Before staging, verify changes with `git diff <file>`:
  - Only stage files containing modifications you created
  - Skip files you don't recognize or didn't intentionally modify
- Avoid blanket staging commands:
  - Never use `git add .` or `git add -A`
  - Never use `git commit -am`
- If pre-commit hooks modify files:
  - Review hook output with `git status`
  - Stage hook-modified files only after hooks complete
  - Add these files and retry the commit

## Commit Messages

### Conventional Commits Format

Use Conventional Commits format: `type(scope): description`

**Required:**

- `type`: One of: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`,
  `perf`, `ci`, `build`, `agents`, `repo`
- `description`: Intent of the change, not implementation details

**Optional:**

- `scope`: Component/module affected (e.g., `auth`, `api`, `cli`)
- `!` after type/scope for breaking changes: `feat!: description`

**Rules:**

- MUST be concise: maximum 70 characters
- Use lowercase for type and description
- No period at end of description
- Describe what the change accomplishes, not how it works
- Sound like the title of an issue being resolved
- Avoid praise adjectives: no "comprehensive", "best practices", "essential"

### Commit Message Body

Use the body (separated by blank line) only when:

- First line alone doesn't capture why the change was made
- Context about rationale (not mechanics) would help future readers
- Breaking changes need explanation (prefix with `BREAKING CHANGE:`)

### Examples

Good commit messages:

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

With body:

```text
feat(auth)!: change authentication token format

BREAKING CHANGE: Authentication tokens now use JWT instead of
custom format. Existing tokens will be invalidated and users
must re-authenticate.
```

Bad commit messages:

```text
updated the code
comprehensive refactor with best practices
changed timeout from 5s to 10s in http_client.py
fixed the bug
fix: the authentication issue
feat: added some features
```

## Commit Scope

- Create commits representing logical units of work
- If changes span unrelated concerns, create separate commits
- Avoid committing partial implementations unless user explicitly requests it
- Check `.gitignore` before staging to avoid committing excluded files

## Branch Operations

- Check current branch before creating new ones: `git branch --show-current`
- Use descriptive branch names which align with Conventional Commits style:
  `feat/description`, `fix/issue-name`
- **!!NEVER!!** force-push to `main` or `master` branches
- Before pushing, verify upstream status: `git branch -vv`

## Error Handling

If commit fails due to pre-commit hooks:

- Review hook output for newly modified files
- Run `git status` to see new changes
- Stage hook modifications and retry commit
- Follow system protocol for amending vs creating new commits

If merge conflicts occur:

- Notify user immediately with conflict locations
- Do not attempt automatic resolution without user confirmation
- Explain the nature of the conflict

## Amending Commits

Amending modifies the most recent commit. Use with extreme caution.

**When to amend:**

- Pre-commit hooks modified files after initial commit
- Typo in commit message needs fixing
- Forgot to stage a file that belongs in the commit
- User explicitly requests amending

**When NOT to amend:**

- Commit has been pushed to remote (creates divergent history)
- Commit author is different from current user
- Multiple commits need changes (use rebase instead)
- Unsure if commit has been shared with others

**Safety checks before amending:**

```bash
# Check commit author matches current user
git log -1 --format='%an %ae'

# Verify commit hasn't been pushed
git status  # Should show "Your branch is ahead"

# Check branch tracking
git branch -vv
```

**Amending examples:**

Add forgotten file to last commit:

```bash
git add forgotten_file.py
git commit --amend --no-edit
```

Fix commit message:

```bash
git commit --amend -m "feat(auth): fix token validation"
```

Add hook-modified files (after pre-commit hook runs):

```bash
# Hook modified files, commit failed
git status  # See what hook changed
git add file_modified_by_hook.py
git commit --amend --no-edit
```

Amend with new message and additional changes:

```bash
git add additional_changes.py
git commit --amend -m "feat(api): implement retry logic with backoff"
```

**Never do:**

```bash
# Don't amend pushed commits
git commit --amend  # If already pushed - creates conflicts

# Don't amend other people's commits
git log -1 --format='%an'  # Check author first

# Don't skip verification
git commit --amend --no-verify  # Unless user explicitly requests
```

*AFTER READING* git_ops.md, always say 'I have remembered the git_ops memory'
