# 4. Steer with principle names

Five one-rule skills you invoke by name mid-task, in plain language, to redirect the agent without restating the whole instruction:

| Say this | To get |
|---|---|
| "apply fix-root-causes" | trace the symptom back to the first wrong decision, don't patch where it surfaced |
| "apply prove-it-works" | run it and show the real output, not "should work now" |
| "apply minimize-reader-load" | the smallest diff a reviewer can hold in their head, no padding |
| "apply outcome-oriented-execution" | name the target shape of a multi-step change and converge on it, no permanent halfway state |
| "apply odoo-reuse-extend" (Odoo projects only) | search Community then Enterprise for something to extend before scaffolding a new model or view, and keep the new module from depending directly on another custom module |

```text
apply prove-it-works before you report this as fixed
```

`nalgene-mode`'s own `SKILL.md` has a **Principles** section grouping the first four under **Core** and **Verification**, and `odoo-reuse-extend` under **Framework** — it reads the matching leaf in full and applies it per playbook step without you asking, and the Odoo feature build order (page 2) already opens with `odoo-reuse-extend`. Naming one explicitly is for steering mid-task when you see the agent drifting — patching a symptom, claiming success without running anything, padding a diff, stalling a migration halfway, reaching for a new model before checking what already exists — not a prerequisite to invoking the others.

## Adding a new principle

A principle is one file, `skills/principle-<kebab-title>/SKILL.md`: `name`, a `description` that starts with "Apply when ...", `disable-model-invocation: true`, then a short body with a **Why** (the cost of skipping it) and a **Pattern** (the concrete steps). Keep it to one page — the four generic principles and `odoo-reuse-extend` are the template to copy.

Where it gets wired in depends on its scope:

- **General** (applies on every project, any stack) → add it under **Core** or **Verification** in `nalgene-mode`'s own Principles section, and reference it from the relevant `playbooks/*.md`, the way `fix-root-causes` is referenced from `bug-fix.md`.
- **Framework-specific** (like `odoo-reuse-extend`) → it still gets a one-line entry under **Framework** in `nalgene-mode`'s Principles section (so the trigger is discoverable), but the real wiring is into that framework's own reference doc and the playbook step that already branches on the framework — `odoo-reuse-extend` sits at the top of `odoo-feature.md`'s build order, referenced from `feature.md`'s Odoo line.

Either way, finish the same checklist:

1. Reference it from the playbook step(s) or framework reference where it applies.
2. Add a row to `nalgene-help`'s lookup table.
3. Add a row to the table above, noting if it's framework-specific.
4. If it's framework-specific, update that framework's section on [page 2](./02-nalgene-mode.md#odoo-projects) (or wherever that playbook's framework notes live).
5. Add a pitfall to [page 6](./06-recipes-and-pitfalls.md) for the failure mode the principle exists to prevent.

This is the exact sequence `odoo-reuse-extend` went through — `git log` for that principle's commit is a worked example of all five steps.

Next: [Gate non-trivial changes](./05-openspec-gate.md).
