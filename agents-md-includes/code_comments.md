# Code comments

## Core Principle

Comments explain WHY, never WHAT. Code structure and naming should make WHAT obvious.

**Inline comments** (within function bodies): Explain non-obvious WHY within implementation
**Documentation comments** (docstrings, JSDoc, etc.): Describe WHAT the API does and HOW to use it

This guide addresses inline comments. Documentation comments follow language-specific conventions.

## When to Write Comments

Comment only when code alone cannot convey:

- Non-obvious business logic or domain constraints
  - Example: `# ISO 8601 requires 'Z' suffix, not '+00:00', for UTC`
- Safety-critical invariants or assumptions
  - Example: `# Buffer must be pre-allocated; reallocation would invalidate pointers`
- Performance trade-offs in non-obvious code
  - Example: `# Linear search faster than binary for N < 50 in benchmarks`
- API quirks, workarounds, or third-party library bugs
  - Example: `# os.rename() fails across filesystems; use shutil.move() instead`
- Complex algorithms where math/logic isn't obvious
  - Example: `# Sieve of Eratosthenes: O(n log log n) vs trial division O(n√n)`
- Security-sensitive operations
  - Example: `# Constant-time comparison prevents timing attacks`

## Never Comment

**Don't describe change history:**

- Avoid past tense verbs: added, removed, changed, updated, modified, fixed
- Avoid version phrases: "now handles", "previously", "updated to support"
- Comments describe current state, not evolution
- Bad: `timeout(10_000)  # Increased from 5s`
- Good: `timeout(10_000)  # API gateway has 8s max response time`

**Don't comment out code:**

- Delete unused code; version control preserves history

**Don't use end-of-line comments:**

- Place comments above code they describe
- Exception: Tabular configuration constants

**Don't leave TODO/FIXME/NOTE/HACK markers:**

- Complete the work or communicate to user
- Never commit untracked markers

**Don't be redundant:**

- Never comment self-explanatory code
- Bad: `user_count += 1  # Increment user count`

## Formatting

**Capitalization and punctuation:**

- Complete sentences with proper capitalization and periods

**Line length:**

- Keep comment lines under 80 characters
- Break long comments into multiple lines

**Spacing:**

- One blank line before comment blocks (except at start of function/block)
- No blank line between comment and code it describes
- Indent comments to match code

**Example:**

```python
def process_payment(amount, currency):
    if amount <= 0:
        raise ValueError("Amount must be positive")

    # Payment processor requires amounts in smallest currency unit.
    # JPY has no fractional unit; skip conversion for Japanese currency.
    if currency == "JPY":
        normalized = int(amount)
    else:
        normalized = int(amount * 100)

    return submit_to_processor(normalized, currency)
```

## Edge Cases

**Regex patterns:** Document complex patterns with breakdown
**Magic numbers:** Explain non-obvious constants with calculation/rationale
**Platform-specific behavior:** Document OS differences explicitly

## Application

- When editing existing code: preserve existing comment style unless it violates
  critical rules (TODO markers, commented-out code)
- When writing new code: apply these rules strictly
- If project has explicit comment conventions (CONTRIBUTING.md, etc.): defer to
  project conventions
