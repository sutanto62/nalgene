### Refactor

No behavior change. If behavior must change, this is a Feature, not a Refactor.

1. Read `openspec-gate`'s `SKILL.md` and follow it if the restructuring crosses a module boundary or something outside your control depends on the touched interface. Skip it for a local, single-file cleanup — say so.
2. Default to deletion. Remove dead code and collapsed abstractions before adding anything new, per **principle-minimize-reader-load**.
3. Keep the diff behavior-preserving at every commit — a reviewer should be able to bisect it.
4. Verify per **principle-prove-it-works**: the same behavior holds before and after. If nothing covers the touched path, add the minimum needed to prove equivalence. If the restructuring touches a SQL transaction (raw SQL, an ORM query with N+1 risk, or a reporting/aggregate view — e.g. consolidating redundant joins in a reporting view), also run **check-sql-performance** for the before/after query-cost numbers.
5. Report: what moved, what was deleted, how equivalence was verified.
