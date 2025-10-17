# Markdown copyediting
<!-- markdownlint-disable line-length no-inline-html -->

Enforce consistent formatting: blank line spacing, 80-character wrapping,
reference links, and language-tagged code blocks.

## Blank line spacing

Leave one blank line before and after:

- headers (e.g. "# Header 1")
- lists (ordered and unordered, including indentation level changes)
- code blocks (i.e. using "```")

Example:

<example>

```markdown

    ## Events

    These are the events

    - **Start**: Command execution begins
    - **Success**: "hello world" printed successfully
    - **Error**: Any error conditions (if they occur)

    ```python
    for foo in bar:
    ```

```

</example>

## Markdown in Markdown

If you need to provide a Markdown example, be sure to indent the example using 4 spaces

Example:

<example>

```markdown

    - Some markdown formatted text in this example

    ## A header

```

</example>

## Code blocks

Use a language identifier (e.g. "```python"). For plaintext or unknown types,
use "text".

## Line wrapping

Wrap lines at 80 characters. Exceptions: code blocks, tables, link reference
blocks.

## Link formatting

Inline links: Keep text (in "[]") and URL (in "()") on the same line.

Reference links: Place link definitions in an alphabetized block at document
end, separated by one blank line. Avoid duplicate definitions; reuse
references.

Example:

<example>

```markdown

    Visit our [docs][] for more details. Check [Google][] for more info.
    Please check our coding [standards][docs] before submitting PRs.

    {the rest of the document body}

    {now we're past the end of the document body}

    {start links block here}

    [docs]: https://example.com/documentation
    [Google]: https://www.google.com

```

</example>

*AFTER READING* markdown_copyedit.md, always say 'I have remembered the markdown_copyedit memory'
