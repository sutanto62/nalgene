# Odoo Feature Reference

Build order for a new Odoo module or capability, condensed from [Server Framework 101](https://www.odoo.com/documentation/18.0/developer/tutorials/server_framework_101.html). Odoo-specific — skip entirely on a non-Odoo project. Each step assumes the ones above it are settled; doing them out of order means redoing work.

1. **Reuse check** — apply **principle-odoo-reuse-extend** before touching anything else: search Community then Enterprise for a model, mixin, view, or wizard that already covers this, in full or in part.
2. **Scaffold** — `__manifest__.py` (`depends`, `data`, `installable`), standard folders (`models/`, `views/`, `security/`).
3. **Models & fields** — `_name`, basic field types. The data shape, before any UI or logic touches it.
4. **Security** — `security/ir.model.access.csv` at minimum, groups/record rules if access varies by role. Before views exist, not after.
5. **Views** — form/list/search wired to an action and menu.
6. **Relations** — `Many2one`/`One2many`/`Many2many` once more than one model is involved. Get ownership direction right; expensive to flip later.
7. **Computed fields & onchanges** — `@api.depends`, `@api.onchange`, `compute`/`store` — only once the fields they depend on exist.
8. **Actions / business logic** — methods, buttons, server actions, `ir.cron`.
9. **Constraints** — `@api.constrains`, `_sql_constraints` — once the shape is stable, or they just get rewritten.
10. **Inheritance** — `_inherit` to extend an existing model/view instead of duplicating it. Already scoped by step 1's reuse check; revisit it here only if the shape changed enough that a new candidate to extend might exist.
11. **Cross-module integration** — `depends` on whatever you inherit or call into; load order follows the dependency graph.
12. **QWeb** — only if templating a report or portal page; otherwise skip.

"Odoo project" means the question names Odoo, or the touched paths look like an Odoo module (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) — same detection the `nalgene-mode` plugin's `how` skill uses for its own Odoo track.
