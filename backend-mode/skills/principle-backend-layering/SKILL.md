---
name: principle-backend-layering
description: "Apply before adding business logic to a handler/route, direct DB/ORM/HTTP access outside a repository or external wrapper, or any code that would duplicate a transport/business-logic/data-access split the project already uses elsewhere. Detect whether that split is already established; if it is, match it. If it isn't, say so and skip rather than scaffolding layers a small project hasn't earned yet."
disable-model-invocation: true
---

# Backend: match the layering that's already there

Not a mandate to impose Clean Architecture's full vocabulary (entities, use cases, interface adapters) on every backend service. A service with three endpoints and no established split doesn't need handler/service/repository/interface scaffolding any more than it needs a microservices split — that's overengineering a project that hasn't grown into needing it. What this gate actually checks is narrower and cheaper: has *this* project already drawn a line between transport, business logic, and data access somewhere, and if so, does the new code respect it.

**Why:** a fixed rule ("always separate into three layers") can't tell a three-endpoint prototype from a service with hundreds of handlers, so it either underkills the large one (nothing stops drift once the convention exists) or overkills the small one (layers added for a project that doesn't need them yet, paid for in indirection with no payoff). Detecting the existing convention first and enforcing only that is what scales the right amount automatically: a project with no layering yet costs nothing to check, and a project that has grown real layers gets real protection against a new handler quietly reaching past them — which is exactly the point in its life where that drift starts costing someone a confusing bug.

**Pattern:**
- Detect first: does the project already separate transport (handler/controller/route) from business logic (service/use case) from data access (repository/DAO) in more than one place — existing sibling files or packages doing this split, not a single example someone wrote once.
  - **Established split found** → this gate applies. New or changed code in this area follows the same split: the handler/route binds the request and calls into business logic, the service holds the business rule, the repository is the only thing touching the DB/ORM/query builder directly, and a third-party call goes through its own wrapper instead of being called inline.
  - **No established split** (a new project, or a small service where everything still lives in one or two files) → this gate doesn't fire. State "no layering convention established yet, skipping" explicitly rather than introducing a layer nobody asked for.
- Anti-patterns to flag once the gate applies: business logic (branching on a domain invariant, orchestrating multiple calls) inside a handler/route; a handler/route, or a shared/presentational unit, calling the DB/ORM/HTTP client directly instead of going through the service/repository; a repository making a business decision beyond what the query itself requires; a "god" service file accumulating unrelated use cases instead of being split the way the rest of the domain already is.
- A refactor consolidating scattered business logic into the existing service layer, or a new domain that's clearly going to grow past a couple of endpoints/events, is where introducing the split proactively is the right call even on a file that hasn't been touched yet — name that as a deliberate choice, not something that happened by accident.
- Name the layering decision explicitly, even when the answer is "nothing to enforce yet, staying flat" or "matches the existing split, no drift found." That's a finding, not silence.
