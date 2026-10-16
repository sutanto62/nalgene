# Explorer Prompt Template (Odoo)

Build each explorer subagent's prompt from this template. Fill in the placeholders.

---

Other explorers are investigating different slices of the same module/feature in parallel. Don't try to cover everything. Focus on your assigned angle and go deep.

## Question

> {QUESTION}

## Your Exploration Angle

{EXPLORATION_ANGLE}

For a layered question, your angle is one layer, not an arbitrary slice — see Exploration order below.

## Project layout

Paths below are relative to the project root. Resolved once by `how` before any explorer spawns — don't rediscover them yourself; if one reads `(not applicable)`, that layer isn't part of this project, skip it.

- **Community (CE)**: `{CE_PATH}`
- **Enterprise (EE)**: `{EE_PATH}`
- **Custom addons**: `{CUSTOM_PATH}`

## Exploration order (layers)

Odoo behavior is the sum of up to three layers stacked on top of each other, each capable of overriding what came before it. Trace them in this order, and tag every finding with the layer it came from:

1. **Community (CE)** at `{CE_PATH}` — the base/core addons. This is the baseline behavior as shipped.
2. **Enterprise (EE)** at `{EE_PATH}` — only if the feature plausibly has an Enterprise variant (accounting, documents, helpdesk, planning, and similar are common ones; many features don't). If `{EE_PATH}` is `(not applicable)` or the feature doesn't plausibly touch it, say so and skip this layer.
3. **Custom addons** at `{CUSTOM_PATH}` — the project's own modules. These can `_inherit` CE, EE, or both.

A later layer's `_inherit` doesn't replace the earlier definition, it extends it — and module load order (driven by each module's `depends`) decides which layer's method or view-`xpath` change actually wins when two layers touch the same thing. Note that load order, don't assume Custom always wins just because it's "yours."

## Where to look

- **Manifest**: `__manifest__.py` — `depends`, `data`, `installable`, module boundaries.
- **Models**: `models/*.py` — `_name` vs `_inherit`/`_inherits`, fields (and `compute`/`depends`/`store`/`related`), `@api.constrains`/`@api.onchange`, SQL constraints, `_sql_constraints`.
- **Views**: `views/*.xml` — form/list/search/kanban, inherited views (`xpath`), actions (`ir.actions.act_window`), menus (`ir.ui.menu`).
- **Security**: `security/ir.model.access.csv` (CRUD per group), `security/*.xml` (record rules, groups).
- **Data**: `data/*.xml` — demo vs non-demo, `ir.cron`, mail templates, sequences.
- **Controllers**: `controllers/*.py` — `@http.route`, auth type, `type="http"` vs `"json"`.
- **Wizards**: `TransientModel` classes — the interaction they drive, what they write back to.

## Output

### Components Found
Models, views, controllers, wizards involved. For each: technical name (e.g. `sale.order.line`), file path, **which layer defines or touches it (CE / EE / Custom)**, one-sentence description.

### Flow
The execution flow step by step: what method/view/action runs, **in which layer**, what it does, what it calls next (compute methods, onchange, cron, workflow transitions). Include what data moves (fields, recordset context) between steps, and flag where a later layer overrides or extends an earlier one's behavior.

### Files Read
Every file you read, so the explainer can reference them.

### Boundaries
What this feature depends on (other modules via `_inherit` or `depends`) and what depends on it.

### Non-Obvious Things
Recordset gotchas (singleton assumptions, iterating without guarding), `sudo()`/ACL bypasses, compute/depends chains that recompute more than expected, module load-order assumptions, a Custom or EE override that silently shadows another layer's behavior, multi-company or multi-currency handling, translation (`_()`) usage, anything that looks like it should work one way but works another.

### Open Questions
Anything you couldn't fully trace. Be honest about gaps.
