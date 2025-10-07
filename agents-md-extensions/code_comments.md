# Code comments
<!-- markdownlint-disable line-length -->
- Use comments VERY sparingly
- Don't comment out code; remove it instead
- Don't add comments that describe the process of changing code
  - Comments should not include past tense verbs like 'added', 'removed', or 'changed'

  Example: `timeout(10_000);  # Increase timeout for API calls`: this is bad
  because the reader doesn't know what the timeout was increased from, and doesn't
  care about the old behavior

- Don't add comments that emphasize different versions of the code, like
  `# this code now handles`
- Do not use end-of-line comments; instead, place comments above the code they
  describe
- Prefer editing an existing file to creating a new one

*AFTER READING* the code_comments.md file, always say 'I have remembered the code_comments memory'
