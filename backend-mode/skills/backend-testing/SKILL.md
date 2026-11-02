---
name: backend-testing
description: "TDD flow for Go and Node.js backend services: depend on interfaces so a test can substitute a mock/fake, write the failing test first, and keep unit tests (mocked, no real IO) and integration tests (real DB/queue/external sandbox) separate and both required. Use for '/backend-testing', writing or running backend tests, or the verification step of any backend-mode playbook."
disable-model-invocation: true
---

# Backend testing

`backend-mode` calls this at the verification step of any matched playbook on a Go or Node.js backend project; it also runs standalone.

**Requires** the `nalgene-mode` plugin, same as `backend-mode` — this skill assumes `backend-mode`'s Detection has already confirmed a Go or Node.js backend project.

## Two rules

1. **Depend on interfaces, not concretes.** A function or struct under test takes its collaborators — repository, external client, clock, message publisher — as interface parameters (constructor injection), not a package-level singleton or a `new RealThing()` built inline. If the code can't be exercised without hitting a real DB, network call, or filesystem, that's the thing to fix before writing the test, not a reason to skip it or make the test an integration test by default.
2. **Keep unit and integration separate, and ship both.** A unit test (mocked collaborators, no real IO) and an integration test (the real DB/queue/sandbox) answer different questions and neither substitutes for the other — a repository or external-call change needs both, not whichever is more convenient.

Full procedure — TDD order, mocking conventions per stack, the unit/integration split, scoped test runs — lives in `playbooks/backend-testing.md`; copy its steps into the todo list the way any `nalgene-mode` playbook is copied in.

## Why

Code wired directly to a concrete repository, a real HTTP client, or `time.Now()`/`Date.now()` can't be unit-tested without standing up the real thing it depends on, so testing gets deferred, or the "unit" test quietly becomes a slow integration test that needs a database to even start. Depending on an interface is what makes the fast, isolated test possible in the first place — TDD only pays off once red → green → refactor is cheap enough to actually run on every change.
