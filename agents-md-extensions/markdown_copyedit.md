# Markdown copyediting
<!-- markdownlint-disable line-length -->
- When writing Markdown *always* leave one blank line between *both* the
    *preceding* and the *following* content, for elements including

  - headers (e.g. "# Header 1")
  - any lists, both ordered and unordered, including lists where the of level of
    indentation changes
  - code blocks (i.e. using "```")

  Example, enclosed in triple-quotes

  """

  ## Events

  These are the events

  - **Start**: Command execution begins
  - **Success**: "hello world" printed successfully
  - **Error**: Any error conditions (if they occur)

  ```text
  Example code block
  ```

  """

- When writing code blocks in Markdown (i.e. using "\```"), *always* use a language
  identifier (e.g. "```python"). If the enclosed text is plaintext, or of unknown
  type, use the language identifier "text".
- When writing Markdown, *always* wrap lines at 80 lines, unless the lines are
  in a code block, or in a link block at the end of the file.
- When writing Markdown, *always* wrap Inline Links so that the text (in "[]")
  and the definition (in "()") are on the same line.
- When writing Markdown, *always* use In-Document Reference Links. The actual
  links should be in an alphabetized block after the end of the document body,
  with a single blank line separating the links from the rest of the document.
  Please do NOT create duplicate links in the links block; simply make a new
  reference instead.

    Example, enclosed in triple-quotes

    """

    Visit our [docs][] for more details. Check [Google][] for more info.
    Please check our coding [standards][docs] before submitting PRs.

    {the rest of the document body}

    {now we're past the end of the document body}

    {start links block here}

    [docs]: https://example.com/documentation
    [Google]: https://www.google.com

    """

*AFTER READING* markdown_copyedit.md, always say 'I have remembered the markdown_copyedit memory'
