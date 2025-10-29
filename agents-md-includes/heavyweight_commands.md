# Heavyweight commands

Don't run commands that:

- Block indefinitely: dev servers, file watchers, REPL sessions
- Take >10 seconds: full test suites, large builds, dependency installations
- Produce >100 lines of output: verbose builds, migration logs

**Examples to avoid:**

```bash
npm run dev            # Dev server
cargo build --release  # Slow compilation
pytest                 # Full test suite
docker build .         # Image building
npm install            # Dependency installation
rails db:migrate       # Database migrations
```

**Instead:** Print the command and ask the user to run it in a separate terminal.

**Exceptions - safe to run:**

- Quick checks: `npm run lint`, `cargo check`, `pytest tests/unit/test_foo.py`
- Commands with `--help` or `--version` flags
- Fast builds explicitly requested by user
