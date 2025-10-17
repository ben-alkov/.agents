---
description: this command writes a conversation summary to "agent-notes" directory
allowed-tools: Bash(git rev-parse:--show-toplevel), Read, Write(*/agent-notes/*.md)
argument-hint: [filename.md]
---

# /context-compacts

Write a concise summary of this conversation to `agent-notes/` directory at
repository root.

## Arguments

- `$1` (optional): Filename for the summary
  - Default: `claude_compact_recent.md`
  - Must be basename only (no `/` or `..`)

## Workflow

1. Find git repository root: `git rev-parse --show-toplevel`
2. Set target path: `{repo_root}/agent-notes/{$1 or claude_compact_recent.md}`
3. Validate filename: reject if contains `/` or `..`
4. If file exists, ask: "Overwrite {filename}?"
5. Generate summary and write to file

## Summary Content

Include in order of relevance:

- **User requests**: What user asked for (verbatim when helpful)
- **Files modified**: Paths and key code changes with snippets
- **Technical decisions**: Technologies, patterns, trade-offs
- **Issues resolved**: Problems encountered and solutions
- **Pending work**: Incomplete tasks from user requests

If user provided custom summarization instructions in this conversation, apply
them.

Use proper Markdown: headers, code blocks with language tags, lists.

## Example Usage

```text
/context-compacts
→ Writes agent-notes/claude_compact_recent.md

/context-compacts auth_refactor.md
→ Writes agent-notes/auth_refactor.md

/context-compacts ../escape.md
→ Rejected: Invalid path
```
