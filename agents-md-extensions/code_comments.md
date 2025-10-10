# Code comments
<!-- markdownlint-disable line-length -->

## Core Principle

Comments should explain WHY, never WHAT. Code structure and naming should make
WHAT obvious.

## Distinction: Inline Comments vs Documentation Comments

This guide primarily addresses **inline comments** (explanatory comments within
function bodies).

**Documentation comments** (docstrings, JSDoc, Javadoc, XML docs, Rustdoc) serve
a different purpose: they document public APIs, function signatures, parameters,
return values, and usage examples. These follow language-specific conventions
and are NOT subject to the "use sparingly" rule.

**Inline comments**: Explain non-obvious WHY within implementation
**Documentation comments**: Describe WHAT the API does and HOW to use it

## When to Write Inline Comments

Write inline comments only when:

- Explaining non-obvious business logic or domain constraints
  - Example: `# ISO 8601 requires 'Z' suffix, not '+00:00', for UTC`
- Documenting safety-critical invariants or assumptions
  - Example: `# Buffer must be pre-allocated; reallocation would invalidate pointers`
- Clarifying performance trade-offs for non-obvious code
  - Example: `# Linear search faster than binary for N < 50 in benchmarks`
- Warning about subtle bugs or API quirks
  - Example: `# os.rename() fails across filesystems; use shutil.move() instead`
- Explaining complex algorithms where the math/logic isn't obvious
  - Example: `# Use Sieve of Eratosthenes: O(n log log n) vs trial division O(n√n)`
- Documenting workarounds for third-party library bugs
  - Example: `# Library v2.3 incorrectly caches None; clear manually`

## Inline Comment Prohibitions

### Don't Describe the Change Process

Never use past tense verbs: added, removed, changed, updated, modified, fixed,
improved, refactored, enhanced.

Comments should describe current state and intent, not historical evolution.

Bad:

```python
timeout(10_000)  # Increased timeout for API calls
max_retries = 5  # Changed from 3 to handle flaky network
```

Why bad: Reader doesn't know what timeout was increased from, and doesn't care
about old behavior.

Good:

```python
timeout(10_000)  # API gateway has 8-second max response time
max_retries = 5  # Network reliability is ~94%; 5 retries gives 99.9% success
```

### Don't Emphasize Code Versions

Avoid phrases like "this code now handles", "previously this was", "updated to
support", "new implementation", "revised approach".

Bad:

```python
# This code now handles edge cases for null values
if value is None:
    return default

# Updated to support async operations
async def fetch_data():
    ...
```

Good:

```python
# Null values indicate optional fields in legacy API v1
if value is None:
    return default

async def fetch_data():
    ...
```

### Don't Comment Out Code

Delete unused code instead of commenting it out. Version control preserves
history; commented code creates clutter and confusion.

Bad:

```python
def calculate_price(item):
    # Old pricing logic
    # base_price = item.cost * 1.5
    # if item.category == "premium":
    #     base_price *= 1.2

    # New pricing logic
    return item.cost * get_markup(item.category)
```

Good:

```python
def calculate_price(item):
    return item.cost * get_markup(item.category)
```

### Don't Use End-of-Line Comments

Place comments above the code they describe. End-of-line comments interrupt
reading flow and become misaligned when code changes.

Bad:

```python
result = calculate_tax(amount, rate)  # Apply regional tax calculation
return result * 1.15  # Add service fee
discount = 0.1 if is_member else 0.0  # Members get 10% off
```

Good:

```python
# Apply regional tax calculation
result = calculate_tax(amount, rate)

# Add 15% service fee required by payment processor
return result * 1.15

# Members receive 10% discount
discount = 0.1 if is_member else 0.0
```

**Exception**: Extremely short configuration or data declarations where the
comment is part of a table-like structure:

```python
# Configuration constants (exception: tabular format)
MAX_CONNECTIONS = 100   # Default pool size
TIMEOUT_MS = 5000       # Socket timeout
RETRY_LIMIT = 3         # Max retry attempts
```

### Don't Add TODO/FIXME/NOTE/HACK Comments

Never commit code with untracked TODO, FIXME, NOTE, HACK, or similar markers.
These create technical debt and clutter.

If you identify work that needs to be done:

