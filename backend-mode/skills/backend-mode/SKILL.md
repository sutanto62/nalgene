---
name: backend-mode
description: "Backend agent style layered on nalgene-mode: contract-first (OpenAPI/AsyncAPI), backward-compatible, security- and performance-minded, TDD-friendly, routed through nalgene-mode's playbooks with a build order per stack (Go, Node.js). Use for /backend-mode, or nontrivial work on a Go or Node.js backend service."
disable-model-invocation: true
---

# Backend mode

Thin router over the `nalgene-mode` plugin's own `nalgene-mode` skill. Shared principles and playbook machinery live there — read them from it, don't restate them here.

**Requires** the `nalgene-mode` plugin. Check first: the `nalgene-mode` skill should be in your skill list. Missing → stop and tell the user to install it (`/plugin install nalgene-mode@nalgene` in Claude Code, `/add-plugin nalgene-mode` in Cursor).

Every reference to `nalgene-mode` below — its Principles section, a playbook, `openspec-gate`, `how`, `check-sql-performance` — means `Read` that file directly off disk and follow it, not a `Skill`-tool call into `nalgene-mode`. On Claude Code, read `nalgene-mode/docs/harness/claude-code.md` once per session before following any of these references; Cursor has no such restriction.

## Sibling skills (this plugin)

| Skill | Job |
|---|---|
| `backend-mode` | This router |
| `principle-backend-api-contract` | The Contract gate below |
| `principle-backend-compat` | The Compatibility gate below |
| `principle-backend-layering` | The Layering gate below |
| `principle-backend-security` | The Security gate below |
| `principle-backend-performance` | The Performance gate below |
| `backend-testing` | The Testing gate below |

## Shared machinery (do not restate)

Read these from the `nalgene-mode` plugin. Do not copy their contents into this file.

| Layer | Where |
|---|---|
| Shared mode (Principles index, non-negotiables, playbook catalog, todo-list convention) | `nalgene-mode`'s own `nalgene-mode` skill |
| Shared playbooks | `nalgene-mode/playbooks/<name>.md` |
| General principle leaves (`fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`, `report-dont-recommend`) | `nalgene-mode`'s `principle-*` skills |
| SQL/ORM query-cost gate | `nalgene-mode`'s `check-sql-performance` skill and `principle-measure-query-cost` |
| openspec gate | `nalgene-mode`'s `openspec-gate` skill |
| "How does X work" before changing something | `nalgene-mode`'s `how` skill |

## Non-negotiables

1. Open a todo list. Item 1: read `nalgene-mode`'s **Principles** section in full. Cite each principle you apply with the concrete choice it changed.
2. Match a `nalgene-mode` playbook (`feature`, `bug-fix`, `refactor`, `worktree-cleanup`, `create-pr`). Copy its steps into the todo list before the gates below. A skipped step stays with a one-line reason instead of being dropped.
3. Apply `nalgene-mode`'s own non-negotiables (openspec-gate before code on anything touching behavior or a public interface, **how** before a nontrivial change, **check-sql-performance** wherever a diff touches a SQL transaction). Do not re-list them here.
4. Apply the **Backend gates** below wherever the matched playbook's own step would otherwise have you write a handler/consumer, change a contract or schema, touch a request/event boundary, add concurrency, or verify with tests — each gate only fires when the project is in scope; see Detection.
5. For `feature` or `bug-fix`, the Contract gate opens the per-stack build order: follow `references/golang-feature.md` (Go) or `references/nodejs-feature.md` (Node.js) instead of improvising one, the same way `odoo-mode` and `web-mode` open their own build orders from their first gate.

### Contract gate

- **Spec first.** Apply **principle-backend-api-contract**: write or update the OpenAPI (HTTP) or AsyncAPI (event/message) spec and generate types from it before writing the handler, publisher, or consumer.

### Compatibility gate

- **Backward compatible by default.** Apply **principle-backend-compat** whenever the change touches an *existing* endpoint, event schema, or database migration: additive first, expand/contract for migrations, a stated deprecation path for anything that would otherwise break a deployed caller or consumer.

### Layering gate

- **Match the existing split, don't impose one.** Apply **principle-backend-layering**: detect whether this project already separates transport, business logic, and data access in more than one place. Found → new/changed code follows that same split, and business logic in a handler or a repository making a business decision is a finding. Not found (a new or small project) → say so and skip; don't scaffold handler/service/repository layers a project hasn't grown into needing yet.

### Security gate

- **Validate and authorize at the boundary.** Apply **principle-backend-security** wherever the change touches a request/event boundary, an authn/authz decision, a secret, or a new third-party dependency.

### Performance gate

- **Bounded concurrency, measured claims.** Apply **principle-backend-performance** wherever the change adds a hot request path, a goroutine/worker/background job, or anything that could block a request thread or the event loop. A change that's purely a SQL/ORM query goes through the core `check-sql-performance` gate instead — these are complementary, not duplicates.

### Testing gate

- **TDD, interfaces over concretes.** Apply **backend-testing** at the matched playbook's verification step: the failing test first, collaborators behind interfaces so a unit test can mock them, unit and integration tests kept separate and both shipped where persistence or a third-party call is involved.
- Full flow: `backend-testing`'s `playbooks/backend-testing.md`. Copy its steps into the todo list wherever the matched `nalgene-mode` playbook's verification step lands.

### Detection

"Backend project" means a service that exposes HTTP/gRPC endpoints and/or consumes events, as opposed to a frontend SPA (`web-mode`'s territory) or an Odoo module (`odoo-mode`'s territory). Two stacks are covered so far:

- **Go** — `go.mod` at the root with a server/service entrypoint (`cmd/`, a `main.go` starting an HTTP server or event listener). Opens `references/golang-feature.md`.
- **Node.js** — `package.json` naming a server framework (`express`, `fastify`, `@nestjs/core`, `koa`, `hapi`) or server-side routes/controllers, with no SPA framework (`react`, `vue`, `@angular/core`, `svelte`) in the same `package.json`. Opens `references/nodejs-feature.md`. `package.json` alone isn't enough — a Node.js SPA frontend matches `web-mode`'s detection instead, and a monorepo can hold both; detect per sub-project touched.

Neither marker present → this plugin doesn't apply; run `nalgene-mode` (or `web-mode`/`odoo-mode`, if that detection fits instead) directly rather than `/backend-mode`. A stack beyond Go and Node.js → say so and fall back to `nalgene-mode` directly; a new `references/<stack>-feature.md` is how a third stack gets added here.

## Report

Same as the matched `nalgene-mode` playbook's own report line, plus:
- Which spec(s) (OpenAPI/AsyncAPI) changed and whether codegen ran.
- The compatibility, layering, security, and performance findings for this change, even when each answer is "not applicable here."
- The test scope (unit/integration) and pass/fail count from the Testing gate.
