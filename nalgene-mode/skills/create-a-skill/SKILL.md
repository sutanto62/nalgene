---
name: create-a-skill
description: "Scaffold a new skill's SKILL.md, validate its frontmatter and structure against the Agent Skills spec (agentskills.io), run a lightweight evaluation pass, then commit and open a PR for it. Use whenever asked to create, author, add, or scaffold a skill, write a new SKILL.md, or check an existing one against spec - for '/create-a-skill', 'make a skill for X', 'new skill', or 'does this skill meet spec'."
disable-model-invocation: true
---

# Create a skill

Scaffolds a skill in this repo's house voice, then validates, lightly evaluates, and opens a PR for it. Modeled on Anthropic's `skill-creator` and Cursor's `create-skill`, scaled down to match how small skills in this repo actually are — most run 10-80 lines.

## 1. Capture intent

Settle three things before writing anything, in one pass, not an interview: what should the skill make the agent do, what phrases or contexts should trigger it, and what should the output look like. If the conversation already shows the workflow (e.g. "turn what we just did into a skill"), pull answers from it instead of asking. If the user proposes a plugin or name, still check it in step 2 — a plausible-sounding suggestion ("put it in web-mode since it's SPA-adjacent") can still be a scope mismatch once you read that plugin's own description.

## 2. Place it

Don't place a skill by category label alone. Read the target plugin's own `<plugin>/skills/<plugin>/SKILL.md` description first and confirm this skill's job actually fits what that plugin already claims — `web-mode` is specifically API-client/session reuse, not "anything frontend"; `odoo-mode` is specifically Community/Enterprise reuse, not "anything Odoo." A skill that's merely adjacent to a plugin's stack, but not actually about what that plugin's router enforces, belongs in `nalgene-mode` instead. A different plugin than these three fits only if none of them do — and that's a new-plugin task, bigger than this skill covers; use the existing plugin dirs as the template instead.

Pick the name now, kebab-case: `principle-<title>` for a skill that steers mid-task with no steps of its own, a plain descriptive name for a task skill. Unicode lowercase alphanumeric and hyphens only — no underscores, no `+` or other symbols, no leading/trailing or doubled hyphen (`n+1-query-check` is invalid; `n-plus-one-query-check` is the fix). The spec requires the directory name and the frontmatter `name` to match exactly — decide both together so they can't drift.

## 3. Write SKILL.md

Frontmatter: `name`, `description` (what it does *and* when to trigger it, with concrete phrases — not "helps with X"; skills undertrigger by default, so name the trigger words explicitly), `disable-model-invocation: true` (every skill in this repo sets this; only leave it unset with a clear reason for wanting auto-invocation).

Body, in this repo's voice:
- Numbered `## Steps` for a task skill (see `openspec-gate`), or a short `**Why:**` + `**Pattern:**` pair for a principle skill (see `principle-prove-it-works`).
- Imperative, not narrative. Explain the why only where the rule would be confusing without it.
- Keep it well under the spec's 500-line ceiling — if you're past ~100 lines, move detail into `references/` and point at it by relative path, the way `teach` points at `references/stack-overviews.md`.

## 4. Validate against spec

Walk `references/agentskills-checklist.md` in full — don't skip items because they look obviously fine; the `name`/`description` constraints fail silently in some harnesses if violated.

## 5. Evaluate, scaled to the skill

Structural skill (fixed steps, a checkable output) → run the lightweight loop in `references/evaluating.md`. Subjective skill (voice, judgment calls, a principle skill) → skip formal evals; just re-read it against 1-2 real scenarios and confirm nothing contradicts its own Why.

## 6. Register discoverability

If the skill is meant for people to find, not just for another skill to call, add one row to `nalgene-mode/skills/nalgene-help/SKILL.md`'s table. If you placed it in `web-mode` or `odoo-mode`, also add it to that plugin's own `SKILL.md` — each carries its own "Sibling skills" table, a second discoverability surface nalgene-help doesn't cover. Skip registration entirely for a skill that only ever gets invoked by another skill.

## 7. Create PR

Run nalgene-mode's **create-pr** playbook to commit, push, and open the PR — don't reimplement those mechanics here. Give it a conventional-commit message (`type(scope): summary` — `feat(<plugin>): add <skill-name> for ...`) and a PR body stating what the skill does, the validation result from step 4, and the eval result from step 5 (or why it was skipped).

## Why

A skill nobody validated against spec is a bet that it'll load correctly somewhere it hasn't been tried. A skill nobody evaluated against a real prompt is a bet that the steps you imagined are the steps that actually fire. Both bets are cheap to settle before the skill ships, expensive to debug after.
