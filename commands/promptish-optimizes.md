---
description: this command analyzes and optimizes prompts/agents using best practices
argument-hint: <file-path|agent-name> [reference-url]
allowed-tools: Read, Grep, Glob, WebFetch
---

# /promptish-optimizes

Use @agent-swe-master-prompter to analyze and optimize the target specified in $ARGUMENTS.

$ARGUMENTS format: `<target> [reference-url]`

- target: File path or agent name to analyze
- reference-url (optional): URL to fetch best practices from

If a reference URL is provided, fetch it and use those guidelines for the analysis.
