---
name: openspec-explore
description: "Read-only lookup of what openspec already documents about a feature, before exploring code. Use before answering a 'how does X work' question, and as step 0 of the how skill."
disable-model-invocation: true
---

# openspec explore

Check what's already specified before spending a dive into code. A behavior documented in a spec is the intended design; code that disagrees with it is either a bug or a spec that's gone stale — worth flagging either way.

This is read-only. It never proposes, edits, or archives anything — that's **openspec-gate**.

## Steps

1. `openspec list` and `openspec list --specs` — see what changes and specs exist at all.
2. Match the question or symbol against spec/change names. No exact match → `openspec view` for the dashboard, or grep spec titles/content for the relevant term.
3. For anything that matches, `openspec show <item>` to read it.
4. Report: the matching spec/change (if any) and what it says, or that nothing in openspec covers this — in which case the code itself is the only source of truth.

## Output

A short note, not a report: what openspec says (or "nothing found"), and whether the live code's behavior matches it. Hand this to the **how** skill's explainer as context, or answer directly if that's all that was asked.
