# Portable Slash Commands

Reusable Claude Code slash commands that work across any project.

## Commands

| Command | Purpose |
|---------|---------|
| `/bl-start-work` | Kick off implementation. Sets task completion standards, prevents premature stopping. |
| `/bl-check-work` | Verify phase completion. Produces checklist status + success criteria tables. |
| `/bl-reminders` | Display critical reminders (no emojis, no estimates, drive to criteria). |
| `/bl-session-id` | Find current session ID, verify it, create a bash alias for restart. |
| `/bl-add-session-to-bashrc` | Add current session as a named bash alias. Takes alias name as argument. |
| `/bl-test-report` | Run all test suites in a project and produce a pass/fail/time report. |
| `/bl-nest-project` | Move a local project folder into the `ooc-team` repo with the right layout, README, and commit. |

## Installation

Symlink each command individually into `~/.claude/commands/`:

```bash
STYLES_DIR="/absolute/path/to/bl-ai-working-styles"

# Create the commands directory if it doesn't exist
mkdir -p ~/.claude/commands

# Symlink each command
for cmd in "$STYLES_DIR"/working-styles/bl/commands/bl-*.md; do
  ln -sf "$cmd" ~/.claude/commands/$(basename "$cmd")
done
```

To verify:

```bash
ls -la ~/.claude/commands/bl-*.md
```

Each entry should be a symlink pointing back to this repo.

## Adding New Commands

1. Create a new `.md` file in this directory
2. Re-run the symlink loop above (or `ln -sf` the individual file)
3. The command appears as `/filename` in Claude Code (without the `.md` extension)

## Project-Specific Commands

Commands that reference project-specific paths (test configs, specific directories) should live in the project's own `.claude/commands/` directory, not here. This repo is for commands that work anywhere.
