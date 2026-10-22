# Odoo Feature Reference

Build order for a new Odoo module or capability, condensed from [Server Framework 101](https://www.odoo.com/documentation/18.0/developer/tutorials/server_framework_101.html). Odoo-specific — skip entirely on a non-Odoo project. Each step assumes the ones above it are settled; doing them out of order means redoing work.

1. **Scaffold** — `__manifest__.py` (`depends`, `data`, `installable`), standard folders (`models/`, `views/`, `security/`).
2. **Models & fields** — `_name`, basic field types. The data shape, before any UI or logic touches it.
3. **Security** — `security/ir.model.access.csv` at minimum, groups/record rules if access varies by role. Before views exist, not after.
4. **Views** — form/list/search wired to an action and menu.
5. **Relations** — `Many2one`/`One2many`/`Many2many` once more than one model is involved. Get ownership direction right; expensive to flip later.
6. **Computed fields & onchanges** — `@api.depends`, `@api.onchange`, `compute`/`store` — only once the fields they depend on exist.
7. **Actions / business logic** — methods, buttons, server actions, `ir.cron`.
8. **Constraints** — `@api.constrains`, `_sql_constraints` — once the shape is stable, or they just get rewritten.
9. **Inheritance** — `_inherit` to extend an existing model/view instead of duplicating it. Check `how`'s Odoo track (Community → Enterprise → Custom) first for what already exists to extend.
10. **Cross-module integration** — `depends` on whatever you inherit or call into; load order follows the dependency graph.
11. **QWeb** — only if templating a report or portal page; otherwise skip.

## Using this

- **feature.md**: Odoo project → walk this top to bottom at step 2, skipping steps the feature doesn't need.
- **bug-fix.md**: the bug lives in one step above (a constraint misfiring, an onchange not recomputing) → go straight to that step instead of rebuilding the whole order.
- **refactor.md**: only steps 5 (relations) and 9 (inheritance) are usually in scope for restructuring; the rest shouldn't move.

"Odoo project" means the question names Odoo, or the touched paths look like an Odoo module (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) — same detection `how` uses.
