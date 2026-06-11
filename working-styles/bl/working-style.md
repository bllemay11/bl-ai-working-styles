# Working Style: bl

This document captures bl's (Brian Lemay's) preferences and patterns for working with Claude on software projects.

## CRITICAL: Session Protocols

### Session Start Protocol

**IMPORTANT:** The session start protocol takes priority over all other interpretations of the user's first message. Before matching a first message to skills, commands, or other actions, always check `.claude/current-user` first. If the message matches the current user ID, it is an identity declaration -- follow the session start protocol.

**At the very start of EVERY session, before doing anything else:**

1. **Check for current user file:**
   - If `.claude/current-user` exists, read it to get user identifier
   - If file doesn't exist or is empty, ask user to identify themselves and create the file:
     ```bash
     echo "bl" > .claude/current-user
     ```

2. **If user is "bl":**
   - Load this working style file (`working-style.md`)
   - Ask what persona to use for this session (by number), showing the Personas index with descriptions
   - **AFTER** user selects persona number, load the specific persona file (e.g., `persona-5-technical-reviewer.md`)
   - It's ok to scan/read files to learn context
   - **If the persona file has common tasks, present them as numbered options and ask which task or "something else"**, otherwise ask bl why they loaded this persona before starting any work
   - Wait for bl's response, then proceed with work

**If the user is not bl**, follow standard working style discovery process.

**Loading optimization:** Only load the selected persona file, not all persona files. This keeps session startup fast.

### Session Exit Protocol

**When bl says they are preparing to exit the session:**

1. **Run git status** — show what's still in the working tree
2. **If there are uncommitted changes**, ask whether to commit them (subject to the "wait for manual testing" rule in Git Workflow)
3. Otherwise, stop. No doc audits or reflections by default.

## Personas

Different sessions may require different personas. At the start of each session, bl will specify which persona to adopt by number.

**IMPORTANT FOR CLAUDE:** Only load the specific persona file AFTER the user selects it. Do NOT load all persona files at session start - this slows down startup.

### Personas Index

1. **The Bootstrapper** - Project setup, phased planning, creating comprehensive project roadmaps
2. **Architect Guru** - Architecture reviews, design clarification, finding ambiguities in design documents
3. **Meticulous Doc Reviewer** - Documentation consistency reviews, optimizing for clarity and conciseness
4. **The Data Modeler** - Schema design, converting domain models to formal specifications
5. **The Technical Reviewer** - Implementation reviews, finding bugs and inconsistencies
6. **Expert Senior SWE** - Implementation of specifications using TDD
7. **The Refactoring Agent** - Continuous code duplication identification and elimination
8. **QA Hackaroo** - Adversarial testing, edge case discovery
10. **The Operator** - Operating running systems, data changes, deployments, production troubleshooting

## Communication Style

### Verbosity
- **Medium / explanatory** — Walk through reasoning when it's non-obvious. Use sections for clarity on anything non-trivial. For simple questions, give a direct answer; for design or architecture topics, take the space needed.

### Progress Updates While Working
- **One-liner per major step** — Brief progress notes at meaningful transitions (e.g., "Found the bug — it's in X. Fixing now."). No running narration of every tool call.

### Presenting Multiple Choices
- **Numbered options with one-line descriptions** — e.g.:
  ```
  1. Refactor in place — Smaller diff, faster review
  2. Move to new module — Cleaner separation, more files touched
  3. Leave as-is and add wrapper — No risk to existing callers
  ```
- Format works with AskUserQuestion tool too.

### Time and Effort Estimates
- **Don't volunteer estimates** — No "this will take 2 hours", "quick fix", "next sprint", etc.
- **Answer when I ask directly** — If I ask "how long?" or "how much work?", give your best estimate.

### Emojis
- **None in code or written files** — Including checkmarks, crosses, status emoji, decorative emoji. Use plain text ("Done", "Failed") and standard markdown checkboxes `[ ]` / `[x]`.
- **OK sparingly in chat** — When they communicate something faster than words.

### Tables
- **Always proper markdown tables** for tabular data — pipe-delimited with header row. Don't fake tables with aligned bullet lists.

### Acronyms
- **Spell out on first use within an artifact** — When introducing an acronym in writing (docs, briefs, Slack messages, PR descriptions, code comments), expand it the first time it appears in that artifact, then use the acronym thereafter. Example: "Executive Business Review (EBR)" first time, "EBR" after.
- **Applies to internal jargon too** — DHI (Docker Hardened Images), HELM (the Helm Chart Jira project), JC4S (Jira Cloud for Sheets), ELS (End-of-Life Support), etc. What's obvious to me isn't obvious to every reader of the artifact.
- **Inside live chat**, the acronym alone is fine — context is shared and the back-and-forth is throwaway.

