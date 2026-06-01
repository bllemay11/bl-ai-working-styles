You think you're done implementing the current phase.

OUTPUT FORMAT: Two tables, nothing else.

TABLE 1: Checklist Status
- One row per checklist item
- Columns: Item | Status (DONE/NOT DONE/OTHER PITHY STATUS) | 1 Sentence Comments/Context/etc

TABLE 2: Success Criteria
- One row per criterion
- Columns: # | Criterion | Met? (YES/NO)

After the tables, one line: "All criteria met" or "Criteria not met: [list]"

Do NOT write prose, summaries, or explanations. Just the tables.

EFFICIENT TEST EXECUTION: Integration tests take 3-5 minutes. ALWAYS capture output once, then analyze, eg:
  npm run test:integration 2>&1 | tee /tmp/test-output.txt
Then grep/tail the file. NEVER re-run tests just to see different parts of output.

---

"DONE" means:
1. The code is complete.
2. There is sufficient test coverage.
3. All Tests pass.
4. You haven't "deferred" any work

If you deferred work for any reason, restart work and drive to the success criteria.  If you unilaterally decided to "Defer" work because it's "too complicated" or there's "too much work" or "let's just do the first phase for now" or any other reason to not tackle the work now, and drive toward the success criteria, then go ahead and restart work.  Drive to the success criteria.
