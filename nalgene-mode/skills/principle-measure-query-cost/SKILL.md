---
name: principle-measure-query-cost
description: "Apply before claiming a SQL or ORM query change is faster or cheaper. Run EXPLAIN ANALYZE (or the ORM's query count) before and after on the same input - reasoning like 'fewer joins' or 'one subquery instead of three' isn't proof on its own."
disable-model-invocation: true
---

# Measure query cost

Specializes **principle-prove-it-works**'s performance-claim bullet for SQL and ORM changes specifically: the planner doesn't always agree with join-count reasoning, so the reasoning isn't the proof.

**Why:** a query rewrite that looks obviously cheaper — fewer joins, one aggregate instead of three, a consolidated CTE — can still regress once the planner picks a different plan, and a rewrite can just as easily change an output value along with the plan without anyone noticing until a report runs wrong. A before/after run catches both at once; reasoning from structure alone catches neither.

**Pattern:**
- Before touching the query, capture its current cost on a representative input: `EXPLAIN ANALYZE` for raw SQL or a view, the ORM's query count/query log for code that issues queries in a loop.
- Make the change.
- Re-run the exact same method on the exact same input.
- Compare cost (time, rows scanned, plan shape) *and* confirm the output values are identical to before — an optimization that also changes behavior isn't done, it's a new bug.
- Report the before → after numbers and the method used. "Fewer joins so it's faster" is not a number; a number with no run behind it is not proof either.
