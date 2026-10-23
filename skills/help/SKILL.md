---
name: help
description: "Answers 'which skill for X' without doing the work. Type /help with a question about this plugin."
disable-model-invocation: true
---

# Help

Answer the question. Don't start the work — that's `nalgene-mode`'s job, and a run spends real tokens.

| Need | Use |
|---|---|
| Nontrivial task, want it routed and gated | `/nalgene-mode` |
| "How does X work", before changing something | `/how` |
| "Why does X work this way", design rationale, regressions | `/why` |
| Check what openspec already says, read-only | `/openspec-explore` |
| Just need a change proposed, validated, or archived | `/openspec-gate` |
| Steer mid-task toward root-causing, not patching | `/principle-fix-root-causes` |
| Steer toward proving a claim instead of asserting it | `/principle-prove-it-works` |
| Steer toward a smaller, more readable diff | `/principle-minimize-reader-load` |
| Steer a multi-step migration toward its end state, not a permanent halfway house | `/principle-outcome-oriented-execution` |

If the ask is actually a task ("fix this", "add X"), don't answer from this table — hand it to `/nalgene-mode` instead.
