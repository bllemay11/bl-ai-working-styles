# bl-ai-working-styles

Portable, reusable working style definitions for AI agent interactions. Define your preferences once, use them across every project.

## Problem

Every new AI coding session starts from scratch. You re-explain your communication preferences, review patterns, git workflow, and operational habits. Multiply that across projects and sessions, and it's a significant tax.

## Solution

This repo is the single source of truth for how you work with AI agents. Projects symlink to it, so updates propagate everywhere instantly.

```
your-project/
  working-styles/ -> /path/to/bl-ai-working-styles/working-styles/
  .claude/
    current-user     # contains "bl" (or your identifier)
    hooks/
      post-compaction # displays reminders after context compaction
```

## How It Works

1. **Symlink** `working-styles/` into your project
2. **Identify** the current user via `.claude/current-user`
3. **Load** the user's working style at session start from `working-styles/{user}/working-style.md`
4. **Select a persona** (optional) for task-specific behavior
5. **Remind** after compaction via the post-compaction hook

## What's In a Working Style

A working style captures everything about how you prefer to interact with AI agents:

- Communication preferences (conciseness, formatting, terminology)
- Review patterns (section-by-section, one-at-a-time)
- Git workflow (commit format, staging rules, push policy)
- Documentation approach (design docs before code, living docs)
- Testing standards (TDD, coverage expectations, execution patterns)
- Decision rules (when to ask vs. when to decide autonomously)
- Anti-patterns (things the agent must never do)

## Persona System

Working styles can include specialized personas -- task-specific modes that tune agent behavior:

| # | Persona | Purpose |
|---|---------|---------|
| 1 | Bootstrapper | Project setup, phased planning, documentation-first |
| 2 | Architect Guru | Find ambiguities and contradictions in architecture |
| 3 | Doc Reviewer | Cross-reference consistency, conciseness optimization |
| 4 | Data Modeler | Schema design, validation rules, constraints |
| 5 | Technical Reviewer | Bug finding, spec-vs-implementation mismatches |
| 6 | Expert SWE | TDD implementation, tier-0 quality code |
| 7 | Refactoring Agent | Autonomous duplication elimination (loop mode) |
| 8 | QA Hackaroo | Edge cases, adversarial testing, breaking things |
| 10 | Operator | Operations, deployments, data changes with rollback |

## Slash Commands

Each working style can include slash commands. bl's commands live in `working-styles/bl/commands/` and can be symlinked to `~/.claude/commands/` for cross-machine sharing. See [working-styles/bl/commands/README.md](working-styles/bl/commands/README.md) for the full list and installation.

## Quick Start

### For a new project

```bash
# 1. Symlink working-styles into your project
ln -s /absolute/path/to/bl-ai-working-styles/working-styles ./working-styles

# 2. Add to .gitignore
echo "working-styles" >> .gitignore

# 3. Set up user identity
mkdir -p .claude
echo "bl" > .claude/current-user

# 4. Install the post-compaction hook
mkdir -p .claude/hooks
cp /path/to/bl-ai-working-styles/templates/claude/hooks/post-compaction .claude/hooks/
chmod +x .claude/hooks/post-compaction

# 5. Add minimal session start protocol to your CLAUDE.md
# (see claude-template.md for the template)
```

### User-level permission (one-time)

Claude Code needs permission to read outside the project directory. Add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "additionalDirectories": [
      "/absolute/path/to/bl-ai-working-styles/working-styles"
    ]
  }
}
```

See [BOOTSTRAP.md](BOOTSTRAP.md) for detailed instructions covering three scenarios (new repo, existing repo without CLAUDE.md, existing repo with CLAUDE.md).

## Project Structure

```
bl-ai-working-styles/
  CLAUDE.md                 # Instructions for working in this repo
  BOOTSTRAP.md              # Detailed project onboarding instructions
  claude-template.md        # Minimal CLAUDE.md template for projects
  design/
    working-styles-pattern.md   # Architecture and design decisions
  templates/claude/
    hooks/post-compaction       # Hook that displays reminders after compaction
    current-user.example        # Example user identity file
  working-styles/
    README.md                   # System documentation
    bl/                         # bl's working style
      working-style.md          # Core style definition (session protocols, preferences)
      reminders.md              # Post-compaction reminders
      commands/                 # Portable slash commands
      persona-*.md              # 10 specialized personas
```

## Multiple Users

Multiple people can use this in the same project. Each user:

1. Creates their own directory under `working-styles/` (e.g., `working-styles/alice/`)
2. Writes their own `working-style.md` and optional personas
3. Sets `.claude/current-user` to their identifier on their machine

The `.claude/current-user` file is gitignored, so each developer's identity stays local.

## License

MIT
