# Persona 7 Loop Instructions - READ EVERY CYCLE

## CRITICAL RULES
- **NEVER STOP TO GIVE STATUS** - Keep working until user stops you
- **NO PERMISSION NEEDED** - You can run tests, refactor, commit, scan autonomously
- **NO HUMAN APPROVAL** - Don't ask, just do standard refactoring operations

## THE LOOP
1. **Find** - Scan for duplication (grep, read, analyze)
2. **Refactor** - Extract to shared utility/hook/constant
3. **Test** - Run `npm test` - ALL tests must pass
4. **Commit** - Clear message describing duplication eliminated
5. **Log** - Append one line to refactor log
6. **Re-read** - Read this file again
7. **Repeat** - Go back to step 1 immediately

## LOG FORMAT
```
<hash> - <description> (<impact>)
```

Examples:
- `abc1234 - Extract foo into shared utility (3 files, 45 lines → 32-line util)`
- `def5678 - Remove duplicate constants (2 files, 12 lines → using shared constant)`
- `ghi9012 - Remove unused imports (4 files, 6 unused imports removed)`

## REFACTORING TARGETS
- Duplicate functions/logic → extract to utility
- Duplicate constants → extract to shared constant file
- Duplicate hooks patterns → extract to custom hook
- Duplicate JSX patterns → extract to shared renderer
- Unused imports → remove
- Inline constants duplicated elsewhere → use shared version

## WHAT NOT TO DO
- Don't stop for status updates
- Don't ask permission for standard operations
- Don't create abstractions for single use cases
- Don't refactor tests (too risky)
- Don't spend more than 2 minutes deciding - just pick something and go
- **NEVER write summary reports** - Just keep looping. No "Summary: X cycles completed" messages

## WHEN STUCK
- Use `find`, `grep`, `wc -l` to scan
- Look for files >300 lines
- Search for duplicate patterns: `grep -r "pattern" --include="*.tsx"`
- Check for unused imports
- If truly nothing found after 3 scans, then report completion

## TEST REQUIREMENTS
- Run tests after every change
- ALL tests must pass before commit
- If tests fail, fix them or revert the change
- Test stability is the top priority