- Complete it immediately if possible
- If not possible, communicate it to the user or team
- Do not leave markers in committed code

Bad:

```python
def process_order(order):
    # TODO: Add validation for order.items
    # FIXME: This doesn't handle international shipping
    # HACK: Temporary workaround for bug in payment library
    # NOTE: Need to refactor this later
    total = sum(item.price for item in order.items)
    return total
```

Good:

```python
def process_order(order):
    # Validation skipped: only called from validated sources (admin panel)
    total = sum(item.price for item in order.items)
    return total
```

Or better: Actually implement the validation instead of leaving a TODO.

### Don't Be Redundant

Never comment code that's already self-explanatory through naming.

Bad:

```python
user_count += 1  # Increment user count
total = price * quantity  # Calculate total
is_valid = check_validation(data)  # Validate data
```

Good:

```python
user_count += 1
total = price * quantity
is_valid = check_validation(data)
```

## Comment Style and Formatting

When you write inline comments:

### Capitalization and Punctuation

- Use complete sentences with proper capitalization
- End sentences with periods
- Use proper punctuation for clarity

Good:

```python
# Payment processor requires amounts in smallest currency unit.
# Calculate annual depreciation using straight-line method.
```

Bad:

```python
# payment processor requires amounts in smallest currency unit
# calculate annual depreciation using straight-line method
```

### Line Length and Wrapping

- Keep comment lines under 80 characters
- Break long comments into multiple lines
- Maintain consistent indentation

Good:

```python
# Payment processor requires amounts in smallest currency unit
# (cents for USD, yen for JPY). JPY has no fractional unit, so
# we skip the conversion step for Japanese currency.
if currency == "JPY":
    normalized = int(amount)
```

Bad:

```python
# Payment processor requires amounts in smallest currency unit (cents for USD, yen for JPY). JPY has no fractional unit.
if currency == "JPY":
    normalized = int(amount)
```

### Spacing and Placement

- Place one blank line before comment blocks (unless at start of
  function/block)
- No blank line between comment and the code it describes
- Indent comments to match the code they describe

Good:

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

Bad:

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

### Comment Blocks vs Single Lines

Use single-line comments for brief explanations:

```python
# Retry with exponential backoff
for attempt in range(max_retries):
    ...
```

Use multi-line comments for complex explanations:

```python
# The Luhn algorithm validates credit card numbers through
# checksum calculation:
# 1. Double every second digit from right to left
# 2. Sum all digits (splitting doubled values: 12 → 1+2)
# 3. Valid if sum modulo 10 equals 0
def validate_card_number(number):
    ...
```

## Edge Cases and Special Situations

### Regex Patterns

Complex regex patterns often need explanation:

```python
# Match email addresses per RFC 5322 (simplified):
# - Local part: alphanumeric + dots (no consecutive dots)
# - Domain: alphanumeric + dots + hyphens
# - TLD: 2+ letters
email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
```

### Magic Numbers

Explain non-obvious constants:

```python
# TCP maximum segment size: 1460 bytes
# (1500 MTU - 20 IP header - 20 TCP header)
MAX_SEGMENT_SIZE = 1460

# Retry delay uses binary exponential backoff capped at 32 seconds
# to prevent excessive wait times on persistent failures
MAX_BACKOFF_SECONDS = 32
```

### Security-Critical Code

Always comment security-sensitive operations:

```python
# Constant-time comparison prevents timing attacks that could
# reveal token length or character positions through response timing
def verify_token(provided, expected):
    return hmac.compare_digest(provided, expected)
```

### Platform-Specific Behavior

Document platform differences:

```python
# Windows requires explicit binary mode for file operations;
# Unix-like systems treat binary/text identically
mode = 'rb' if os.name == 'nt' else 'r'
```

## Summary: When to Comment

**Always comment:**

- Non-obvious WHY (business logic, domain constraints)
- Safety-critical invariants
- Performance trade-offs
- API quirks and workarounds
- Complex algorithms
- Security-sensitive code

**Never comment:**

- Obvious WHAT (redundant to code)
- Change history (use version control)
- Code versions (old/new)
- Commented-out code (delete it)
- TODOs/FIXMEs (complete or communicate)

*AFTER READING* the code_comments.md file, always say 'I have remembered the code_comments memory'
