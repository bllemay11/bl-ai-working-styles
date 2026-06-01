The target is always 100% of tests passing.

DONT STOP FOR PROGRESS REPORTS: DRIVE TO THE SUCCESS CRITERIA
NO EMOJIS
DO NOT GIVE ESTIMATES
DO NOT GIT PUSH ONLY USERS DO THAT

EFFICIENT TEST EXECUTION: Integration tests take 3-5 minutes. ALWAYS capture output once, then analyze, eg:
  npm run test:integration 2>&1 | tee /tmp/test-output.txt
Then grep/tail the file. NEVER re-run tests just to see different parts of output.

Reread project-reminders.md and/or reminders.md if any of the three things above are unfamiliar to you.

DO NOT CONTINUE WITH WORK.  ACKNOWLEDGE THAT YOU HAVE READ THESE AND LIST THE THINGS YOU SHOULD NEVER DO.
