# this command restores context

## Session Initialization

At the beginning of each new session, regardless of the current working directory

1. Check if `claude_compact_recent.md` exists in the current directory
2. If it exists, ask the user if they would like to continue with the
   conversation context from that file
3. If the user agrees, incorporate the content as context for the current session

## Conversation Management

When handling the "/compact" command

1. Add the conversation summary to `claude_compact_history.txt` with timestamp
2. Overwrite `claude_compact_recent.txt` with the same content
3. Format the header as: `# Conversation Summary - <timestamp>`
4. Use local time with format: `YYYY-MM-DD HH:MM:SS TZ`
