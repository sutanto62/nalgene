---
name: odoo-mode
description: "Odoo agent style layered on nalgene-mode: reuse Community/Enterprise before scaffolding anything new, routed through nalgene-mode's playbooks. Use for /odoo-mode, or nontrivial work on an Odoo module."
disable-model-invocation: true
---

# Odoo mode

Thin router over the `nalgene-mode` plugin's own `nalgene-mode` skill. Shared principles and playbook machinery live there — read them from it, don't restate them here.

**Requires** the `nalgene-mode` plugin. Check first: the `nalgene-mode` skill should be in your skill list. Missing → stop and tell the user to install it (`/plugin install nalgene-mode@nalgene` in Claude Code, `/add-plugin nalgene-mode` in Cursor).

Every reference to `nalgene-mode` below — its Principles section, a playbook, `openspec-gate`, `how` — means `Read` that file directly off disk and follow it, not a `Skill`-tool call into `nalgene-mode`. On Claude Code, read `nalgene-mode/docs/harness/claude-code.md` once per session before following any of these references; Cursor has no such restriction.

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
2. Match a `nalgene-mode` playbook from its own catalog (`nalgene-mode`'s `SKILL.md` — do not hard-code a subset here, it drifts). This includes `playbooks/create-pr.md` for any request to open a PR: that match is mandatory and applies the same way in Odoo work as anywhere else, including mid-session or as a standalone follow-up. For `feature`, `bug-fix`, `refactor`, or `worktree-cleanup`, copy its steps into the todo list before the Odoo gate below. A skipped step stays with a one-line reason instead of being dropped.
3. Apply `nalgene-mode`'s own non-negotiables (openspec-gate before code on anything touching behavior or a public interface, **how** before a nontrivial change). Do not re-list them here.
4. Apply the **Odoo gate** below wherever the playbook's own step would otherwise have you scaffold a new model, view, or wizard from scratch. It only fires when the project is in scope — see Detection.
5. Apply the **Testing gate** below wherever the playbook's own verification step (`principle-prove-it-works`) would run any kind of Odoo test — Python, JS, or integration. Same scope condition.
6. Apply the **State gate** below whenever the request would insert, rename, or alter any model's state. It runs ahead of non-negotiable 3's `openspec-gate` step — the playbook's own confirmation step decides whether and how that step fires, not the other way round. Same scope condition.

### Odoo gate

- **Reuse and extend.** Apply **principle-odoo-reuse-extend** (leaf, this plugin): before scaffolding, spawn an explorer to check whether Community or Enterprise already covers the feature, in full or in part, and keep custom modules from depending on each other directly.
- In a matched **Feature** or **Bug fix** playbook, this gate opens the build order: follow `references/odoo-feature.md` instead of improvising one. In a **Refactor** restructuring relations or inheritance, check that reference's steps 5 and 9 before moving anything.

### Testing gate

- **Scoped runs only.** Apply **odoo-testing** (leaf, this plugin) — one playbook per test kind (Python backend, JS unit, integration/tour), each scoping its own run instead of re-executing everything.
- Pick the matching playbook and copy its steps into the todo list wherever the matched `nalgene-mode` playbook's verification step lands: `odoo-testing`'s own `SKILL.md` carries the dispatch table (`playbooks/backend-testing.md`, `playbooks/js-testing.md`, `playbooks/integration-testing.md`) — don't hard-code a single one here, it drifts.

### State gate

- **Map blast radius before proposing.** Before inserting, renaming, or altering any model's state, check the canonical model's state behavior, then the custom model's state behavior, analyze impact, and report it with a statechart before anything reaches openspec.
- Full procedure: this plugin's own `playbooks/state-change-impact.md`. Copy its steps into the todo list ahead of non-negotiable 3's `openspec-gate` step.
- This gate ends in a stop-and-ask: don't let the matched playbook or non-negotiable 3 carry the work into `openspec-gate` until the user has confirmed the reported impact.

### Detection

"Odoo project" means the question names Odoo, or the touched paths look like an Odoo module (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) — same detection the `nalgene-mode` plugin's `how` skill uses for its own Odoo track. Not an Odoo project → this plugin doesn't apply; run `nalgene-mode` directly instead of `/odoo-mode`.

## Report

Same as the matched `nalgene-mode` playbook's own report line, plus:
- Which Community/Enterprise option was found and chosen, even when the answer is "nothing reusable."
- The name(s) of the Odoo module(s) touched — created or modified (the `__manifest__.py` `name`, or its technical directory name if a `name` hasn't been set yet).
- Which playbook(s) applied (backend/JS/integration), the exact scoping value used to verify (`--test-tags`, `?filter=`/`?tag=`, or a tour name), and the pass/fail count, whenever tests ran.
- For a state-change request: the blast-radius list (breaks / extend / unaffected-by-design) and whether the user confirmed before openspec started.
