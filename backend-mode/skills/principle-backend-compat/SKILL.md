---
name: principle-backend-compat
description: "Apply when changing an existing endpoint's request/response shape, an existing event/message schema, or a database migration. Default to additive changes; version or deprecate anything that would break an existing caller, consumer, or deployed row instead of changing it in place."
disable-model-invocation: true
---

# Backend: backward compatibility by default

A backend service almost never has exactly one caller on exactly one version. Mobile and third-party clients can't be forced to upgrade the instant a deploy lands; other services and event consumers keep running against whatever contract or schema was in place when they last deployed; a migration runs against rows written by the previous version of the code.

**Why:** a change that looks like a small, obviously-correct cleanup from inside the diff — renaming a field, making an optional field required, dropping a column, changing an enum's meaning — is a breaking change for anything still on the old shape, and "still on the old shape" usually includes something the person making the change can't see from the repo: an old mobile build, a partner integration, an event consumer deployed from a different repo on its own schedule. The cost of skipping this shows up as an outage or a silent data-loss bug well after the change merged, not at review time.

**Pattern:**
- Prefer additive changes: a new optional field, a new endpoint or API version, a new enum value. Don't remove, rename, or repurpose a field, endpoint, or event name a caller or consumer might depend on without a stated deprecation path and timeline.
- Request/response changes: widen, don't narrow. Making an optional field required, renaming a field, or changing a field's type or meaning in place are breaking changes even when the diff is one line. Ship the new shape alongside the old (a new field, a new version) and deprecate the old one on a stated timeline instead of changing it under existing callers.
- Event/message schema changes: a consumer can lag behind a producer indefinitely in an async system, and there's no request/response moment to catch the mismatch. New fields are additive and optional by default; a schema change that removes a field or makes a new one required needs a new event version or topic, not an in-place edit.
- Database migrations: expand/contract, never a single migration that adds and removes in the same step. Expand (add a nullable column or new table) → deploy code that writes the new shape (and reads it, once ready) → backfill existing rows → contract (drop the old column/table) only once nothing still reads it. Every migration ships a working rollback and is idempotent (`CREATE ... IF NOT EXISTS` / `DROP ... IF EXISTS`), since a partial or retried deploy can re-run it.
- Name the compatibility risk explicitly, even when the answer is "this is purely additive, nothing on the old shape breaks." That's a finding, not silence.
