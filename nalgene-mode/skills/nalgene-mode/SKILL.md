---
name: nalgene-mode
description: "Router for nontrivial work: give it a goal and a way to check it's done, it picks a playbook, gates on openspec when the change is non-trivial, and runs the right skills as each step fires. Use for /nalgene-mode or requests to work in this style."
disable-model-invocation: true
---

# Nalgene mode

Give a goal in plain words and how you'll know it's done. This skill picks a playbook, drives it step by step, and reports what it skipped and why.

## Playbooks

- Bug, regression, "X doesn't work" → `playbooks/bug-fix.md`.
- New capability, endpoint, screen, or behavior → `playbooks/feature.md`.
- Restructuring with no behavior change → `playbooks/refactor.md`.
- Stale or orphaned git worktrees, reclaiming disk space → `playbooks/worktree-cleanup.md`.
- Verified work ready to ship, opening a PR → `playbooks/create-pr.md`.
- Authoring or updating a skill in this plugin or a sibling one → `playbooks/authoring-a-skill.md`.
- None of these fit → say so, propose the closest playbook, ask before deviating.

Once matched, open a todo list whose first items are the matched playbook's steps, copied in verbatim, before starting work on any task-specific todos. Mark each in progress when you start it and done when it's done. A step you choose not to do stays on the list with a one-line reason instead of being deleted.

## Non-negotiables

- Nontrivial change, architecture decision, or an "are we sure how this works" question before you touch code → the **how** skill.
- Any change that touches behavior, a public interface, or more than one module goes through **openspec-gate** before code starts. Trivial fixes (typo, one-line guard, dependency bump, local rename) skip it — state why when skipping.
- The Principles section below grounds every non-obvious call. Name each principle that shaped a decision and the specific choice it changed. Cite only principles whose leaf `SKILL.md` you read this run.

## Principles

Read the leaf skill in full for any principle you apply. Each entry names when it applies. Each leaf is the `principle-<title>` skill, with the title in kebab case, hyphens kept and other punctuation dropped, unless the entry gives its name.

**Core**

- **Minimize Reader Load.** Writing or reviewing a diff. Favor the version a reviewer can hold in their head — fewer layers, less hidden state, no comments that restate the code.
- **Outcome-Oriented Execution.** Planning a multi-step change, migration, or rewrite with explicit phases. Converge on the target shape, don't let a temporary compatibility state become permanent.
- **Report, Don't Recommend.** Presenting options, research findings, or a comparison the person will decide on. Describe tradeoffs; don't resolve the decision for them, even with a hedge.

**Verification**

- **Fix Root Causes.** Debugging. Trace a symptom back to the first place the system went wrong and fix it there. A guard is a fix only when the state it handles is legitimate.
- **Prove It Works.** After a task, before declaring done. Verify against the real artifact, not a proxy or "it compiles."

No **Framework** entries currently — every stack-specific principle discovered so far (Odoo, web/SPA) ships as a sibling plugin instead of growing this list. See `docs/guide/04-steer-with-principles.md` for when one would still belong here.

## Steps

1. Restate the goal and the check that proves it's done, in one or two sentences. If either is missing, ask once.
2. Pick the playbook. Say which one and why.
3. Read `openspec-gate`'s `SKILL.md` in full and follow it, unless the change is trivial (say why if skipped). `disable-model-invocation: true` on `openspec-gate` keeps the `Skill` tool from dispatching to it from inside this run — load it from disk the same way the Principles section does (see `docs/harness/claude-code.md` on Claude Code; Cursor has no such restriction).
4. Follow the playbook. Name the principle that shaped any non-obvious choice, citing only ones whose leaf you actually read this run.
5. Report: what changed, how it was verified, what's still open.

A question about which skill fits a situation, not a task to do, goes to **nalgene-help** instead.
