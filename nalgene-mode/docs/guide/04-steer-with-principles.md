# 4. Steer with principle names

Six one-rule skills you invoke by name mid-task, in plain language, to redirect the agent without restating the whole instruction:

| Say this | To get |
|---|---|
| "apply fix-root-causes" | trace the symptom back to the first wrong decision, don't patch where it surfaced |
| "apply prove-it-works" | run it and show the real output, not "should work now" |
| "apply minimize-reader-load" | the smallest diff a reviewer can hold in their head, no padding |
| "apply outcome-oriented-execution" | name the target shape of a multi-step change and converge on it, no permanent halfway state |
| "apply report-dont-recommend" | describe tradeoffs on options or findings; stop before picking a winner, even a hedged one |
| "apply measure-query-cost" | an EXPLAIN ANALYZE (or ORM query count) before and after a SQL/ORM change, not "fewer joins so it's faster" |
```text
apply prove-it-works before you report this as fixed
```

`nalgene-mode`'s own `SKILL.md` has a **Principles** section grouping the five above under **Core** and **Verification**, with no `Framework` entries currently — it reads the matching leaf in full and applies it per playbook step without you asking. `tech-advisory` already cites `report-dont-recommend` in its own Present step, since presenting researched options without resolving the decision is exactly the situation this principle exists for. Naming one explicitly is for steering mid-task when you see the agent drifting — patching a symptom, claiming success without running anything, padding a diff, stalling a migration halfway, sliding a hedge into a recommendation, reaching for a new model before checking what already exists — not a prerequisite to invoking the others.

A stack-specific style — Odoo's reuse-and-extend gate, web/SPA's shared-client gate, or a personal/team workflow — doesn't go in `nalgene-mode`'s own `Framework` section at all. It ships as a separate sibling plugin with its own router that routes through `nalgene-mode`'s playbooks and adds its own gate on top, the way `odoo-mode` and `web-mode` do. See [the odoo-mode plugin](../../../odoo-mode/README.md) and [the web-mode plugin](../../../web-mode/README.md) for worked examples, and the note at the end of [page 2](./02-nalgene-mode.md#sibling-plugins) for when to reach for a sibling plugin instead of a `Framework` entry here.

## Adding a new principle

A principle is one file, `skills/principle-<kebab-title>/SKILL.md`: `name`, a `description` that starts with "Apply when ...", `disable-model-invocation: true`, then a short body with a **Why** (the cost of skipping it) and a **Pattern** (the concrete steps). Keep it to one page — the five generic principles are the template to copy.

Where it gets wired in depends on its scope:

- **General** (applies on every project, any stack) → add it under **Core** or **Verification** in `nalgene-mode`'s own Principles section, and reference it from the relevant `playbooks/*.md`, the way `fix-root-causes` is referenced from `bug-fix.md`.
- **A universal framework fact nalgene-mode itself should know about** → it would get a one-line entry under a **Framework** heading in `nalgene-mode`'s Principles section (so the trigger is discoverable), with the real wiring into that framework's own reference doc and the playbook step that branches on the framework. Nothing currently occupies this category — both frameworks discovered so far (Odoo, web/SPA) turned out to fit the sibling-plugin shape below instead. Reach for this only if a framework fact is genuinely too thin to warrant its own plugin, and say why.
- **A stack- or style-specific layer** (like `odoo-mode`'s `principle-odoo-reuse-extend` or `web-mode`'s `principle-spa-shared-client`) → it does *not* go in `nalgene-mode`'s own files at all. Ship it as a sibling plugin with its own thin router (its own `<name>-mode` skill) that routes through `nalgene-mode`'s playbooks and layers its own gate on top, the way `odoo-mode` and `web-mode` do. This keeps `nalgene-mode`'s own `Framework` section from growing one entry per stack as more get added (Android, iOS, backend, ...).

For a general or universal-framework principle, finish the same checklist:

1. Reference it from the playbook step(s) or framework reference where it applies.
2. Add a row to `nalgene-help`'s lookup table.
3. Add a row to the table above, noting if it's framework-specific.
4. If it's framework-specific, update [page 2's Sibling plugins section](./02-nalgene-mode.md#sibling-plugins) (or wherever that playbook's framework notes live).
5. Add a pitfall to [page 6](./06-recipes-and-pitfalls.md) for the failure mode the principle exists to prevent.

For a sibling-plugin principle, the checklist is the same shape but runs against the new plugin's own files — `odoo-mode` and `web-mode`'s own `README.md`s are worked examples, and `git log` for either extraction's commit shows the actual sequence of edits.

Next: [Gate non-trivial changes](./05-openspec-gate.md).
