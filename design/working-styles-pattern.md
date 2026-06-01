# AI Agent Working Styles Pattern

## Problem Statement

Different users have different preferences for how they work with AI agents:
- Communication style (verbose vs concise)
- Review processes (section-by-section vs wholesale)
- Decision-making approaches (ask vs infer)
- Session protocols (start/exit procedures)
- Specialized personas for different tasks

Currently, these preferences must be re-explained in every project and every session, leading to:
- Wasted time re-establishing preferences
- Inconsistent experiences across projects
- No reusability of working style definitions
- Difficulty onboarding new collaborators with different styles

## Solution: Portable Working Styles Pattern

Create a reusable working styles system that can be linked into any project:

1. **Source repository** (bl-ai-working-styles): Contains working style definitions
2. **Symlink pattern**: Projects link to working-styles/ directory
3. **Minimal template**: Projects use lightweight CLAUDE.md that references linked styles
4. **Discovery mechanism**: AI agent discovers available styles by directory listing
5. **Session protocols**: Defined in working styles themselves, not project files

## Architecture

```
bl-ai-working-styles/              (source repository)
├── claude.md                       (instructions for working IN this repo)
├── claude-template.md              (template for projects to copy)
├── BOOTSTRAP.md                    (Claude bootstrap/verify instructions)
├── .gitignore                      (standard ignores)
├── design/                         (design docs)
│   └── working-styles-pattern.md   (this file)
├── templates/                      (templates for projects)
│   └── claude/                     (Claude Code configuration templates)
│       ├── README.md               (template usage instructions)
│       ├── current-user.example    (user identifier template)
│       └── hooks/
│           └── post-compaction     (displays reminders after compaction)
└── working-styles/                 (portable working styles)
    ├── README.md                   (explains the system)
    └── bl/                         (bl's working style)
        ├── working-style.md        (core style + session protocols)
        ├── reminders.md            (condensed reminders for post-compaction)
        ├── persona-*.md            (specialized personas)
        └── infinite-loop-refactoring-agent.md

project-repo/                       (any project using the pattern)
├── CLAUDE.md                       (from template - minimal, project-specific)
├── .gitignore                      (ignores working-styles/ and .claude/current-user)
├── .claude/
│   ├── current-user                (user identifier - gitignored)
│   └── hooks/
│       └── post-compaction         (copied from templates)
└── working-styles/ -> /path/to/bl-ai-working-styles/working-styles/
```

## Components

### 1. claude-template.md
Minimal template for project CLAUDE.md files. Contains only:
- Reference to working-styles/README.md for session startup instructions
- Everything else is project-specific

The template is intentionally minimal - all working style logic lives in working-styles/README.md

### 2. BOOTSTRAP.md
Instructions for Claude (AI agent) on how to bootstrap the pattern in a project. Human-readable so users can understand the process. Covers three scenarios:
- **Scenario A**: Brand new empty repository
- **Scenario B**: Existing repository with no CLAUDE.md
- **Scenario C**: Existing repository with existing CLAUDE.md (merge/append)

Includes:
- User-level permissions configuration in ~/.claude/settings.json
- Symlink creation commands (handling different starting locations)
- Claude hooks setup (.claude/hooks/post-compaction)
- Current user file creation (.claude/current-user)
- .gitignore modifications (careful appending, not overwriting)
- Verification steps (10 steps including hooks, symlinks, permissions)
- Debugging guidance

### 3. working-styles/README.md
Explains the working styles system itself:
- What working styles are
- How they work (including .claude/current-user optimization)
- How to add new styles
- Session flow examples
- Instructions for AI agents at session start

### 4. working-styles/bl/
Complete bl working style:
- working-style.md: Core style, session protocols (including .claude/current-user check), personas index
- reminders.md: Condensed reminders displayed after conversation compaction
- Persona files: Specialized modes for different tasks
- infinite-loop-refactoring-agent.md: Autonomous refactoring mode