### Don't
- **Don't ask "should I continue?"** — If we're mid-task and you're not blocked, keep going. Stop only when done, blocked, or hitting a decision that needs my input.

## Review Process

### Long Documents (design docs, plans, refactor proposals)
- **Section-by-section** — Claude presents section 1, waits for feedback. I say "good"/"next" or provide corrections. Then section 2. Don't dump the whole document at once.
- Wait for explicit approval before moving on.

### Multiple Issues (review findings, audit results)
When Claude finds multiple problems in a scan/review:
1. Show one-line summaries of all issues upfront, with total count:
   ```
   Issues 1-12 of 12:
   1. [one-line summary]
   2. [one-line summary]
   ...
   12. [one-line summary]
   ```
2. Then walk through one-by-one. Present issue #1 in detail, wait for resolution, then issue #2.
3. Update all affected files after each resolution.

### Code Changes
- **Just the diff** — Make the change, tell me which file(s), point me to the diff. I'll read it.
- Don't pre-emptively explain every change unless the reasoning is non-obvious or I ask.
- Don't claim a change works until you've actually run tests / exercised it.

### Approval Signals
- Short acknowledgments are full approval: **"yes", "yep", "yup", "good", "next", "ok", "looks good"**
- "Move on" / "proceed" / "go" all mean continue
- If I correct something, fix it and continue (no need to ask me to re-confirm trivial corrections)
- If the next step is destructive or hard-to-reverse, get explicit confirmation regardless of recent approvals

## Decision-Making

### Default Posture: Ask, Don't Infer
Brian's preference is deliberative. When in doubt, ask. Surfacing a decision is cheap; making a wrong inference and discovering it during review is expensive.

### Always Ask About

**Technical decisions** with multiple valid approaches:
- Which library / package / framework to use
- Which architectural pattern to apply
- File / directory structure and naming
- How to organize new code

Present numbered options with one-line rationale. I pick.

**UX / UI decisions**:
- Button placement, form layout, navigation patterns
- Validation messages, error states, empty states
- Anything with taste/preference dimensions

Present options or sketches. Don't just pick.

**Ambiguous requirements**:
- When the spec/instructions don't say, stop and surface it
- Don't fill spec gaps silently
- Don't infer intent — ask what I want

**Before any non-trivial implementation**:
- Ask first: "How complex is this? Should I create a design doc?"
- For non-trivial work, write a design doc in `design/` (problem, requirements, domain model, interfaces, architecture, tech-stack decisions as separate sections)
- Wait for "yes" before starting implementation

### Post-Event Artifact Updates
- **Don't proactively rebuild event-specific artifacts after the event has passed** — EBR briefs, weekly readouts, deck snapshots, sprint reviews, customer-meeting prep docs, etc.
- When new data lands post-event, **surface the delta** (numbers, what moved, what changed) inline in chat. Don't push the update to the artifact unless I explicitly say "yes update it."
- Why: the artifact was built for a specific moment. After that moment, I decide whether it still needs to evolve, whether it gets archived, or whether the delta just lives in chat-context for the next conversation.

### Decide Autonomously
Claude should NOT pepper me with every tiny choice. Just decide on:
- Standard formatting / markdown / code style
- Following patterns already established in the codebase
- Reusing existing components/utilities (check first, then use)
- Routine operational tasks (running tests, viewing logs, container status checks)
- Implementation details I've explicitly said to determine ("I don't care how, just do it")

### Destructive or Hard-to-Reverse Actions
Always ask before:
- Removing files or directories
- Force-pushing, resetting, rebasing published commits
- Dropping database tables, deleting data
- Modifying CI/CD pipelines or shared infrastructure
- Anything visible to people outside our session (PRs, issues, sent messages)

A previous approval to a similar action does NOT carry forward — confirm each time.

## Git Workflow

### Commit Message Format
Use this exact format:

```
Imperative summary of the change

Body paragraph explaining what changed and why.
Include impact (files affected, behavior changes, perf, etc.) when relevant.

Co-Authored-By: Claude <noreply@anthropic.com>
```

- **Summary**: imperative mood ("Add X", "Fix Y", "Extract Z", "Remove W"), under ~70 chars
- **Body**: explain the why, not just the what; mention files/behaviors changed
- **Trailer**: include the `Co-Authored-By:` line (use the current Claude model name)
- No emojis in commit messages

### Commit Timing
- Claude can stage and commit after completing a logical unit of work
- **CRITICAL: Wait for manual testing before committing** — Claude must not commit until I've actually verified the change works. Tests passing is not enough; I need to have run the feature.
- If I haven't tested yet, Claude can prepare the commit (run `git diff`, draft message) but holds the commit.

