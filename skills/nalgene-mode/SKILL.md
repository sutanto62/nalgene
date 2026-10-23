---
name: nalgene-mode
description: "Router for nontrivial work: give it a goal and a way to check it's done, it picks a playbook, gates on openspec when the change is non-trivial, and runs the right skills as each step fires. Use for /nalgene-mode or requests to work in this style."
disable-model-invocation: true
---

# Nalgene mode

Give a goal in plain words and how you'll know it's done. This skill picks a playbook, drives it step by step, and reports what it skipped and why.

## Routing

- Bug, regression, "X doesn't work" → `playbooks/bug-fix.md`.
- New capability, endpoint, screen, or behavior → `playbooks/feature.md`.
- Restructuring with no behavior change → `playbooks/refactor.md`.
- None of these fit → say so, propose the closest playbook, ask before deviating.

## Non-negotiables

- Nontrivial change, architecture decision, or an "are we sure how this works" question before you touch code → the **how** skill.
- Any change that touches behavior, a public interface, or more than one module goes through **openspec-gate** before code starts. Trivial fixes (typo, one-line guard, dependency bump, local rename) skip it — state why when skipping.
- Root-cause every bug, don't patch the symptom. See **principle-fix-root-causes**.
- Prove it works by running it. See **principle-prove-it-works**.
- Keep the diff the reader can hold in their head. See **principle-minimize-reader-load**.
- State the target shape of a multi-step change up front and converge on it. See **principle-outcome-oriented-execution**.

## Steps

1. Restate the goal and the check that proves it's done, in one or two sentences. If either is missing, ask once.
2. Pick the playbook. Say which one and why.
3. Run **openspec-gate**, unless the change is trivial (say why if skipped).
4. Follow the playbook. Name the principle that shaped any non-obvious choice.
5. Report: what changed, how it was verified, what's still open.

A question about which skill fits a situation, not a task to do, goes to **help** instead.
