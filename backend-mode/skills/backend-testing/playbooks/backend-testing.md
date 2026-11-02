### Backend testing

General flow for TDD on a Go or Node.js backend service, scoped to the package/module under test.

1. **Red first.** Write the failing test before the implementation for new behavior, or a regression test that reproduces the bug before touching the fix, per **principle-fix-root-causes**. A test that passes before any code change is written isn't testing the change.
2. **Depend on interfaces, not concretes.** The function/struct under test takes its collaborators (repository, external client, clock, publisher) as interface parameters so a mock or fake can stand in. If the current code reaches a concrete singleton or does IO directly, that's a structural fix to make before the test, not after.
3. **Unit tests — no real DB, HTTP, filesystem, or env.**
   - Go: table-driven `t.Run` cases, one mock/fake per interface (hand-rolled or generated, e.g. `mockgen`/`gomock`), `t.Parallel()` on tests that own no shared mutable state, verified with `go test -race` whenever concurrency is involved.
   - Node.js: Jest/Vitest, collaborators swapped via dependency injection or the framework's module mocking, no real network — stub it (`nock`/`msw`) if an HTTP call sneaks into a "unit" test.
4. **Integration tests — real DB/queue/external sandbox, a separate suite.**
   - Go: suffix `_integration_test.go`, skip under `testing.Short()` (or an equivalent build tag) so the default run excludes it.
   - Node.js: a separate file/project convention (`*.integration.test.ts` or a distinct test config) excluded from the default fast run.
   - Required *alongside* the unit test for anything touching persistence or a third-party call — it isn't a replacement for the unit test, and the unit test isn't a replacement for it.
5. **Run scoped, not the whole repo.** Target the changed package/module (`go test ./internal/service/foo/...`, `jest path/to/feature`) rather than a full-suite run on every change — the same "smallest unit that proves it" idea **principle-minimize-reader-load** applies to diffs, applied here to test runs.
6. **Verify per principle-prove-it-works.** A failing or skipped test anywhere in the scoped run is blocking. Re-run the same scoped command after a fix; don't widen the run just to double-check, and don't declare done against a subset of the output.
7. **Report:** what ran (unit/integration, scope), the pass/fail count, and which collaborators were mocked vs exercised for real.
