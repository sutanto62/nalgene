### Manual QA plan

A click-through (or equivalent) document a non-developer can run with no code, to confirm a feature works by hand. Stack-agnostic skeleton — check for a stack-specific supplement first (see "Sibling plugin supplements" below); write from this file directly only when none applies.

1. **Gather source material, richest first.** A coverage audit for the same feature (`playbooks/coverage-audit.md`'s own output, if one exists) beats the spec/capability text, which beats reading the code's user-facing surface cold. Each case below should trace back to something already found, not be invented fresh.
2. **Name the audience and the medium, once, up top.** Who runs this (QA, a stakeholder, support) and what they actually touch — a browser UI, a CLI, an API client, a physical device. Every case speaks in that medium's real vocabulary (button labels, command names, request/response shapes) from here on, never internal field or variable names.
3. **Write one-time setup separately from the cases.** Accounts, roles/permissions, feature flags, environment config, seed data — numbered, done once, not repeated per case.
4. **Write each case the same shape**: a short scenario name, the exact observable steps in order, the expected result, and a Pass/Fail line. A step that depends on something earlier states that dependency instead of silently assuming the reader remembers.
5. **Name what the surface deliberately hides, and how to check it anyway.** Some state a feature manages on purpose isn't visible on the medium from step 2 (a hash instead of a plaintext value, an internal status with no UI indicator). Say so explicitly per case, and give the indirect check that does exist (a log line, a side effect, a different screen) — don't let the plan imply something is checkable when it isn't.
6. **Cross-reference the source.** One line pointing back to the coverage audit or spec each case came from, so the two documents don't drift silently out of sync later.
7. **Report:** the medium, the case count, and where the file was saved (next to the coverage audit it was derived from, when one exists).

## Sibling plugin supplements

A sibling plugin with its own testing skill may carry a stack-specific version of this playbook with richer, native conventions this generic skeleton can't know (menu paths, a CLI's exact flags, device-specific setup). Check there first:

- Odoo project → `odoo-testing/playbooks/manual-qa-plan.md`.
- No match for the current stack → this file is the whole plan; don't wait for a supplement that doesn't exist.

## Why

A coverage audit and a set of passing automated tests both answer "does the code do what it's supposed to," read by someone who already trusts the code's own internal vocabulary. Neither answers "can a person who has never read this code verify it works" — that's a different reader, with a different, much lower bar for what counts as evidence. Writing that reader's document separately, derived from whatever already proved the feature works rather than re-deriving it, keeps the two honest: a claim here that the audit or spec doesn't back is a sign the plan drifted, not a sign the audit was incomplete.
