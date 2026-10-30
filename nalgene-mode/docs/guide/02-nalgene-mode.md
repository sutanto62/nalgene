# 2. Route work through `/nalgene-mode`

State a goal and how you'll know it's done:

```text
/nalgene-mode add a "resend invite" button to the team settings page. done when a resent invite shows a new expiry and the old link 410s.
```

`nalgene-mode` restates the goal and the check, picks one of six playbooks, says why, and runs the rest without you hand-picking skills.

## The six playbooks

| Signal | Playbook |
|---|---|
| Bug, regression, "X doesn't work" | `bug-fix` — reproduce first, root-cause, smallest fix, re-run the repro |
| New capability, endpoint, screen, behavior | `feature` — gate, build against the approved spec, verify the real path |
| Restructuring with no behavior change | `refactor` — default to deletion, behavior-preserving at every commit |
| Stale or orphaned git worktrees, reclaiming disk space | `worktree-cleanup` — audit, pause on uncommitted work, prune the confirmed set, reclaim ecosystem caches |
| Verified work ready to ship, opening a PR | `create-pr` — rebase onto the integration branch, write the PR with `technical-writing` and `plain-speech`, open it ready |
| Authoring or updating a skill in this plugin or a sibling one | `authoring-a-skill` — call `create-a-skill`, then open the PR per `create-pr` |

None fit → `nalgene-mode` says so and proposes the closest one rather than guessing silently.

Once matched, `nalgene-mode` opens a todo list with the playbook's steps before starting — see [`../harness/claude-code.md`](../harness/claude-code.md) or [`../harness/cursor.md`](../harness/cursor.md) for which tool that maps to on your host. A step it skips stays on the list with a one-line reason instead of disappearing.

## What happens automatically

- **openspec-gate** runs before code starts on anything that touches behavior, a public interface, or more than one module. A one-line guard, a typo, a dependency bump, or a local rename skips it — `nalgene-mode` states why.
- **how** runs first for a nontrivial change or an "are we sure how this works" question, before touching code.
- **check-sql-performance** runs wherever a diff touches a SQL transaction — raw SQL, an ORM query with N+1 risk, or a reporting/aggregate view — before the change is marked done.
- Each playbook's steps name the principle that shaped a non-obvious call (root-causing, proving it works, minimizing the diff, converging on a target shape, measuring query cost) — see [page 4](./04-steer-with-principles.md).

## Sibling plugins

`nalgene-mode` itself is fully stack-agnostic — no stack-specific principle lives in its own `Framework` section. A stack- or style-specific layer — Odoo, web/SPA, Android, iOS, backend, or a personal workflow — ships as its own sibling plugin instead. `odoo-mode` and `web-mode` are the first two: each has its own thin router skill that routes through `nalgene-mode`'s own playbooks (the same `feature`/`bug-fix`/`refactor` steps above) and layers one extra gate on top — `odoo-mode` applies **principle-odoo-reuse-extend** before scaffolding a model or view, `web-mode` applies **principle-spa-shared-client** before a feature touches a shared API client or session wiring. See [`odoo-mode/README.md`](../../../odoo-mode/README.md) and [`web-mode/README.md`](../../../web-mode/README.md) — see [page 4](./04-steer-with-principles.md#adding-a-new-principle) for when a new stack warrants a sibling plugin versus a `Framework` entry here.

Odoo-aware *exploration* is a separate matter from the sibling plugins above, and stays inline in `nalgene-mode` rather than moving to `odoo-mode`: `how`'s Community → Enterprise → Custom track (see [page 3](./03-investigate.md#odoo-projects)) and `teach`'s Odoo stack overview are built into those skills' own track systems, not into `nalgene-mode`'s router/Framework machinery, and there's no sibling-plugin mechanism yet for extending another plugin's exploration tracks from outside.

## Worktree cleanup

`worktree-cleanup` is the one playbook that doesn't touch application code, so **openspec-gate** never applies to it — the risk is deleting uncommitted work, not drifting from a spec. It audits every `git worktree list` entry, pauses for a decision on anything with local changes or still in use, and only prunes what you've confirmed. `skills/nalgene-mode/references/worktree-caches.md` lists what else to reclaim per ecosystem — Odoo, Android, iOS, Node.js, and Golang each leave different build caches and simulators/emulators behind that removing a worktree alone doesn't clear.

Next: [Investigate before you touch code](./03-investigate.md).
