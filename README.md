# nalgene

A minimal, stack-agnostic agent workflow plugin for Cursor and Claude Code: one router, one gate, and a handful of principles — no platform-specific skills yet.

## What's here

- **`nalgene-mode`** — the router. Give it a goal and a way to check it's done; it picks a playbook (`bug-fix`, `feature`, `refactor`, `worktree-cleanup`), gates non-trivial changes through `openspec-gate`, and reports what it skipped and why. On an Odoo project, the playbooks point at `references/odoo-feature.md` for build order instead of improvising one; `worktree-cleanup` points at `references/worktree-caches.md` for the Odoo/Android/iOS/Node.js/Golang caches and simulators a `git worktree remove` doesn't clear.
- **`how`** — "how does X work" / architecture walkthroughs. Checks `openspec-explore` first, routes to a general or framework-specific (Odoo) explorer/explainer track, and fans out read-only `nalgene-explorer` subagents for complex questions.
- **`why`** — "why does X work this way" / design rationale, regressions, and historical motivation. Fixed two-source roster (git + `gh`, and openspec documents), fans out read-only `nalgene-explorer` investigators per source, then a synthesizer produces a confidence-tiered, cited answer. Defers to `how` for "what the code does" instead of re-deriving it. Deliberately scoped to two sources (no MCP discovery) and no Odoo track yet.
- **`teach`** — "teach me this" / onboarding a new learner. Composes `how` and `why` into one plain explanation, opening with a short architecture overview for the project's detected stack (Odoo, Android, iOS, Node.js, Go, and a few more — see `skills/teach/references/stack-overviews.md`) before the specific question, so there's a map to place the rest on. Every reply runs through `plain-speech` before being shown instead of a separate prose-style mandate.
- **`tech-advisory`** — outside perspective on a technology, library, pattern, algorithm, or AI model: recent news, known issues, alternatives, community sentiment, via live web research. Routes the ask into one of three playbooks (`landscape`, `known-issue`, `compare-options`, `skills/tech-advisory/playbooks/`), each naming its own research angles in priority order, with investigators capped at 3 concurrent and queued beyond that — the synthesizer always runs last, after every queued angle returns. Same investigator/synthesizer shape as `why` (fans out read-only `nalgene-explorer`s, then a synthesizer), but the epistemics are tuned for the open web instead of git history: confidence tiers run Documented → Corroborated → Reported sentiment → Speculative → Unknown, and the synthesizer never recommends a course of action — it names where this project's own scale or constraints might not fit something and stops there. Standalone; doesn't run as part of `teach`, though it runs every reply through `plain-speech` the same way `teach` does.
- **`plain-speech`** — rewrite text so anyone, including a non-technical reader, understands it in one read with no AI-sounding tells left in it. Runs a numbered catalog of AI writing tells through a draft → critique → final workflow with a no-fabrication rule, plus a plain-language simplification pass that neither alone covers (`skills/plain-speech/references/plain-language.md`). Invoke it standalone, or get it automatically — both `teach` and `tech-advisory` run every reply through it before showing it.
- **`openspec-gate`** — drives `openspec`'s propose → validate → implement → archive loop. Called by `nalgene-mode` for any change that touches behavior, an API, or more than one module; invoke directly for anything else.
- **`openspec-explore`** — read-only: what does openspec already document about this? Used by `how`, or standalone.
- **`nalgene-help`** — answers "which skill for X" without spending tokens doing work. Type `/nalgene-help <question>`.
- **`principle-*`** — one-rule skills you invoke by name mid-task to redirect the agent: `fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`, `report-dont-recommend`.
- **`agents/nalgene-explorer`** — the plugin's own read-only, general-purpose subagent, shared by `how` and `why`. Bundled rather than relying on a host's built-in equivalent, so both skills work the same in Cursor and Claude Code.

## Install

**Claude Code:** this repo is its own marketplace (`.claude-plugin/marketplace.json`, one entry pointing at the repo root). In Claude Code:

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene@nalgene
```

(or run the equivalent `claude plugin marketplace add` / `claude plugin install` from the shell).

**Cursor:** `/add-plugin` pointing at this repo, or via Customize in the sidebar. Uses `.cursor-plugin/plugin.json`.

## Use

```text
/nalgene-mode the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

State the goal and a way to check it — don't hand-pick skills yourself. `nalgene-mode` routes to the right playbook and calls `openspec-gate` and the principles as the steps need them.

Mid-task, steer it by naming a principle directly: "apply prove-it-works, show me the real output."

## Guide

[`docs/guide/`](./docs/guide/README.md) walks through setup, routing, investigation, steering, and the gate with realistic prompts. [`docs/harness/`](./docs/harness/) covers the Claude Code vs. Cursor differences relevant to authoring or editing a skill.

## Status

v1: router + openspec gate + 5 principles + `how` + `why` + `teach` + `tech-advisory` + `plain-speech`, deliberately stack-agnostic except for `how`'s Odoo track and `teach`'s stack-overview step (`skills/teach/references/stack-overviews.md`, open-ended the same way `worktree-caches.md` is) (the first framework-specific explorer/explainer pair — others follow the same pattern: `references/explorer-prompt-<name>.md` + `references/explainer-prompt-<name>.md`, plus a routing line in `how`'s SKILL.md). `why` has no Odoo track yet — add a `references/sources/code-archaeology-odoo.md` if a real question ever needs layer-aware git history. `nalgene-mode`'s Odoo build-order support is a single reference (`skills/nalgene-mode/references/odoo-feature.md`, condensed from Odoo's own Server Framework 101 tutorial) that the `bug-fix`/`feature`/`refactor` playbooks point into, not a fourth playbook — framework knowledge, not a different workflow shape. `worktree-cleanup` is the actual fourth playbook, since disk-reclamation is a different workflow shape than a code change; its own per-ecosystem reference (`references/worktree-caches.md`) covers Odoo, Android, iOS, Node.js, and Golang cache and simulator cleanup. `tech-advisory` has its own, smaller playbook set (`landscape`, `known-issue`, `compare-options`) for picking research angles by what's driving the ask, independent of `nalgene-mode`'s own playbooks. Platform-specific *action* skills (Android, iOS, web/Svelte, backend feature-building, beyond exploration and worktree maintenance) are intentionally not in here yet — add them once a real, repeated need shows up.
