---
name: odoo-mode
description: "Odoo agent style layered on nalgene-mode: reuse Community/Enterprise before scaffolding anything new, routed through nalgene-mode's playbooks. Use for /odoo-mode, or nontrivial work on an Odoo module."
disable-model-invocation: true
---

# Odoo mode

Thin router over the `nalgene-mode` plugin's own `nalgene-mode` skill. Shared principles and playbook machinery live there — read them from it, don't restate them here.

**Requires** the `nalgene-mode` plugin. Check first: the `nalgene-mode` skill should be in your skill list. Missing → stop and tell the user to install it (`/plugin install nalgene-mode@nalgene` in Claude Code, `/add-plugin nalgene-mode` in Cursor).

## Sibling skills (this plugin)

| Skill | Job |
|---|---|
| `odoo-mode` | This router |
| `principle-odoo-reuse-extend` | The Odoo gate below |
| `odoo-testing` | The Testing gate below |

## Shared machinery (do not restate)

Read these from the `nalgene-mode` plugin. Do not copy their contents into this file.

| Layer | Where |
|---|---|
| Shared mode (Principles index, non-negotiables, playbook catalog, todo-list convention) | `nalgene-mode`'s own `nalgene-mode` skill |
| Shared playbooks | `nalgene-mode/playbooks/<name>.md` |
| General principle leaves (`fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`, `report-dont-recommend`) | `nalgene-mode`'s `principle-*` skills |
| openspec gate | `nalgene-mode`'s `openspec-gate` skill |
| "How does X work", including the Community → Enterprise → Custom exploration track | `nalgene-mode`'s `how` skill |

## Non-negotiables

1. Open a todo list. Item 1: read `nalgene-mode`'s **Principles** section in full. Cite each principle you apply with the concrete choice it changed.
2. Match a `nalgene-mode` playbook (`feature`, `bug-fix`, `refactor`, `worktree-cleanup`). Copy its steps into the todo list before the Odoo gate below. A skipped step stays with a one-line reason instead of being dropped.
3. Apply `nalgene-mode`'s own non-negotiables (openspec-gate before code on anything touching behavior or a public interface, **how** before a nontrivial change). Do not re-list them here.
4. Apply the **Odoo gate** below wherever the playbook's own step would otherwise have you scaffold a new model, view, or wizard from scratch. It only fires when the project is in scope — see Detection.
5. Apply the **Testing gate** below wherever the playbook's own verification step (`principle-prove-it-works`) would run backend tests. Same scope condition.

### Odoo gate

- **Reuse and extend.** Apply **principle-odoo-reuse-extend** (leaf, this plugin): before scaffolding, spawn an explorer to check whether Community or Enterprise already covers the feature, in full or in part, and keep custom modules from depending on each other directly.
- In a matched **Feature** or **Bug fix** playbook, this gate opens the build order: follow `references/odoo-feature.md` instead of improvising one. In a **Refactor** restructuring relations or inheritance, check that reference's steps 5 and 9 before moving anything.

### Testing gate

- **Scoped runs only.** Apply **odoo-testing** (leaf, this plugin): tag the test classes for the module under test with its technical name, then verify with `--test-tags module_name` — never an unscoped `--test-enable` run across every installed module.
- Full flow (writing tests, install/upgrade order, narrowing to one failure, combining modules): `odoo-testing`'s `playbooks/backend-testing.md`. Copy its steps into the todo list wherever the matched `nalgene-mode` playbook's verification step lands.

### Detection

"Odoo project" means the question names Odoo, or the touched paths look like an Odoo module (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) — same detection the `nalgene-mode` plugin's `how` skill uses for its own Odoo track. Not an Odoo project → this plugin doesn't apply; run `nalgene-mode` directly instead of `/odoo-mode`.

## Report

Same as the matched `nalgene-mode` playbook's own report line, plus:
- Which Community/Enterprise option was found and chosen, even when the answer is "nothing reusable."
- The name(s) of the Odoo module(s) touched — created or modified (the `__manifest__.py` `name`, or its technical directory name if a `name` hasn't been set yet).
- The `--test-tags` value used to verify, and the pass/fail count, whenever backend tests ran.
