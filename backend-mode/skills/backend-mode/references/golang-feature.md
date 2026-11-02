# Go Backend Feature Reference

Build order for a new capability or behavior change in a Go backend service (HTTP API and/or event consumer). Each step assumes the ones above it are settled — doing them out of order means redoing work. This is a default layering, not a mandated folder structure: the anti-patterns called out per step matter more than the exact names.

1. **Contract first** — apply **principle-backend-api-contract**: write or update the OpenAPI spec for a new/changed HTTP operation, or the AsyncAPI spec (or project equivalent) for a new/changed event, before writing a line of handler/consumer code. Generate server types/interfaces from it.
2. **Compatibility check** — apply **principle-backend-compat** whenever this touches an *existing* endpoint, event schema, or table: additive by default, expand/contract for migrations, a stated deprecation path for anything narrower.
3. **Layering check** — apply **principle-backend-layering**: detect whether this project already separates handler/service/repository. If it does, steps 4-6 below follow that split and anything violating it (business logic in a handler, a repository making a business decision) is a finding. If it doesn't yet (a new or small service), say so and skip straight to step 4 without scaffolding layers it hasn't grown into needing.
4. **Handler/transport layer** — bind the request (or event payload) to the generated type, call one or more service methods, map the result to a response/status.
5. **Service layer** — business rules, validation, authorization decisions, and orchestration across repositories/external clients live here, behind an interface the handler depends on. No transport types (no HTTP request/context type) leaking into this layer — keeps it testable without standing up a server.
6. **Repository layer** — data access only: queries, transactions, mapping rows to domain structs. One file per method is a reasonable default if the project already does that; match whatever convention is already there.
7. **External integrations** — a third-party call goes through its own wrapper package behind an interface, never called directly from the service or handler. This is what makes step 10 (TDD) possible without hitting the real vendor in a unit test.
8. **Concurrency & performance** — apply **principle-backend-performance** for anything spawning a goroutine, a worker pool, or sitting on a hot path: bound concurrency, tie goroutine lifetime to the request's `context.Context`, benchmark a claimed improvement.
9. **Migrations** — forward and backward (up/down) SQL, idempotent (`IF NOT EXISTS`/`IF EXISTS`), additive before destructive — see **principle-backend-compat**'s expand/contract pattern.
10. **Security** — apply **principle-backend-security** at the boundary this step introduced: input validation beyond the generated type's shape check, the specific authz decision, parameterized queries, secrets through the project's mechanism, a dependency check (`govulncheck`) if a new module was added.
11. **Tests** — TDD per **backend-testing**: the failing test first, dependencies behind interfaces so a unit test can mock them, a separate integration test for the repository/external call.
12. **Report** — what was built, which spec(s) changed, how compatibility/layering/security/performance were addressed or found not to apply, and the test scope/result.

"Go backend project" means `go.mod` at the root with a server/service entrypoint (`cmd/`, a `main.go` starting an HTTP server or event listener) — same marker `worktree-cleanup`'s cache reference uses for Go.
