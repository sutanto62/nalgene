---
name: nalgene-help
description: "Answers 'which skill for X' without doing the work. Type /nalgene-help with a question about this plugin."
disable-model-invocation: true
---

# Nalgene help

Answer the question. Don't start the work — that's `nalgene-mode`'s job, and a run spends real tokens.

| Need | Use |
|---|---|
| Nontrivial task, want it routed and gated | `/nalgene-mode` |
| "How does X work", before changing something | `/how` |
| "Why does X work this way", design rationale, regressions | `/why` |
| New to a subsystem, want it taught from the ground up, not just explained | `/teach` |
| Outside perspective on a tech/library/pattern/model: news, known issues, alternatives, no recommendation | `/tech-advisory` |
| Rewrite text so it reads like a human wrote it, in plain words anyone can follow | `/plain-speech` |
| Write or review a doc, PR description, or commit message against a layered writing standard | `/technical-writing` |
| Check what openspec already says, read-only | `/openspec-explore` |
| Just need a change proposed, validated, or archived | `/openspec-gate` |
| Scaffold a new skill, validate it against the Agent Skills spec, evaluate it, and open a PR | `/create-a-skill` |
| Steer mid-task toward root-causing, not patching | `/principle-fix-root-causes` |
| Steer toward proving a claim instead of asserting it | `/principle-prove-it-works` |
| Steer toward a smaller, more readable diff | `/principle-minimize-reader-load` |
| Steer a multi-step migration toward its end state, not a permanent halfway house | `/principle-outcome-oriented-execution` |
| Steer toward describing tradeoffs instead of picking a winner, even a hedged one | `/principle-report-dont-recommend` |
| Odoo feature: check Community/Enterprise for something to reuse or extend, and keep custom modules from depending on each other | `/odoo-mode` (`odoo-mode` plugin) |
| Web/SPA feature: reuse the app's API client and session wiring instead of duplicating it | `/web-mode` (`web-mode` plugin) |

If the ask is actually a task ("fix this", "add X"), don't answer from this table — hand it to `/nalgene-mode` instead.
