---
name: principle-odoo-reuse-extend
description: "Apply when adding or creating a new Odoo feature, before scaffolding models or views. Spawn an explorer to check whether Community or Enterprise already covers it, in full or in part, prefer extending over duplicating, and keep custom modules from depending on each other directly."
disable-model-invocation: true
---

# Odoo: reuse and extend before creating

Odoo ships a large surface in Community and Enterprise. A new feature is often a thin `_inherit` away from something that already exists, not a reason to start a module from scratch.

**Why:** a model, view, or wizard built fresh duplicates fields, security rules, and upgrade-path maintenance that CE or EE already carries, and a later Odoo upgrade can silently fix or change behavior you just reimplemented from scratch. The same waste shows up one level higher, in the dependency graph: a custom module that `depends` on another custom module ties two unrelated features' install, upgrade, and uninstall lifecycles together, and that coupling tends to spread silently until the whole custom tree is one tangled unit nobody can touch in isolation.

**Pattern:**
- Before scaffolding, spawn a `nalgene-explorer` to search Community then Enterprise, in that order, for a model, mixin, view, wizard, or workflow that already does this or most of it. Resolve `{CE_PATH}`/`{EE_PATH}` the same way `how`'s Odoo track does, and reuse its explorer prompt (`skills/how/references/explorer-prompt-odoo.md`), scoped to "does X already exist" rather than a full architecture writeup.
- Prefer, in this order: use as-is, no code > `_inherit` to extend > compose a new model that depends on the existing one > write new. Only fall to "write new" once the search comes back empty on all three.
- An Enterprise hit is a real option, not a default to skip, but the module now requires an EE license. Name that cost in the report so the human can weigh it against building the behavior in Community.
- Name what you found and which option you chose, even when the answer is "nothing reusable." That's a finding, not silence.

**Dependency boundary:**
- A custom module can `depends` on as many canonical Community or Enterprise modules as it needs — that's the normal extend-don't-duplicate path above.
- A custom module must not `depends` on another custom module. Each custom module stays installable, upgradable, and removable on its own.
- The one exception is a project-level aggregator module whose entire purpose is composing multiple custom and/or canonical modules — shared menus, cross-module security groups, glue views. Inter-custom coupling lives only there, never as a direct edge between two feature modules.
- If two custom modules need the same logic, that's a signal it belongs one layer up, not between them: move it into a canonical module both can depend on, or into the aggregator that composes both. Don't add a `depends` edge from one custom module straight to another to get there.
- When scaffolding (`odoo-feature.md` step 2), check the new module's intended `depends` against this boundary before writing the manifest, not after the dependency is already in place.
