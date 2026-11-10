---
name: lahiru-dev-style
description: 'Guide for reviewing ABAP code when asked to review programs, classes, function modules, or includes.'
---
Use this skill when the user asks to review ABAP code (reports, classes, methods, function modules, includes, or enhancements).

Review goals:
- Find bugs, runtime risks, behavioral regressions, and missing tests first.
- Keep summaries brief and place them after findings.
- Prioritize issues by severity.

ABAP review workflow:
1. Identify reviewed objects and execution context (transaction, report, batch job, RFC, OData, update task).
2. Scan for correctness risks:
   - Wrong or missing `sy-subrc` checks after `SELECT`, `READ TABLE`, `CALL FUNCTION`, and authority checks.
   - Unsafe assumptions about empty internal tables or initial values.
   - Possible short dumps (`CX_SY_*`, conversion errors, null dereference patterns, index access without checks).
3. Check data access and performance:
   - `SELECT *` where only few fields are needed.
   - `SELECT` inside loops, nested database access, missing buffering awareness.
   - Missing or weak WHERE conditions leading to full table scans.
   - `FOR ALL ENTRIES` usage without guarding empty driving table.
4. Validate transactional behavior:
   - Incorrect `COMMIT WORK` / `ROLLBACK WORK` placement.
   - Update task (`IN UPDATE TASK`) and enqueue/dequeue consistency.
5. Validate security and compliance:
   - Missing `AUTHORITY-CHECK` before sensitive operations.
   - Dynamic SQL or dynamic program execution without sanitization.
   - Exposure of sensitive data in logs/messages.
6. Check maintainability and style:
   - Complex monolithic methods that need extraction.
   - Inconsistent naming and weak typing where strong typing is possible.
   - Missing comments only where logic is non-obvious.
7. Check testability:
   - Missing ABAP Unit coverage for business-critical branches.
   - No seams/mocks for DB or external calls.

Output format for the review response:
- Findings first, ordered by severity (`High`, `Medium`, `Low`).
- Each finding should include:
  - Location (`object`, include/method, and line if available).
  - Why it is a problem.
  - Suggested fix.
- Then list open questions/assumptions.
- End with a short change summary (or explicitly state no findings).

If no defects are found, explicitly say no findings were discovered and mention residual risks (for example, missing runtime test evidence or uncovered integration paths).
