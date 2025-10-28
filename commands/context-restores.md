---
description: this command loads conversation context from "agent-notes" directory
allowed-tools: Read
argument-hint: [filename]
---

# /context-restores

Load conversation context from a previous session stored in the `agent-notes`
directory at repository root.

## Task

Read `{repo_root}/agent-notes/{filename}` where filename is `$1`.

- Sanitize filename: use basename only (reject paths containing `/` or `..`)
- If file exists: read and internalize content for session reference
- If file not found: inform user with full path, continue normally

## Success Criteria

Context successfully loaded when you can answer questions about the previous
session based on file contents.
