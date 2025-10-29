<!-- markdownlint-disable no-bare-urls -->
> NOTE: this repo is very much oriented towards Software Engineering tasks. I
> expect you to read the files to figure out what they do/how they work (you
> should be doing so regardless)

My personal .agents (.claude ⤷) directory, containing my config, plus custom:

- subagents
- slash-commands
- AGENTS.md (CLAUDE.md ⤷) includes (keeps AGENTS.md *very* short, and allows for
  better organization)
- convenience ./claude.json ⤷ ~/.claude.json

### Workflow for Prompt-likes

For creating any prompt-like files in here (subagents, AGENT.md includes,
slash-commands, probably skills some day), I come up with a basic idea, then go
through several rounds of Q&A and improvement using the swe-master-prompter
sub-agent. Then:

- invoke the swe-master-prompter subagent with a prompt roughly like:

  ```markdown
    Using {e.g.} https://docs.claude.com/en/docs/claude-code/sub-agents as a
    reference, please analyze/enhance/optimize the prompt {or file, or whatever}
    FOO"
  ```

  - Using specific links from docs.claude.com as a reference keeps
  swe-master-prompter focused on what *type* of prompt it should be working on.

  I use:

  - https://docs.claude.com/en/docs/claude-code/sub-agents - for subagents
  - https://docs.claude.com/en/docs/claude-code/slash-commands#custom-slash-commands,
    for slash-commands
  - Sadly, the docs for CLAUDE.md files aren't very good for this type of task

  > Note that "analyze/enhance/optimize" are actually specifically chosen and
    very distinct
- invoke the swe-master-prompter subagent with the same prompt **a second time**
  -- this is actually important. Prompted like this, swe-master-prompter tends to
  be quite completist and verbose.
- invoke the swe-master-prompter subagent with a prompt like:

  ```markdown
    Analyze and optimize this agent file using
    https://docs.claude.com/en/docs/claude-code/sub-agents as a reference.

    Focus on:

    - Excessive verbosity (reduce unnecessary instructions)
    - Proportional complexity (match complexity to task scope)
    - Tool alignment (ensure granted tools match actual needs)
    - Clear role definition and success criteria
    - Instruction clarity and structure

    Provide specific, actionable improvements. Propose concrete changes rather than
    general suggestions.

    File to analyze: {absolute_path_to_file}
  ```

  > Note that "enhance" is missing

  This *forces* swe-master-prompter to be quite parsimonius, literally counting
  every token. I've run this in a loop over "agents" and "agents-md-includes",
  and it tersified the files down by roughly 50% on avg.
