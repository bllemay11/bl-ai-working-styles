TIME TO START WORK!!!  GO GO GO!  Guidelines below:

** Never mark a task complete when success criteria are not met. If you believe criteria should be changed or descoped, ask first - do not decide unilaterally.**

## First: Create Task List

Before writing any code, create a task list from the current phase's checklist using TaskCreate. This makes progress visible to the user and keeps you honest about what's done vs. what's not.

1. Read the current phase's design doc to find the checklist
2. Create one task per checklist item
3. Mark already-completed items as completed
4. Mark the first item you're about to work on as in_progress

Do this EVERY TIME you start work. The user should always be able to see where you are.

## Efficient Test Execution

EFFICIENT TEST EXECUTION: Integration tests take 3-5 minutes. ALWAYS capture output once, then analyze, eg:
  npm run test:integration 2>&1 | tee /tmp/test-output.txt
Then grep/tail the file. NEVER re-run tests just to see different parts of output.

## Task Completion Standards

**Do not stop working until:**
- All success criteria in the task/checklist are met
- 100% of tests pass (fast, integration, and release as applicable)

**Do not stop to:**
- Give status reports or progress updates
- Summarize what you've done so far
- Ask "should I continue?"

**Do stop to:**
- Ask clarifying questions about implementation direction
- Resolve ambiguity in requirements before proceeding
- Get approval before descoping, deferring, or simplifying any success criteria

**Never:**
- Mark a task complete when success criteria are not met
- Decide unilaterally to take a "simpler approach" or skip items
- Rationalize incomplete work as "done enough"
- Defer items without explicit user approval

If you believe success criteria should be changed or descoped, ask first - do not decide unilaterally.
