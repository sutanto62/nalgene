### Feature

1. Read `openspec-gate`'s `SKILL.md` and follow it first — propose before you build.
2. Name the data shape and where it lives before writing logic.
3. Implement against the approved proposal's spec deltas and tasks, not a freehand interpretation of the conversation.
4. Verify per **principle-prove-it-works**: exercise the real path a user would hit (run the app, hit the endpoint, render the screen), not just the tests. If this touches a SQL transaction (raw SQL, an ORM query with N+1 risk, or a reporting/aggregate view), also run **check-sql-performance** for the before/after query-cost numbers.
5. Archive the openspec change once merged — see **openspec-gate** step 6.
6. Report: what was built, how it maps to the proposal, how it was verified.
