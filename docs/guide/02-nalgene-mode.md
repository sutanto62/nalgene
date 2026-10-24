# 2. Route work through `/nalgene-mode`

State a goal and how you'll know it's done:

```text
/nalgene-mode add a "resend invite" button to the team settings page. done when a resent invite shows a new expiry and the old link 410s.
```

`nalgene-mode` restates the goal and the check, picks one of three playbooks, says why, and runs the rest without you hand-picking skills.

## The three playbooks

| Signal | Playbook |
|---|---|
| Bug, regression, "X doesn't work" | `bug-fix` — reproduce first, root-cause, smallest fix, re-run the repro |
| New capability, endpoint, screen, behavior | `feature` — gate, build against the approved spec, verify the real path |
| Restructuring with no behavior change | `refactor` — default to deletion, behavior-preserving at every commit |

None fit → `nalgene-mode` says so and proposes the closest one rather than guessing silently.

Once matched, `nalgene-mode` opens a todo list with the playbook's steps before starting — see [`../harness/claude-code.md`](../harness/claude-code.md) or [`../harness/cursor.md`](../harness/cursor.md) for which tool that maps to on your host. A step it skips stays on the list with a one-line reason instead of disappearing.

## What happens automatically

- **openspec-gate** runs before code starts on anything that touches behavior, a public interface, or more than one module. A one-line guard, a typo, a dependency bump, or a local rename skips it — `nalgene-mode` states why.
- **how** runs first for a nontrivial change or an "are we sure how this works" question, before touching code.
- Each playbook's steps name the principle that shaped a non-obvious call (root-causing, proving it works, minimizing the diff, converging on a target shape) — see [page 4](./04-steer-with-principles.md).

## Odoo projects

Feature and refactor work on an Odoo project follows `skills/nalgene-mode/references/odoo-feature.md`'s build order (reuse check → scaffold → models → security → views → relations → ...) instead of improvising one. The build order opens with **principle-odoo-reuse-extend**: before anything is scaffolded, an explorer checks whether Community or Enterprise already covers the feature, in full or in part, so a new model or view only gets written once that search comes back empty. Bug fixes use the same reference to guess which build step the bug likely sits in, so the fix starts in the right place instead of a fresh search.

Next: [Investigate before you touch code](./03-investigate.md).
