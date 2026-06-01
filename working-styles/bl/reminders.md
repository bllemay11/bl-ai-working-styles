# BL Working Style Reminders

These reminders appear after every conversation compaction to maintain consistency across sessions.

## Communication

- **Medium-verbose, structured.** Walk through reasoning when non-obvious. Use sections for non-trivial answers.
- **One-liner progress updates** at meaningful transitions — no running narration.
- **Numbered options with one-line descriptions** when presenting choices.
- **Never volunteer time/effort estimates.** Only answer if asked directly.
- **No emojis in code or written files.** Plain text only. Standard markdown checkboxes `[ ]` / `[x]` are fine.
- **Always use proper markdown tables** for tabular data — no fake tables built from bullet lists.
- **Don't ask "should I continue?"** — keep going unless blocked or hitting a decision that needs me.

## Review

- **Section-by-section** for long documents. Wait for "good"/"next" before proceeding.
- **Multiple issues:** one-line summary of all upfront, then walk one-by-one.
- **Code changes:** just the diff. Don't claim it works until you've actually run tests / exercised it.
- Short approvals ("yes", "yep", "good", "next", "ok") are full approval to continue.

## Git Workflow — Non-Negotiable

- **NEVER `git add -A` or `git add .`** — stage explicit files only.
- **NEVER push to remote** — pushing is always my job.
- **NEVER commit before I've manually tested** — tests passing isn't enough.
- **Commit format:** imperative summary + body explaining why + `Co-Authored-By: Claude <noreply@anthropic.com>` trailer.

## Decision-Making — Ask, Don't Infer

- **Technical / library / pattern choices:** present numbered options, I pick.
- **UX / UI decisions:** present options, don't just decide.
- **Ambiguous requirements:** stop and surface. Don't fill spec gaps.
- **Non-trivial implementation:** ask about complexity first; write design docs in `design/` before coding.
- **Destructive actions** (rm, force-push, drop, etc.): always ask. Prior approvals don't carry forward.

## Anti-Patterns — Zero Tolerance

1. NEVER `git add -A` or `git add .`
2. NEVER `pkill` or `killall` — only kill processes you specifically started
3. NEVER use `EnterPlanMode` — write plans to `.md` files in `design/` instead
4. NEVER mark work complete without actually running / testing it
5. NEVER commit before I've manually tested
6. NEVER push to remote
7. NEVER use emojis in code or files
8. NEVER volunteer time estimates
9. NEVER ask "should I continue?"
10. NEVER infer requirements that aren't specified
11. NEVER decide tech / pattern choices unilaterally
12. NEVER carry forward approval to a new destructive action

## Testing — TDD Always

- Tests FIRST, then implementation.
- High coverage: happy paths, edge cases, errors, security.
- **Capture-once, analyze-many:** redirect slow test runs to a file, grep/tail without re-running.
- Don't claim tests complete without running them. If blocked, say so.

## Component Reuse — Always Check First

- Before writing any new component / hook / utility, search the codebase for existing ones.
- If `design/reusable-components-index.md` exists, check it.
- When creating new reusable code, update the index immediately.

## Documentation — Living and Documentation-First

- Design docs in `design/` BEFORE non-trivial implementation (numbered prefix pattern, cross-referenced).
- **PRIMARY checklist:** git-tracked `design/phase-N-checklist.md`. Update as you complete tasks.
- After any change that affects existing docs, update them in the same commit.
- **Comments:** default to none. Only when WHY is non-obvious. One short line max.

## Session Protocols

**Session Start**
- Check `.claude/current-user` for identifier.
- If "bl", load `working-styles/bl/working-style.md` and ask which persona to use.

**Session End**
- Run `git status`, show what's in the working tree.
- If uncommitted changes, ask whether to commit (respecting "wait for manual testing" rule).
- Otherwise stop.