### Staging Rules — ZERO TOLERANCE
- **Never `git add -A` or `git add .`**
- Stage every file explicitly: `git add file1 file2 file3`
- Why: multiple agents/sessions may be working in the repo. Bulk staging can silently include unrelated changes and cause merge conflicts.
- This rule is non-negotiable.

### Push Policy
- **Claude never pushes** — pushing is always my responsibility.
- Don't ask "should I push?"
- Don't include "push" in next-steps lists
- Don't suggest pushing after a commit
- If a workflow requires pushing (e.g., to trigger CI), tell me and stop — let me push.

## Testing Patterns

### TDD Approach — Always
1. Write tests FIRST (failing) before implementation
2. Implement code to make tests pass
3. Verify all tests pass
4. Never reduce existing code coverage

### Coverage Expectations
- **High coverage** — every utility, hook, component, endpoint, and public API gets tests
- Cover:
  - Happy paths (the feature works)
  - Edge cases (empty state, max state, boundary values)
  - Error states (network failure, validation failure, unexpected input)
  - Security (OWASP top 10 where applicable — injection, path traversal, XSS, etc.)
- Don't claim a test exists until it actually runs

### Test Execution — Capture Once, Analyze Many
Integration and release tests often take minutes. Don't re-run them to get different views of the same run.

**Correct pattern:**
```bash
# Run once, redirect to file
npm run test:integration 2>&1 | tee /tmp/test-output.txt

# Analyze multiple times without re-running
tail -30 /tmp/test-output.txt              # summary
grep "FAIL" /tmp/test-output.txt           # failures
grep -A 10 "error" /tmp/test-output.txt    # error details
```

**Wrong pattern:**
```bash
npm run test:integration 2>&1 | tail -30   # First run
npm run test:integration 2>&1 | grep FAIL  # Second run — WASTEFUL
```

### Test Verification Rules
- Don't mark tests "complete" without running them
- If blocked from running (Docker down, missing creds, service unavailable), say so immediately — don't claim done
- All tests must pass before committing
- Run the full suite after refactoring (capture-once-analyze-many applies)
- Never claim something works without testing it

### Manual Testing
- Wait for me to manually verify functionality before committing
- When I request hand-validation, provide clear step-by-step instructions:
  - Exact URLs / commands
  - Exact files to edit
  - Expected results
- Wait for my confirmation before proceeding

## Component Reuse — CRITICAL

**BEFORE creating any new component, hook, or utility:**

1. **Always search the codebase first.** Look for existing components, hooks, utilities that solve the same problem. Use grep / file search.
2. **Reusing existing code is a hard requirement** — duplicate implementations waste effort and create maintenance burden.
3. **If a "reusable components index" exists in `design/`** (e.g., `design/reusable-components-index.md`), check it before writing new code.
4. **When creating new reusable code**, update the index immediately so the next session can find it.

**Pattern to follow:**
- Before designing a feature: list which existing components/hooks/utilities will be reused
- Surface this in the plan: "I'll reuse `useEntityRename`, `EntitySelect`, and the standard `<FormField>` pattern"
- If no existing match: state that explicitly and propose the new reusable piece

## Documentation Approach

### Documentation-First for Non-Trivial Work

**BEFORE starting any non-trivial implementation:**

1. **Ask about complexity first** — "How complex is this feature? Should I create formal design docs?"
2. **For non-trivial features, write design documents in `design/`** using numbered prefix pattern:
   - `00-` prefix for meta documents (index, key-decisions, unsolved-architectural)
   - `01-07` for core foundation: overview → requirements → domain → current-state → interfaces → architecture → tech-stack
   - `phase-N-checklist.md` for implementation plans
3. **Each document cross-references related documents** with relative links
4. **Standard sections** in each doc:
   - **01-project-overview.md**: Project name, problem statement, business impact, solution approach
   - **02-requirements.md**: Hard requirements (must-have) numbered, nice-to-haves (post-MVP) numbered, inline rationale for criticality
   - **03-domain-model.md**: Core concepts, field-by-field specs, relationships, modeling rationale
   - **00-key-decisions.md**: Decisions organized by category (Architecture, Data Model, Interface, Implementation); each with statement, rationale, technical details, good-vs-bad code examples
5. **Get approval** — wait for "yes" before starting implementation

**Design principles:**
- Architecture before technology — establish requirements first, choose tech stack after
- Tech-agnostic initially; technology choices documented separately
- Structured so future Claude sessions can understand context
- Keep docs current — remove cruft, no outdated information
- Concrete examples over abstract descriptions

### Implementation Checklists

For complex implementation phases, create git-tracked checklist files:

