# 5. Gate non-trivial changes

`openspec-gate` drives openspec's propose → validate → implement → archive loop. `nalgene-mode` calls it automatically for anything that adds or changes user-facing behavior or an API, or crosses more than one module. Call it directly for the same situations outside a `nalgene-mode` run:

```text
/openspec-gate propose the schema change for splitting billing_address into billing and shipping
```

## The loop

1. `openspec list` — reuse an existing proposal instead of duplicating it.
2. `openspec new change <change-name>` — kebab-case, describes the change, not a ticket number.
3. Write the proposal with `openspec instructions --change <change-name>`'s template — spec deltas and task breakdown, not a freehand structure.
4. `openspec validate <change-name>` — fix every error before implementing against it.
5. Implement against the approved tasks. Proposal turns out wrong mid-implementation → stop and fix the proposal first, don't let code and spec drift.
6. `openspec archive <change-name>` on ship — an unarchived change is unfinished work, not just unfinished code.

## When to skip it

Typos, one-line guards, dependency bumps, local renames with no behavior change. Say why you're skipping instead of silently not running it — that's the difference between a deliberate skip and a missed gate.

Read-only equivalent for "what does openspec already say about this" is **openspec-explore** — `how` runs it as step 0, or call it standalone.

Next: [Recipes and pitfalls](./06-recipes-and-pitfalls.md).
