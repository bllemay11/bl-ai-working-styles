# Infinite Loop Refactoring Agent Instructions

## Overview

This document describes how to run Claude Code as an autonomous infinite loop refactoring agent that continuously scans for and eliminates code duplication without stopping.

## Core Principles

### 1. No Feedback Unless Asked
- **CRITICAL**: Do NOT give status updates, progress reports, or commentary
- Do NOT say things like "I'm going to..." or "Let me check..."
- Work silently - the only output should be commits appearing in git history
- Exception: Only speak when explicitly asked by the user

### 2. Infinite Loop Pattern
The agent follows this loop indefinitely:
1. Scan codebase for duplication patterns
2. Identify refactoring opportunity
3. Extract/refactor the duplication
4. Run tests to verify
5. Commit changes if tests pass
6. Update refactor.log
7. **Immediately return to step 1 - DO NOT STOP**

### 3. Avoid Permission-Requiring Commands
- Do NOT use commands that require user approval
- Pre-approved commands you CAN use freely:
  - `npm test`
  - `git add`, `git commit` (but NOT `git push`)
  - File read/write operations
  - `grep`, `find`, `wc`, etc.
- Commands to AVOID:
  - Interactive git operations (`git commit -i`, `git rebase -i`)
  - Operations requiring user input
  - Anything that would pause execution

## Initial Prompt Template

```
You are an infinite loop refactoring agent. Your task:

1. Continuously scan the codebase for code duplication and opportunities to extract shared logic
2. When you find duplication:
   - Extract it into a shared utility, hook, or component
   - Run all tests to verify nothing broke
   - Commit the change with a descriptive message
   - Update refactor.log with the commit hash and description
3. Immediately continue scanning - DO NOT STOP OR PAUSE
4. Do NOT give feedback, status updates, or ask questions
5. Work autonomously and silently

CRITICAL RULES:
- DO NOT give any commentary or status updates
- DO NOT pause to ask for input
- DO NOT stop after completing a refactoring
- DO NOT use commands that require permission
- YOU ARE AN INFINITE LOOP - keep working until explicitly told to stop

You have access to the full codebase. Tests are in `src/**/*.test.ts*` files.
There are currently X passing tests and Y pre-existing failures.

Begin scanning now. Work silently.
```

## What Makes the Loop Work

### Silence is Key
The main thing that breaks the infinite loop is generating output that prompts the user for a response. To maintain the loop:
- Never describe what you're about to do
- Never ask "Should I...?"
- Never say "I found..." or "Let me..."
- Just do the work and commit

### Rapid Iteration
- Make small, focused refactorings (one pattern at a time)
- Don't try to plan multiple refactorings in advance
- After each commit, immediately start scanning again
- Don't batch multiple refactorings before committing

### Test-Driven Safety
- Always run tests after making changes
- Only commit if tests still pass
- If tests fail, revert and try a different approach
- Never commit broken code

## Example Session

Here's what a successful infinite loop session looks like (from git history):

```
commit 09bf72a - Extract alias rendering into shared utility function
commit b1f4be5 - Extract lifecycle rendering into shared utility functions
commit c3d4e5f - Extract form validation helper
commit f6g7h8i - Consolidate error handling utilities
... (continues indefinitely)
```

The key is that each commit happens automatically with no user interaction.

## Refactor.log Format

Maintain `/Users/nate.fitch/projects/allthefish/refactor.log` with this format:

```
<commit-hash> - <description> (<metrics>)
```

Example:
```
09bf72a - Extract alias rendering into shared utility function (57 lines duplicated → 30-line utility + 2 lines usage, +3 tests)
b1f4be5 - Extract lifecycle rendering into shared utility functions (50 lines duplicated → 2 utilities + formatter functions)
```

## Common Duplication Patterns to Look For

1. **Repeated JSX patterns** - Extract into components or render utilities
2. **Similar hooks** - Extract common data fetching patterns
3. **Duplicated validation logic** - Centralize into validators
4. **Repeated type definitions** - Extract shared types
5. **Similar function implementations** - Extract generic versions
6. **Test setup duplication** - Extract test helpers
7. **API call patterns** - Extract request utilities
8. **Error handling** - Consolidate error handlers

## When to Stop Scanning an Area

Move on to a new area if:
- The remaining duplication is < 5 lines
- Abstracting would make code less clear
- The duplication is in domain-specific logic that shouldn't be generalized
- You've already refactored it in a previous pass

## Handling Failures

If a refactoring breaks tests:
1. Revert the changes immediately (`git restore`)
2. Delete any new files created
3. Move on to scan for a different pattern
4. Do NOT stop the loop
5. Do NOT explain what went wrong

## Example Commit Messages

Format:
```
<Title line - what was extracted>

<Details about the refactoring>
<Metrics: lines saved, new files created, tests added>

Generated with Claude Code

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

Example:
```
Extract lifecycle rendering into shared utility functions

Extracted duplicated lifecycle rendering patterns from TeamDetail and ServiceDetail.
Previously 50 lines duplicated across both files.

Added to lifecycleColors.ts:
- formatTeamLifecycle() - capitalize first letter
- formatServiceLifecycle() - replace dashes, capitalize words

Added to fieldRenderers.tsx:
- renderLifecycleBadge() - colored border badge
- renderLifecycleField() - colored underline display

All 768 tests passing (10 pre-existing failures).

Generated with Claude Code

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

## Debugging a Stuck Loop

If the loop stops working:

1. **Check if you're generating feedback** - Review last message for any commentary
2. **Check for permission-requiring commands** - Did you use an interactive command?
3. **Check if tests are passing** - Is the test suite broken?
4. **Resume the loop explicitly** - Tell Claude: "Continue the infinite loop without feedback"

## Success Metrics

A successful infinite loop session:
- Multiple commits per hour
- No pauses for user input
- Steadily decreasing code duplication
- All tests remain passing
- Clean git history with atomic commits

## Anti-Patterns to Avoid

❌ "I found 5 patterns we could refactor. Should I proceed?"
❌ "Let me analyze the codebase first..."
❌ "I'll start by checking X, then Y, then Z..."
❌ "The tests are passing. What would you like me to do next?"
❌ Batching multiple refactorings before committing

✅ <silent work>
✅ <commit>
✅ <silent work>
✅ <commit>
✅ (repeat forever)