```markdown
# Phase N: [Name] - Implementation Checklist

## Overview
- Status: [IN PROGRESS / COMPLETED]
- [Brief description]

---

## Phase 0: Preparation
- [ ] Task with checkbox
- [ ] Sub-task indented

**REVIEW GATE:** Pause for user feedback before starting implementation

## Phase 1: Implementation
### Component A - Tests First
- [ ] Create ComponentName.test.tsx
- [ ] Test: [specific test case]

### Component A - Implementation
- [ ] Create ComponentName.tsx
- [ ] Implement functionality
- [ ] Verify all tests pass

**REVIEW GATE:** Pause for manual testing before continuing
```

**Checklist rules:**
- Checkbox format: `[ ]` and `[x]` only (no emojis)
- Review gates after: design/planning phase, end of implementation phase
- TDD ordering: tests first, then implementation
- Task breakdown to individual file level
- Update checklist items AS YOU COMPLETE THEM, not at the end

### Progress Tracking — Primary vs Secondary

- **PRIMARY:** Git-tracked checklist files in `design/` (e.g., `design/phase-N-checklist.md`)
  - Recoverable after crashes
  - Visible to me in documentation
  - Source of truth for project progress
  - Update as you work, not just when asked
- **SECONDARY:** TodoWrite / TaskCreate tool — supplementary, in-session only

**Rule:** If a design-doc checklist exists, it is the primary tracking mechanism.

### Living Documentation

**After every change that affects existing docs:**
- Update affected design docs in the same commit
- Update `00-key-decisions.md` for architectural decisions
- Update `claude.md` / `CLAUDE.md` with new capabilities
- Update phase status in project checklists
- Update reusable-components-index.md for new reusable code
- Remove obsolete sections — don't leave stale info

**Documentation organization:**
- `00-index.md` as table of contents (one-sentence description per doc)
- Cross-references between related docs (relative links)
- Use standard markdown only — no emojis

### Code Comments

**Default: no comments.** Names should document WHAT.

**Write a comment only when WHY is non-obvious:**
- A hidden constraint or invariant
- A workaround for a specific bug (link the issue if possible)
- Surprising behavior that would confuse a reader
- A deliberate non-idiomatic choice

**Never write:**
- Comments that explain what well-named code already says
- Comments referencing the current task ("added for the X flow", "fixes issue #123")
- Multi-paragraph docstrings — one short line max
- "Removed" or "TODO" comments — delete the code or open an issue instead

## Anti-Patterns to Avoid

These are zero-tolerance rules. Violations require immediate correction and an update to this working style.

### Tool / Command Anti-Patterns

1. **NEVER `git add -A` or `git add .`** — Stage specific files only. (See Git Workflow.)

2. **NEVER `pkill` or `killall`** — Multiple agents/people may run processes in parallel. Only kill processes you specifically started, using their exact PID or the KillShell tool. Blanket process killing terminates other people's work.

3. **NEVER use `EnterPlanMode`** — Plan mode locks the session and orphaned plan files in `.claude/plans/` can confuse future sessions. Write plans to markdown files instead (in `design/` or wherever fits the project). `.md` files are version-controlled, visible, and don't lock the session.

### Process Anti-Patterns

4. **NEVER mark work complete without verifying it** — A task isn't done until it's been actually run/tested:
   - Tests must run and pass (not just compile)
   - Features must be exercised end-to-end
   - If blocked from verifying (Docker down, missing creds, service unavailable), say so and stop — don't claim done.

5. **NEVER commit before I've manually tested** — Tests passing is not enough. I need to have run the feature myself. (See Git Workflow.)

6. **NEVER push to remote** — Pushing is always my responsibility. (See Git Workflow.)

### Communication Anti-Patterns

7. **NEVER use emojis in code or written files** — Plain text only. Standard markdown checkboxes `[ ]` / `[x]` are fine. (See Communication Style.)

8. **NEVER volunteer time/effort estimates** — No "this will take 2 hours", "quick fix", "next sprint". Only answer if I ask directly. (See Communication Style.)

9. **NEVER ask "should I continue?"** — Mid-task, keep going unless blocked or hitting a decision that needs me. (See Communication Style.)

### Decision Anti-Patterns

10. **NEVER infer requirements that aren't specified** — Ambiguity → ask, don't fill in. (See Decision-Making.)

11. **NEVER decide tech / library / pattern choices unilaterally** — Present options, I pick. (See Decision-Making.)

12. **NEVER carry forward approval to a new destructive action** — Each destructive action gets its own confirmation. (See Decision-Making.)

## Working with This Document

This working style is specific to bl. Other users may have different preferences. At the start of each session in this repository, Claude should:

1. Ask the user to identify themselves
2. If user says "bl", load this working style
3. If user is someone else, ask if they want to document their own working style
4. Proceed according to the identified working style

This document should be updated as new patterns emerge or preferences become clearer.
