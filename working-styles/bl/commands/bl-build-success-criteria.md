You are about to start implementation. STOP. Build success criteria FIRST.

This is not a suggestion. This is a gate. Do not write code until success criteria are defined, reviewed, and added to the current phase's design doc.

---

## Step 1: Define success criteria for the current phase

Write success criteria that cover:

### Functional correctness
- What must work when this phase is done? Be specific -- name the commands, endpoints, UI flows.
- What must produce the same results through different entry points (CLI vs server, API vs UI)?
- What existing behavior must NOT break?

### Documentation is up to date and consistent
- Every file touched or created: does the relevant documentation reflect it?
- Design docs, READMEs, component indexes, field playbooks -- all consistent with code changes.
- If adding a reusable component, hook, or utility: it must be in the reusable-components-index.
- If changing a schema or field: the field-change-playbook trail must match.
- No stale references to removed code, renamed fields, or deleted files.

### ALL tests pass
- Run the targeted test suites for every package touched.
- ALL tests must pass. Not just yours. ALL of them.
- If you find a pre-existing failure that "isn't yours" -- fix it anyway. You own the green bar. There is no "someone else's problem" for test failures. FIX ALL THE TESTS ALWAYS.
- If a test is failing because of a real bug, fix the bug. If the test is wrong, fix the test. Either way, green.
- Capture test output to `./tmp/` and verify. Do not eyeball scrolling terminal output.

### All imports and paths follow project conventions
- Read `design/technical/path-conventions.md` in the project root. That is the spec.
- Shared library imports use `@lib` alias. NEVER relative paths like `../../lib/` or `../../../lib/`.
- Frontend API fetches from `/app/` context use `../api/` (relative, never absolute `/api/`).
- No absolute paths in frontend code.
- Every new package: tsconfig `paths` configured, vitest `resolve.alias` configured if applicable.
- Grep changes for violations before committing.

### Schema, API, and UI changes reviewed by human
- If changing database schema, API endpoints, or user-facing UI behavior: the human must review and approve before commit.
- Do not silently commit schema migrations, new API routes, or UX changes.
- Show the human what changed and get explicit approval.
- Code-only refactors and test additions do not require this gate.

---

## Step 2: Add to the design doc

Write the success criteria into the current phase's design doc, under the implementation checklist. They live with the work they govern.

---

## Step 3: Add close-out checklist items

The LAST items on the implementation checklist must be:

- [ ] Run `/bl-check-work` -- verify all success criteria are met
- [ ] Meticulously review every checklist item. Do not skim. Read each item and verify it was actually completed -- not "probably done" or "I think I did that." Actually check.
- [ ] Check off every box. If a box cannot be checked, explain why and resolve it before closing.

---

## Step 4: Get human approval on the criteria

Show the success criteria to the user. Do not proceed to implementation until they confirm the criteria are correct and complete. The criteria are a contract -- both sides agree on what "done" means before work begins.
