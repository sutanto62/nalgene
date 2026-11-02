# backend-mode

Backend agent style layered on [nalgene-mode](../nalgene-mode/). nalgene-mode does the shared work: router, openspec gate, core principles, playbook machinery. This plugin adds five gates a backend service needs — contract-first (OpenAPI/AsyncAPI), backward compatibility, layering that matches what the project already does (not a mandated architecture), security at the boundary, and bounded/measured concurrency — plus a build order per stack (Go, Node.js) and a TDD-shaped testing gate.

## Install

**Claude Code:**

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene-mode@nalgene
/plugin install backend-mode@nalgene
```

**Cursor:** `/add-plugin` pointing at `backend-mode/` in this repo, or via Customize in the sidebar. Uses `.cursor-plugin/plugin.json`.

This plugin requires `nalgene-mode` to be installed alongside it — its `backend-mode` skill routes through `nalgene-mode`'s own playbooks rather than restating them.

## Skills

| Skill | Use it when |
|---|---|
| [`/backend-mode`](./skills/backend-mode/SKILL.md) | Default entry for nontrivial work on a Go or Node.js backend service. Routes through `nalgene-mode`'s playbooks with the five gates below on top. |
| [`principle-backend-api-contract`](./skills/principle-backend-api-contract/SKILL.md) | Referenced by `backend-mode`. Spec first: OpenAPI for HTTP endpoints, AsyncAPI for events/messages, codegen before the handler. |
| [`principle-backend-compat`](./skills/principle-backend-compat/SKILL.md) | Referenced by `backend-mode`. Additive by default for endpoints, event schemas, and migrations (expand/contract); version or deprecate instead of breaking a deployed caller. |
| [`principle-backend-layering`](./skills/principle-backend-layering/SKILL.md) | Referenced by `backend-mode`. Detect whether the project already separates transport/business-logic/data-access; enforce it only where it already exists, skip scaffolding it onto a project too small to have earned it. |
| [`principle-backend-security`](./skills/principle-backend-security/SKILL.md) | Referenced by `backend-mode`. Validate at the boundary, enforce the specific authz decision, never hardcode a secret, parameterized queries, scan new dependencies. |
| [`principle-backend-performance`](./skills/principle-backend-performance/SKILL.md) | Referenced by `backend-mode`. Benchmark a concurrency/hot-path claim, bound goroutines/promises instead of spawning them unbounded, don't block the event loop. |
| [`backend-testing`](./skills/backend-testing/SKILL.md) | Referenced by `backend-mode`. TDD: collaborators behind interfaces, failing test first, unit and integration tests kept separate and both shipped. |

## What this plugin's backend-mode skill adds over nalgene-mode

- **Five extra gates.** Contract-first before a handler or event publisher/consumer is written; backward compatibility before an existing endpoint, event schema, or migration changes; layering matched to whatever transport/business-logic/data-access split the project already has (and skipped, not imposed, where none exists yet); security validation and the specific authz decision at every request/event boundary; bounded, benchmarked concurrency on any hot path or background job.
- **A build order per stack.** `skills/backend-mode/references/golang-feature.md` and `references/nodejs-feature.md` each sequence a feature in that stack's own shape (contract → compatibility check → layering check → transport layer → service layer → repository layer → external integrations → concurrency → migrations → security → tests), opened by the Contract gate. Bug fixes and refactors read the matching reference to locate the likely layer. A layered build order, not a mandated folder structure — the Layering gate decides whether layering applies at all before the anti-patterns named at each step (business logic in a handler, a route importing the DB client directly, an unbounded goroutine per request) are worth flagging.
- **A TDD-shaped testing gate.** `backend-testing` requires collaborators behind interfaces (so a test can mock them without real IO), a failing test before the implementation, and unit + integration tests shipped together rather than either one standing in for the other.

## Not shipped here

- Principles, playbooks, the openspec gate, `how`/`why`/`teach`/`tech-advisory`, the todo-list convention, and the SQL/ORM query-cost gate (`check-sql-performance`, `principle-measure-query-cost`) ship in `nalgene-mode` — this plugin's own performance principle covers concurrency and hot paths, not SQL cost, to avoid duplicating that gate.
- A third stack (beyond Go and Node.js) isn't covered yet. Add it the way this plugin itself was added: a new `references/<stack>-feature.md` plus a Detection entry in `backend-mode`'s own `SKILL.md`.

## License

MIT
