---
name: check-sql-performance
description: "Verify a SQL or ORM change doesn't regress query cost before calling it done. Use when a diff touches a raw SQL statement, a cursor/db execute, an ORM query that can run per-row, or a reporting/aggregate view - for 'check SQL performance', 'N+1 query', 'verify ORM performance', or 'EXPLAIN ANALYZE'."
disable-model-invocation: true
---

# Check SQL performance

`nalgene-mode` calls this as a gate wherever a diff touches a SQL transaction; it also runs standalone.

## Does this apply?

Gate it if the diff adds, removes, or restructures a raw SQL statement, a cursor/db `execute`, an ORM query with N+1 risk (one query per row of a loop), or a reporting/aggregate view. Skip it for a query that's purely additive against a cold path, or one already measured by a baseline elsewhere in the same change — say so instead of running this skill.

## Steps

1. Detect scope. Confirm the diff actually touches a SQL transaction as described above. If it doesn't, this skill doesn't apply — say so and stop.
2. Baseline. Apply **principle-measure-query-cost**: before changing the query, capture its current cost (`EXPLAIN ANALYZE`, or the ORM's query count/log) on a representative input.
3. Make the change.
4. Re-measure. Same method, same input, after the change.
5. Compare. Cost should drop, or at least not regress. Confirm the output values are identical to before — an optimization that also changes behavior isn't done, it's a new bug.
6. Report the before → after numbers and the method used. A plausible number with no run behind it doesn't ship.

## Why

"Fewer joins" or "one subquery instead of three" sounds obviously faster, but the planner doesn't always agree — an `EXPLAIN ANALYZE` run is the only way to know, and the same run catches a rewrite that quietly changed an output value along with the plan.
