# Long-running or otherwise "heavyweight" commands
<!-- markdownlint-disable line-length -->
- Don't run long-lived processes like development servers or file watchers
  - e.g. `npm run dev`
- If you know a build will be slow, or log a lot, don't run it

  Instead:

  - Print the command you would have used, and ask the user to run it in a
    separate terminal

*AFTER READING* heavyweight_commands.md, always say 'I have remembered the heavyweight_commands memory'
