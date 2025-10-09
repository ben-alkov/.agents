---
description: this command creates a quick conversation summary with key context
allowed-tools: Write(*.md)
argument-hint: [filename]
---
<!-- markdownlint-disable single-h1 no-inline-html -->
# /context-compacts Command

Create a concise summary of the conversation and write it
to the `agent-notes` directory at the repository root.

## Arguments

- `$1` (optional): Filename within `agent-notes/` directory
  - Default: `claude_compact_recent.md`
  - Security: Only basename used; paths with `/` or `..` rejected

## Pre-requisites

1. Locate the git repository root directory
2. Construct file path: `{repo_root}/agent-notes/{filename}`
   - Use `claude_compact_recent.md` if no argument provided
3. Check if the file exists:
   - **File exists:**
     - Ask user: "Would you like to write context to `{filename}`?"
     - If user confirms:
       - Continue to "## Task"
     - If user declines:
       - Acknowledge: "Context compact skipped"

## Task

Your summary must include:

1. **Primary Request and Intent**: All explicit user requests in detail
2. **Key Technical Concepts**: Technologies, frameworks, and architectural decisions
3. **Files and Code Sections**: Specific files examined/modified with code
   snippets and rationale
4. **Problem Solving**: Issues resolved and ongoing troubleshooting
5. **Pending Tasks**: Explicitly requested work not yet completed
6. **Current Work**: What was being worked on immediately before this summary
   (include file names and code snippets)
7. **Optional Next Step**: Only if directly aligned with explicit user requests;
   include verbatim quotes showing what task you were working on

Before writing the summary, analyze the conversation chronologically in
`<analysis>` tags to identify:

- User's explicit requests and intents
- Your approach to addressing requests
- Key decisions and code patterns
- Specific details (file names, function signatures, code snippets)

If additional summarization instructions are present in the conversation,
incorporate them, remembering to follow these instructions when creating the
above summary.

Examples of additional summarization instructions include:

Example 1

<example>

## Compact Instructions

When summarizing the conversation focus on Python code changes and also
remember the mistakes you made and how you fixed them.

</example>

Example 2

<example>

# Summary instructions

When you are using compact - please focus on test output and code changes.
Include file reads verbatim.

</example>

Use proper Markdown formatting with appropriate headers, code blocks, and lists.

## Example Usage

```text
/context-compacts
→ Writes agent-notes/claude_compact_recent.md

/context-compacts previous_work.md
→ Writes agent-notes/previous_work.md

/context-compacts ../etc/passwd
→ Rejected: Invalid path
```
