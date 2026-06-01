# Add Current Session to Bashrc

Add the current Claude session as a bash alias for easy restart.

**Arguments:** $ARGUMENTS (the alias name, e.g., "atf-mini-bl")

## Instructions

### Step 1: Find the session ID (ALL verification steps are MANDATORY)

The session ID is a UUID in the JSONL filename under `~/.claude/projects/`. The project directory name is the cwd with slashes replaced by dashes (e.g., `/Users/nate.fitch/projects/allthefish` becomes `-Users-nate-fitch-projects-allthefish`).

**You MUST perform ALL THREE verification steps. Do NOT skip any.**

**1a. Find candidate file by modification time:**
```bash
ls -lat ~/.claude/projects/{project-dir}/*.jsonl | head -3
```

**1b. MANDATORY: Verify the sessionId in the JSON content matches the filename:**
```bash
tail -2 ~/.claude/projects/{project-dir}/{uuid}.jsonl | cut -c1-300
```
Look for `"sessionId":"{uuid}"` in the output. The UUID in the JSON MUST match the filename.

**1c. MANDATORY: Verify this is the current conversation by checking recent content:**
```bash
grep -o '"text":"[^"]*"' ~/.claude/projects/{project-dir}/{uuid}.jsonl | tail -3
```
The output should contain text from the recent conversation (e.g., references to this command, recent topics discussed). If it doesn't match, you have the WRONG session.

**Do NOT proceed until all three checks pass.**

### Step 2: Read ~/.bashrc

Read the file to see the existing alias pattern:
```
alias claude-{name}='cd /path/to/project && claude -r {session-id}'
```

### Step 3: Add the alias

Add a new alias following that exact pattern:
```
alias claude-$ARGUMENTS='cd {current-working-directory} && claude -r {session-id}'
```

### Step 4: Report

Show the user:
- The session ID found
- Evidence from verification (what content you saw that confirmed it)
- The exact alias line added
- Remind them to run `source ~/.bashrc`
