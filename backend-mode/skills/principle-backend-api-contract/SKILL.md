---
name: principle-backend-api-contract
description: "Apply when adding or changing a backend endpoint, or an asynchronous event/message. Write or update the OpenAPI (sync) or AsyncAPI (async) contract first and generate server types/stubs from it, instead of writing the handler or publisher/consumer freehand."
disable-model-invocation: true
---

# Backend: contract first (OpenAPI / AsyncAPI)

A backend service's real interface is the spec, not whatever the handler happens to return today. OpenAPI documents request/response HTTP endpoints; AsyncAPI documents messages and events over a broker, queue, or pub/sub topic. Either way, the spec comes first.

**Why:** a handler or event publisher written freehand before the contract settles tends to quietly disagree with what the client or consumer actually expects — a field renamed, an enum value added, a status code assumed — and that gap isn't caught until an integration test or production traffic hits it. A type generated from the spec can't drift from the spec; a type hand-written to mirror it is a second copy that can. For an event, an undocumented schema means every consumer has to read the producer's source to find out what the message looks like, instead of reading one spec.

**Pattern:**
- Before writing a handler, write or update the OpenAPI spec for it. Before writing an event publisher or consumer, write or update the AsyncAPI spec (or the project's established equivalent) for that message.
- Generate server interfaces, request/response types, and (where the project does so) client stubs from the spec. Confirm the generated server interface is fully implemented before calling the endpoint or event handler done — an unimplemented operation is a contract the service claims to support but doesn't.
- Don't hand-write a type that mirrors a generated one "just for this handler" — that's the second copy this principle exists to prevent. If codegen genuinely isn't wired up yet for this project, say so explicitly and keep the spec and the hand-written type in the same commit, in sync.
- Add realistic `example`s on request/response and event schemas, especially IDs, enums, and nested objects — they double as the first form of documentation and as fixtures for contract tests.
- Name the spec file(s) touched and whether codegen ran, even when the answer is "no codegen configured for this project yet." That's a finding, not silence.
