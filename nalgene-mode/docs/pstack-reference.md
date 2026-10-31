# pstack reference

nalgene is a scaled-down, stack-agnostic descendant of [`poteto-mode`/pstack](https://github.com/cursor/plugins/tree/main/pstack): one router, a handful of playbooks and principles, `how`/`why`/`teach`, and a gate. pstack is the same shape at far larger scale — 23 playbooks, 24 principles, design-synthesis and verification-generation machinery nalgene doesn't carry. This page distills [pstack's own guide](https://github.com/cursor/plugins/tree/main/pstack/docs/guide) into what matters when you're deciding whether and how to grow nalgene. Read it before adding a skill, playbook, or principle — it doesn't replace [`docs/guide/`](./guide/README.md), which is for using nalgene, not extending it.

## Same shape, smaller

| nalgene | pstack | Notes |
|---|---|---|
| `nalgene-mode` | `poteto-mode` | Router: goal in, playbook matched, principles applied, steps run. |
| `how` / `why` / `teach` | `/how` / `/why` / `/teach` | Same division of labor: mechanics, motivation, onboarding synthesis. |
| `openspec-gate` | — | nalgene's one addition pstack doesn't have: propose → validate → implement → archive for non-trivial changes. |
| `principle-*` (5) | `principle-*` (24) | Same mechanism — a name the agent reads once, cites, and must show a decision for. |
| `odoo-mode` / `web-mode` (sibling plugins) | single plugin, no siblings | nalgene's real structural departure — see below. |
| `tech-advisory`, `plain-speech`, `technical-writing`, `create-a-skill` | same names, same jobs | Ported close to 1:1. |

## What pstack has that nalgene doesn't — and why that's deliberate, not unfinished

| pstack | What it does | Why nalgene skips it (for now) |
|---|---|---|
| `/architect`, `/arena`, `/interrogate`, `/swarm` | Multi-candidate design synthesis, cross-model adversarial review, coverage fan-out | No design-synthesis layer at all. `openspec-gate` covers propose/validate for one design, not competing candidates. Reconsider only once nalgene sees a contested, expensive-to-reverse design often enough to justify the model-panel setup these need. |
| `/tdd`, `/no-comments`, `/unslop`, `/deslop` | Dedicated build-and-clean passes | No standalone cleanup skills. `fix-root-causes` and `minimize-reader-load` cover some of this ground as rules an agent applies mid-task, not as a separate pass over a finished diff. |
| `/benchmark-checklist`, `/create-verification-skill`, `/create-formal-verification-skill` | General verification-skill and formal-verification-skill generators, a 7-question number-vetting checklist | nalgene has `check-sql-performance` + `measure-query-cost` — narrow, SQL-specific, not generators. No general "scaffold a way to drive this app" or "prove this property" skill yet. |
| `/correct`, `/reflect`, `/automate-me` | Mine history for repeated mistakes and fix the repo; mine transcripts for a personal mode | None of these exist in nalgene. `openspec-gate`'s validate step is the closest analog for "catch a mistake before it ships," but nothing yet turns a *repeated* mistake into a lint, type, or check automatically. |
| Overnight contract, `autopilot-full/stack`, `orchestrate` | Autonomous multi-hour runs with a decision log, PR-queue autopilot, program-scale coordination | No autonomous-run or fleet playbook. `worktree-cleanup` is nalgene's only lifecycle playbook beyond the four build playbooks (`bug-fix`, `feature`, `refactor`, `create-pr`). |
| 24 principles | A full menu across core, architecture, verification, delegation | nalgene has 5 general + 2 stack-specific (in `odoo-mode`/`web-mode`). Pull one at a time when a real repeated failure names it — pstack's list is a menu, not a target count. |

## Patterns worth taking verbatim

- **Say the goal, not the ceremony.** Already nalgene's house rule (`docs/guide/02-nalgene-mode.md`). pstack's phrasing of *why* — "a hand-written sequence usually reorders or drops steps the playbook would have kept" — is worth reusing when explaining the rule to someone new.
- **A principle citation needs a decision behind it.** "The agent still has to say which decision the rule changed. A citation with no decision means the agent named the rule without applying it." Hold nalgene's `principle-*` skills to the same bar.
- **A class counts once it has happened twice.** `/correct`'s threshold for when a recurring mistake deserves a fix in the repo. Use it as the bar for "does this deserve a new principle or skill," even without building `/correct` itself.
- **Rank the fix by how well it holds:** architecture > types/lint/CI > test > doc or agent rule, in that order, docs last because "nothing fails when an agent skips a rule." Useful whenever you're tempted to solve a recurring nalgene failure with a line in a SKILL.md instead of a check.
- **Test a skill change blind, with sanitized candidates.** `create-a-skill`'s `references/evaluating.md` is the scaled-down version of pstack's eval playbook; check it still respects the observer-effect warning (an agent that knows it's being evaluated behaves differently) if you extend it.
- **Don't enumerate skills in the prompt; don't skip the routing skill because "the agent will figure it out."** Both pitfalls appear near-verbatim in nalgene's own `06-recipes-and-pitfalls.md` — confirmation the port held, not something to re-derive.

## nalgene's one real departure: sibling plugins, not one Framework section

pstack is a single plugin. nalgene forks stack- or style-specific behavior (`odoo-mode`, `web-mode`) into separate sibling plugins that route through `nalgene-mode`'s own playbooks, instead of growing `nalgene-mode`'s `Framework` section per stack. This is the one place nalgene's architecture diverges from pstack's on purpose — see the root [`README.md`](../../README.md#adding-a-new-plugin) and `nalgene-mode`'s own [`README.md`](../README.md#status). Don't let a pstack pattern pull this back toward one growing plugin.

## Checklist before porting a pstack pattern into nalgene

1. **Does a `principle-*` skill already cover this as a rule an agent applies, instead of a new skill or playbook?** Prefer the rule.
2. **Is it stack- or style-specific?** It belongs in a sibling plugin, not `nalgene-mode` — see `create-a-skill`'s step 2 placement test.
3. **Can the `SKILL.md` stay well under 100 lines?** If porting the idea needs pstack's full design-panel or model-role machinery to work, that's a sign nalgene isn't at the scale where it pays off yet — port the one-sentence guardrail instead of the machinery.
4. **Would this duplicate `openspec-gate`'s job?** Non-trivial changes already gate through propose → validate → implement → archive; a new review pass shouldn't re-litigate what the gate already covers.
5. **New skill → route it through `create-a-skill`,** the same rule pstack states for its own `SKILL.md` authoring ("writing a skill matches the Authoring... playbook... let the playbook hold that bar rather than writing a SKILL.md freehand").

## Source

Full guide: <https://github.com/cursor/plugins/tree/main/pstack/docs/guide>. Short on time? Read page 2 (`poteto-mode` routing) and page 8 (principles) — the rest is pstack's larger surface area, not its core idea.
