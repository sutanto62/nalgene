# odoo-mode

Odoo agent style layered on [nalgene-mode](../nalgene-mode/). nalgene-mode does the shared work: router, openspec gate, core principles, playbook machinery. This plugin adds the one gate this stack needs — search Community then Enterprise before scaffolding anything new — and the build order for a feature in that shape.

## Install

**Claude Code:**

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene-mode@nalgene
/plugin install odoo-mode@nalgene
```

**Cursor:** `/add-plugin` pointing at `odoo-mode/` in this repo, or via Customize in the sidebar. Uses `.cursor-plugin/plugin.json`.

This plugin requires `nalgene-mode` to be installed alongside it — its `odoo-mode` skill routes through `nalgene-mode`'s own playbooks rather than restating them.

## Skills

| Skill | Use it when |
|---|---|
| [`/odoo-mode`](./skills/odoo-mode/SKILL.md) | Default entry for nontrivial work on an Odoo module. Routes through `nalgene-mode`'s playbooks with the Odoo gate on top. |
| [`principle-odoo-reuse-extend`](./skills/principle-odoo-reuse-extend/SKILL.md) | Referenced by `odoo-mode`. Search Community then Enterprise for something to reuse or extend before scaffolding, and keep custom modules from depending on each other. |

## What this plugin's odoo-mode skill adds over nalgene-mode

- **One extra gate.** Before scaffolding a new model, view, or wizard, spawn an explorer to check whether Community or Enterprise already covers it, in full or in part.
- **A build order for this shape.** `skills/odoo-mode/references/odoo-feature.md` sequences a new Odoo feature (reuse check → scaffold → models → security → views → relations → ...), opened by the gate above. Bug fixes and refactors read the same reference to locate the likely build step or justify restructuring relations/inheritance.

Odoo-aware *exploration* (the Community → Enterprise → Custom track in `nalgene-mode`'s own `how` skill, and the Odoo stack overview in its `teach` skill) stays in `nalgene-mode` — this plugin only adds the action-side gate and build order, the same split `web-mode` uses for the SPA shared-client principle.

## Not shipped here

- Principles, playbooks, the openspec gate, `how`/`why`/`teach`/`tech-advisory` (Odoo exploration included), and the todo-list convention ship in `nalgene-mode`.

## License

MIT
