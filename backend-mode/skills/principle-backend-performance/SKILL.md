---
name: principle-backend-performance
description: "Apply when adding a hot request path, a goroutine/worker/background job, or anything that can block a request thread or the event loop. Benchmark before/after on a representative load, bound concurrency instead of spawning it unbounded, and don't block the request path on work that doesn't need to finish before the response."
disable-model-invocation: true
---

# Backend: bounded concurrency, measured claims

Specializes **principle-prove-it-works**'s performance-claim bullet for backend request handling and concurrency specifically, the way the core `principle-measure-query-cost` specializes it for SQL/ORM changes — read that one too when the hot path is a query; this one covers everything else on the request path.

**Why:** a backend's latency and resource usage under concurrent load is governed by what runs on which path and how many of it can run at once, not by a single request's trace looking fast in isolation. "This should be faster" or "this runs in the background now" is a plausible-sounding claim that a benchmark or a load test either confirms or quietly contradicts — concurrency bugs and resource exhaustion only show up under load, never in a single manual request.

**Pattern:**
- Benchmark before/after on a representative load for anything claimed faster, cheaper, or more concurrent: `go test -bench` and `pprof` in Go, `clinic`/`autocannon`/`0x` in Node.js. "Should be faster" is not a number; a number with no run behind it is not proof either — same standard **principle-measure-query-cost** holds SQL changes to.
- Don't block the request path on work that doesn't need to finish before the response — hand it off to a background job or queue instead. But bound that handoff's concurrency (worker pool, semaphore, bounded queue); an unbounded goroutine or promise spawned per request accumulates under load until the process falls over.
- Go: every goroutine spawned per request carries that request's `context.Context` and has a real exit path (cancellation, timeout, or a result it's waited on) — a goroutine with no path to exit is a leak, not a background task.
- Node.js: a synchronous CPU-heavy loop, or a sync `fs`/`crypto` call, on a hot path blocks the entire event loop for every other in-flight request, not just the one that triggered it — offload to a worker thread or a queue instead of optimizing it in place.
- Size connection pools deliberately (DB, HTTP clients to external services, message broker) and name the number chosen; an unbounded or default pool size is frequently the actual bottleneck, not the application code the diff touches.
- Name the before/after numbers and method, even when the answer is "not a hot path, skipping measurement" — that's a finding, not silence.
