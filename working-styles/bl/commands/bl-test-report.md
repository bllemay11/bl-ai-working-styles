Run every test suite in the project and produce a comprehensive test report.

## Finding Test Suites

Look for test suites in these locations (in order of priority):
1. A `quintessential-test-report.md` or similar test report file in the project
2. `package.json` scripts containing "test" in the name
3. Test configuration files (jest.config, vitest.config, pytest.ini, etc.)
4. Common test directories (test/, tests/, __tests__/, spec/)

If a test report file exists, run every suite listed in it AND check if any new
suites exist that aren't in that file.

## Execution Rules

REQUIRED: Capture wall-clock duration for every suite. Pipe each test run
through `tee` to a temp file (e.g. `2>&1 | tee /tmp/test-fast.txt`) so the
duration line is never lost to output truncation. Include the Time column in
the report table -- every row must have a time value (e.g. "58.1s", "1.2s").

DO NOT SKIP ANY SUITE. "Every" means ALL of them. If a suite requires Docker,
check if Docker is running (docker compose ps) and run it. If Docker is
genuinely not running, report that as a blocker -- do not silently skip the
suite and mark it "not run."

If a suite fails to execute, report the failure. Never omit a suite from the
report.

## Output Format

Print a table to screen with columns:

| Suite | Status | Passed | Failed | Skipped | Time |

After the table:
- Total pass/fail/skip counts
- List any suites not in the existing report file (if one exists)

## On Failure

If tests are broken, do a deep dive investigation of why they are broken and
report that at the end.
