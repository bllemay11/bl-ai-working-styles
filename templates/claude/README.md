# Claude Code Configuration Template

This directory contains templates for Claude Code hooks and configuration.

## Installation

Copy these files to your project's `.claude/` directory:

```bash
# From your project root
mkdir -p .claude/hooks
cp /path/to/bl-ai-working-styles/templates/claude/hooks/post-compaction .claude/hooks/
cp /path/to/bl-ai-working-styles/templates/claude/current-user.example .claude/
chmod +x .claude/hooks/post-compaction
```

## Setup

1. **Create your user identifier file:**
   ```bash
   echo "your-identifier" > .claude/current-user
   ```

2. **Add to .gitignore:**
   ```
   # Claude Code per-user configuration
   .claude/current-user
   .claude/settings.local.json
   ```

3. **Verify hook works:**
   ```bash
   ./.claude/hooks/post-compaction
   ```

## Files

### hooks/post-compaction
- **Purpose:** Displays reminders after conversation compaction
- **Type:** Executable bash script
- **Git:** Check into version control

### current-user.example
- **Purpose:** Template for user identifier
- **Git:** Check into version control

### current-user (not in template)
- **Purpose:** Your user identifier
- **Format:** Single line with identifier (e.g., "bl")
- **Git:** Gitignore this file (per-user)

## How It Works

1. After conversation compaction, Claude Code runs `.claude/hooks/post-compaction`
2. Hook reads user identifier from `.claude/current-user`
3. Hook displays `working-styles/{user}/reminders.md` content
4. Hook optionally displays `design/project-reminders.md` if it exists

## Related Documentation

- Main bootstrap instructions: `../BOOTSTRAP.md`
- Working styles pattern: `../design/working-styles-pattern.md`
