# nalgene

A minimal, stack-agnostic agent workflow plugin for Cursor and Claude Code. Inspired by [pstack](https://github.com/cursor/plugins/tree/main/pstack), scoped down to one router, one gate, and four principles — no platform-specific skills yet.

## What's here

- **`nalgene-mode`** — the router. Give it a goal and a way to check it's done; it picks a playbook (`bug-fix`, `feature`, `refactor`), gates non-trivial changes through `openspec-gate`, and reports what it skipped and why.
- **`how`** — "how does X work" / architecture walkthroughs. Checks `openspec-explore` first, routes to a general or framework-specific (Odoo) explorer/explainer track, and fans out read-only `nalgene-explorer` subagents for complex questions.
- **`openspec-gate`** — drives `openspec`'s propose → validate → implement → archive loop. Called by `nalgene-mode` for any change that touches behavior, an API, or more than one module; invoke directly for anything else.
- **`openspec-explore`** — read-only: what does openspec already document about this? Used by `how`, or standalone.
- **`nalgene-help`** — answers "which skill for X" without spending tokens doing work. Type `/nalgene-help <question>`.
- **`principle-*`** — one-rule skills you invoke by name mid-task to redirect the agent: `fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`.
- **`agents/nalgene-explorer`** — the plugin's own read-only, general-purpose subagent. Bundled rather than relying on a host's built-in equivalent, so `how` works the same in Cursor and Claude Code.

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

## Status

v1: router + openspec gate + 4 principles + `how`, deliberately stack-agnostic except for `how`'s Odoo track (the first framework-specific explorer/explainer pair — others follow the same pattern: `references/explorer-prompt-<name>.md` + `references/explainer-prompt-<name>.md`, plus a routing line in `how`'s SKILL.md). Platform-specific *action* skills (Android, iOS, web/Svelte, backend) beyond exploration are intentionally not in here yet — add them once a real, repeated need shows up.
