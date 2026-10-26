---
name: openspec-gate
description: "Drive the openspec propose -> validate -> implement -> archive loop for non-trivial changes. Use before starting a feature, API change, or cross-cutting refactor, or whenever asked to 'openspec' something."
disable-model-invocation: true
---

# openspec gate

Non-trivial work gets a change proposal before code. `nalgene-mode` calls this as a gate; it also runs standalone.

## Is this non-trivial?

Gate it if the change adds or changes user-facing behavior or an API, crosses more than one module or package, or something other than you will rely on the result. Skip the gate for typos, one-line guards, dependency bumps, or local renames with no behavior change — say so instead of running this skill.

## Steps

1. Check for an existing proposal: `openspec list`. If one already covers this work, use it — don't create a duplicate.
2. If none exists, create one: `openspec new change <change-name>`. Use a short kebab-case name describing the change, not a ticket number.
3. Write the proposal: what changes, the spec deltas, the task breakdown. Run `openspec instructions --change <change-name>` for this project's enriched template and fill that in — don't invent a different structure.
4. Validate before implementing: `openspec validate <change-name>`. Fix every reported error; don't start code against an invalid proposal.
5. Implement against the approved tasks only. If implementation reveals the proposal was wrong, stop and update the proposal first — don't let code and spec drift apart.
6. When the change ships, archive it: `openspec archive <change-name>`. This folds the delta into the main specs. An unarchived change is unfinished work, not just unfinished code.

## Why

A proposal the agent didn't read isn't a gate, it's paperwork. Steps 3-4 make the spec the actual source of truth for "done," not a transcript of the conversation. Step 6 exists because specs that never get archived rot into a second, stale copy of the real behavior.
