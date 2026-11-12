### Coverage audit (what's missing, not what passes)

Read-only gap analysis: for a given module, find every testable unit, check whether a happy-path and a sad-path test exist for it, and report the gaps — it never runs a test or writes one. This is a planning step that feeds the other three playbooks, not a substitute for any of them.

1. Enumerate the module's testable units straight from its source, by kind:
   - **Models** (→ `playbooks/backend-testing.md`): every `@api.constrains`, `@api.depends`, `@api.onchange` method in `models/*.py`, plus any state-changing `action_*` method and any overridden `create`/`write`/`unlink`.
   - **Access** (→ `playbooks/backend-testing.md`): every row in `security/ir.model.access.csv` and every `ir.rule` record in `security/*.xml`, one unit per (model, group) pair.
   - **Controllers** (→ `playbooks/backend-testing.md`, via `HttpCase`): every `@http.route` in `controllers/*.py`.
   - **UI flows** (→ `playbooks/integration-testing.md`): interactive elements in `views/*.xml` that gate a user journey — buttons with `type="object"`, statusbar/state widgets, wizards.
   - **Isolated JS units** (→ `playbooks/js-testing.md`): exported components/services under `static/src/**/*.js` that are small enough to test without a full tour.
2. Define happy/sad per kind, don't improvise a different split per module:
   - Model method: happy = valid input produces the expected state/value; sad = invalid input raises the expected exception (`ValidationError`, `UserError`) rather than silently succeeding or crashing with something unrelated.
   - Access rule: happy = a user in the permitted group can perform the operation; sad = a user outside it gets `AccessError`, not a silent empty recordset or a 500.
   - Controller: happy = a valid request returns the expected response; sad = a malformed or unauthorized request returns the expected error status, not an unhandled exception.
   - UI flow: happy = the journey completes to its expected end state; sad = a validation or permission error surfaces correctly in the UI (blocked save, hidden/disabled button, visible error) rather than the flow silently continuing or crashing.
   - Isolated JS unit: happy = correct behavior given valid props/state; sad = invalid/edge input is handled without an unhandled error.
3. Find what already exists, per kind, and classify it:
   - Backend: grep `tests/*.py` for each unit's name. A test method that wraps the call in `assertRaises(ValidationError|UserError|AccessError, ...)` counts as sad-path coverage for whatever's inside; a plain `assertEqual`/`assertTrue` on the result counts as happy-path.
   - Integration: grep `static/tests/tours/*.js` tour `content` strings (now Given/When/Then per the Gherkin convention in `playbooks/integration-testing.md`) for the unit's flow. Words like "error," "blocked," "denied," "required," "invalid" in a step's `content` are the signal for sad-path coverage; a step with no such language toward an expected end state is happy-path.
   - JS unit: grep `static/tests/*.test.js` (Hoot) for the unit's name; `expect(...).toThrow()` or an asserted error/fallback state counts as sad-path, a plain behavior assertion counts as happy-path.
4. Treat a name match as a lead, not proof. Read the matched test before marking anything "done" — a unit's name appearing in a test file doesn't mean that test actually exercises it meaningfully (it may be an unrelated fixture, a different model with a similar name, or dead code). Mark a unit "unclear" rather than "done" when a quick read doesn't settle it, and say why.
5. Build one gap table, one row per unit: `Unit | Kind | Happy? | Sad? | Playbook to close it`. Values are `done`, `MISSING`, or `unclear (+ reason)`. "Playbook to close it" is always one of `backend-testing`, `js-testing`, `integration-testing` — never this playbook, which doesn't author tests.
6. Flag priority, don't just list. A `MISSING` sad-path on a state-changing action, an access rule, or anything touching money/compliance fields is higher-risk than a `MISSING` happy-path on a cosmetic UI flow — call out the high-priority rows separately so the report is actionable, not just exhaustive.
7. Report: the module audited, the full gap table, a one-line summary count (units audited, happy covered, sad covered, gaps, high-priority gaps), and nothing else — no test code, no stub files. Closing a gap is a separate run of whichever playbook column 5 named.

## Why

A happy-path-only module looks fully tested right up until the first invalid input, denied user, or malformed request reaches production — the three run-and-verify playbooks above only prove that the tests which exist still pass, they say nothing about the tests that were never written. This playbook exists to surface that blind spot on demand, as its own check, so "we have tests for this module" and "we have tests for what happens when this module is used wrong" stay two different, separately answerable questions.
