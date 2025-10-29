# Markdown copyediting

Apply these rules when editing markdown files in documentation, READMEs, or
user-facing content. Preserve existing project style when it differs.

## Core rules

**Blank lines:** One blank line before/after headers, lists, and code blocks.

**Line wrapping:** 80 characters. Exceptions: code blocks, tables, URLs.
<!-- markdownlint-disable no-space-in-code -->
**Code blocks:** Always use language tags (```python, ```bash, ```text).

**Links:**

- Inline: Keep `[text](url)` on same line, even if it exceeds 80 chars
- Reference: Place definitions alphabetically at document end, separated by one
  blank line

## Examples

Blank line spacing:

```markdown
    ## Section Header

    Paragraph text here.

    - List item one
    - List item two

    ```python
    code_here()
    ```

    Next paragraph.
```

Reference links:

```markdown
    See [documentation][docs] and [examples][docs] for details.
    Check [Google][] for general search.

    [docs]: https://example.com/documentation
    [Google]: https://www.google.com
```

Markdown examples (indent 4 spaces):

```markdown
    Example of markdown:

        ## Header

        - List item
```
