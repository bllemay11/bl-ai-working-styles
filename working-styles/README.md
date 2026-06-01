# AI Agent Working Styles System

## What Are Working Styles?

Working styles are personalized preference profiles that define how different users want to interact with AI agents. They capture:

- **Communication preferences**: Verbose vs concise, formal vs casual
- **Review processes**: Section-by-section vs wholesale review
- **Decision-making patterns**: When to ask vs when to infer
- **Session protocols**: How to start and end work sessions
- **Specialized personas**: Different modes for different tasks (implementation, review, refactoring, etc.)

## Why Working Styles?

Without working styles, preferences must be re-explained in every project and session, leading to:
- Wasted time establishing context
- Inconsistent experiences
- No reusability across projects

With working styles, AI agents automatically adapt to each user's preferences by loading their style at session start.

## How It Works

### For AI Agents (Claude)

At the start of each session:

1. **Check for current user** - Read `.claude/current-user` file if it exists:
   ```bash
   cat .claude/current-user
   ```
   If file doesn't exist or is empty, ask "Please identify yourself" and create the file:
   ```bash
   echo "user-id" > .claude/current-user
   ```

2. **Load the user's style** - Read `working-styles/{user-id}/working-style.md`

3. **Follow the protocols** - The working-style.md file contains:
   - Session start protocol (personas, task selection, etc.)
   - Session exit protocol (git status, doc checks, reflection)
   - Communication preferences
   - Review processes
   - All other user-specific preferences

**Note:** Available styles can be discovered by listing directories in `working-styles/`:
```bash
ls working-styles/
```

### For Users (Humans)

When you want to work with an AI agent that supports working styles:

1. The project's CLAUDE.md will prompt the agent to ask for your identity
2. Respond with your user ID (e.g., "bl", "jane", "bob")
3. The agent loads your working style and adapts to your preferences
4. Work proceeds according to your defined protocols

### Directory Structure

```
working-styles/
├── README.md           (this file)
└── {user-id}/          (one directory per user)
    ├── working-style.md (core preferences and protocols)
    └── persona-*.md     (optional specialized personas)
```

## Available Working Styles

Current working styles in this repository:

- **bl**: Brian Lemay's working style (see working-styles/bl/working-style.md)

## Creating Your Own Working Style

To add your own working style to a project:

1. Create a directory with your identifier: `working-styles/{your-id}/`
2. Create `working-style.md` in that directory documenting:
   - How you prefer to communicate
   - Your review process preferences
   - When you want to be asked vs when agent should decide
   - Session start protocol (if you use personas or other patterns)
   - Session exit protocol (git checks, reflections, etc.)
3. Add any additional files (personas, checklists, etc.) as needed

The AI agent will automatically discover your style directory and offer it as an option.

## Session Flow Example

1. **Session starts**
   ```
   Agent: (checks .claude/current-user file)
   Agent: (finds "bl" in current-user file)
   Agent: (reads working-styles/bl/working-style.md)
   Agent: (follows bl's session start protocol)
   Agent: "Which persona would you like to use? 1. Bootstrapper, 2. Architect Guru..."
   User: "6"
   ```

   **Or if current-user file doesn't exist:**
   ```
   Agent: "Please identify yourself"
   User: "bl"
   Agent: (creates .claude/current-user with "bl")
   Agent: (reads working-styles/bl/working-style.md)
   Agent: (follows bl's session start protocol)
   ```

2. **Agent loads persona and works**
   ```
   Agent: (reads working-styles/bl/persona-6-expert-swe.md)
   Agent: (follows TDD workflow, writes tests first, etc.)
   ```

3. **Session ends**
   ```
   User: "I'm preparing to exit"
   Agent: (follows bl's exit protocol: git status check, doc scan, reflection)
   ```

## For Project Maintainers

To use this working styles system in your project, see the bl-ai-working-styles repository's BOOTSTRAP.md for complete instructions.

The setup includes:
- Symlinking working-styles/ directory
- Configuring user-level permissions
- Setting up Claude Code hooks for post-compaction reminders
- Creating .claude/current-user file
- Updating .gitignore

BOOTSTRAP.md covers three scenarios (new repo, existing repo without CLAUDE.md, existing repo with CLAUDE.md) and includes comprehensive verification steps.

## Benefits

- **Consistency**: Same preferences across all projects
- **Efficiency**: No re-explaining preferences each session
- **Portability**: Single source of truth, linked into projects
- **Collaboration**: Multiple users with different styles in same project
- **Discoverability**: Agent automatically discovers available styles
- **Extensibility**: Easy to add new styles or personas