### 5. templates/claude/
Templates for Claude Code hooks and configuration that projects copy:
- hooks/post-compaction: Displays reminders after conversation compaction
- current-user.example: Example user identifier file
- README.md: Installation and usage instructions

### 6. claude.md (for bl-ai-working-styles repo)
Meta-level instructions for working IN this repository:
- Project structure explanation
- Reference to this design doc
- Reference to BOOTSTRAP.md
- Standard working styles session protocols apply

## Key Design Decisions

### Symlinks in .gitignore
When using symlinked working-styles, add to .gitignore:
```
# AI agent working styles (symlinked)
working-styles
```

This prevents committing the symlink while allowing each developer to point to their own bl-ai-working-styles location.

### Dynamic Style Discovery
Template uses directory listing to discover available styles:
```
Available working styles: {ls working-styles/}
```

This allows multiple users to coexist (bl, jane, bob, etc.) without template changes.

### Minimal Template Philosophy
Keep claude-template.md minimal - only entrance protocol. This is meta-programming the AI agent interaction layer, not technical implementation. Projects add their own:
- Technology choices
- Development environment
- Project-specific rules
- Git workflows

### Exit Protocols in Working Styles
Session exit protocols (git status check, doc scan, reflection) belong in individual working-style.md files, not the template. This allows different users to have different exit preferences.

### Bootstrap Safety
BOOTSTRAP.md emphasizes careful handling of existing repos:
- Check for existing .gitignore before appending
- Check for existing CLAUDE.md before creating/merging
- Provide clear verification steps
- Include rollback instructions

## Implementation Checklist

### Phase 1: Core Structure
- [x] Create design/ directory
- [x] Create this design doc
- [x] Create initial claude.md referencing design

### Phase 2: Copy Working Styles
- [x] Create working-styles/bl/ directory
- [x] Copy all files from allthefish/working-styles/bl/
- [x] Copy infinite-loop-refactoring-agent.md from allthefish root to working-styles/bl/
- [x] Verify all files copied correctly

### Phase 3: Documentation
- [x] Create working-styles/README.md (explain system)
- [x] Create claude-template.md (minimal entrance protocol)
- [x] Create BOOTSTRAP.md (all 3 scenarios + verification)

### Phase 4: Finalization
- [x] Create .gitignore for bl-ai-working-styles
- [x] Update claude.md with complete structure and references
- [x] Review all files for consistency

### Phase 4b: Extract Comprehensive Patterns
- [x] Scan allthefish for design patterns, checklists, git history
- [x] Integrate comprehensive patterns into working-style.md
- [x] Add 378 lines of detailed guidance (design, testing, component reuse, etc.)

### Phase 5: Testing (with allthefish)
- [x] Create backup of allthefish/working-styles/
- [x] Symlink bl-ai-working-styles/working-styles into allthefish
- [x] Update allthefish/.gitignore
- [x] Configure user-level permissions in ~/.claude/settings.json
- [x] Test: start new session, verify working style loads from symlink
- [x] Test: verify persona files accessible
- [x] Test: verify agent can work normally with linked files

## Verification Steps

After bootstrap, verify (see BOOTSTRAP.md for complete verification procedure):
1. User-level permissions configured in ~/.claude/settings.json
2. Symlink exists and points to correct target
3. Symlink accessible (can list and read files)
4. CLAUDE.md has working styles section
5. .gitignore includes working-styles/ and .claude/current-user
6. Claude hooks directory exists with post-compaction hook
7. Hook is executable
8. Current user file exists and contains user identifier
9. User reminders file accessible via symlink
10. Hook execution test passes

## Success Criteria

- bl-ai-working-styles contains complete, portable working styles
- Projects can link to working styles without copying
- Single source of truth for working style definitions
- New projects can bootstrap in under 5 minutes
- Pattern works for multiple users in same project
- AI agents can discover and load styles automatically
