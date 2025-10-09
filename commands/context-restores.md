---
description: this command loads conversation context from agent-notes directory
to continue previous session
allowed-tools: Read
argument-hint: [filename]
---

# /context-restores Command

Load conversation context from a previous session stored in the `agent-notes`
directory at the repository root.

## Arguments

- `$1` (optional): Filename within `agent-notes/` directory
  - Default: `claude_compact_recent.md`
  - Security: Only basename used; paths with `/` or `..` rejected

## Task

1. Locate the git repository root directory
2. Construct file path: `{repo_root}/agent-notes/{filename}`
   - Use `claude_compact_recent.md` if no argument provided
3. Check if the file exists:
   - **File exists:**
     - Ask user: "Would you like to restore context from `{filename}`?"
     - If user confirms:
       - Read entire file content
       - Present the content verbatim (it has already been compacted)
       - Keep content available for reference throughout session
     - If user declines:
       - Acknowledge: "Context restoration skipped"
   - **File not found:**
     - Inform user: "No context file found at `{full_path}`"
     - Do not treat as error; continue normally

## Expected Behavior

**Success indicators:**

- User can reference information from restored context
- You can answer questions about previous session work
- Conversation continuity established

**Failure cases:**

- **File unreadable:** Report error with full path and permission details
- **Invalid content:** Load anyway, note if content appears malformed
- **Path traversal attempt:** Reject with security warning

## Constraints

- If argument contains path separators, extract basename only
- Do not modify any files; this is read-only operation

## Example Usage

```text
/context-restores
→ Loads agent-notes/claude_compact_recent.md

/context-restores previous_work.md
→ Loads agent-notes/previous_work.md

/context-restores ../etc/passwd
→ Rejected: Invalid path
```
