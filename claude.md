# Claude Context - bl-ai-working-styles

## Project Overview

**Project Name:** bl-ai-working-styles
**Purpose:** Reusable AI agent working styles system for consistent cross-project collaboration

This repository contains portable working style definitions that can be linked into any project. It enables:
- Consistent AI agent interactions across multiple projects
- Reusable working style definitions (no re-explaining preferences)
- Multiple users with different styles in the same project
- Single source of truth for working style documentation

## Critical Rules

1. **NO EMOJIS:** Never use emojis in any files. Standard markdown checkboxes `[ ]` and `[x]` are acceptable.
2. **FOLLOW WORKING STYLES:** This project uses the bl working style. At session start, load from `working-styles/bl/working-style.md`.
3. **NO VERIFICATION SCRIPT:** Verification is manual only (documented in BOOTSTRAP.md). Don't ask about creating verification scripts.
4. **BOOTSTRAP REFERENCE:** All setup instructions are in BOOTSTRAP.md. Reference this file when discussing setup or installation.
5. **KEEP DOCS CURRENT:** After changes, update this file and design docs to reflect current state.

## Project Structure

```
bl-ai-working-styles/
├── claude.md                       (this file - instructions for working IN this repo)
├── claude-template.md              (example of minimal pointers for project CLAUDE.md)
├── BOOTSTRAP.md                    (Claude instructions for bootstrap/verify)
├── .gitignore                      (standard ignores)
├── LICENSE                         (project license)
├── design/                         (design documentation)
│   └── working-styles-pattern.md   (complete design + checklist)
├── templates/                      (templates for projects)
│   └── claude/                     (Claude Code configuration templates)
│       ├── README.md               (template usage instructions)
│       ├── current-user.example    (example user identifier file)
│       └── hooks/
│           └── post-compaction     (hook that displays reminders after compaction)
└── working-styles/                 (portable working styles)
    ├── README.md                   (explains the system)
    └── bl/                         (bl's working style)
        ├── working-style.md        (core style + session protocols)
        ├── reminders.md            (condensed reminders for post-compaction)
        ├── persona-1-bootstrapper.md
        ├── persona-2-architect-guru.md
        ├── persona-3-doc-reviewer.md
        ├── persona-4-data-modeler.md
        ├── persona-5-technical-reviewer.md
        ├── persona-6-expert-swe.md
        ├── persona-7-refactoring-agent.md
        ├── persona-7-loop-instructions.md
        ├── persona-8-qa-hackaroo.md
        ├── persona-10-operator.md
        ├── infinite-loop-refactoring-agent.md
        └── commands/                  (portable slash commands)
            ├── README.md              (installation and usage)
            ├── bl-start-work.md       (kick off implementation)
            ├── bl-check-work.md       (verify phase completion)
            ├── bl-reminders.md        (critical reminders display)
            ├── bl-session-id.md       (find session ID + create alias)
            ├── bl-add-session-to-bashrc.md (add session alias to bashrc)
            ├── bl-test-report.md      (run all test suites, produce report)
            └── bl-nest-project.md     (move a local project into the ooc-team repo)
```

## Design Documentation

**Primary design document:** `./design/working-styles-pattern.md`

This document contains:
- Problem statement and solution architecture
- Component descriptions (template, bootstrap, working-styles/)
- Key design decisions (symlinks, discovery, minimal template)
- Implementation checklist
- Verification steps
- Success criteria

## How Projects Use This

Projects that want to use these working styles:

1. **Symlink** the working-styles/ directory into the project
2. **Add minimal pointers** to project's CLAUDE.md (see claude-template.md for example)
3. **Add** working-styles to project's .gitignore
4. **Session starts** with user identification, agent loads from working-styles/{user}/

**For Claude:** See BOOTSTRAP.md for complete instructions on bootstrapping projects (3 scenarios) and verifying installations.

## Working in This Repository

When working in bl-ai-working-styles itself:
- Follow bl working style (load from working-styles/bl/working-style.md)
- Update design docs when structure changes
- Keep claude.md (this file) current
- Test changes by linking into test projects
- No emojis in any files

## Git Workflow

- Commit changes with descriptive messages
- Use standard git commit format:
  ```
  Commit message summary

  Details...

  Generated with Claude Code

  Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
  ```
- Stage specific files only (never `git add -A` or `git add .`)
- Never push without user permission

## Current Status

**Implementation Phase:** Core structure complete
**Files Created:**
- claude.md (this file)
- claude-template.md (minimal pointers example)
- BOOTSTRAP.md (Claude bootstrap/verify instructions with hook verification)
- .gitignore
- design/working-styles-pattern.md (complete design + checklist)
- templates/claude/ (hook templates and configuration examples)
- working-styles/README.md (system explanation)
- working-styles/bl/ (working-style.md, reminders.md, personas, loop files)

**Next Steps:** Test pattern by linking into a real project
