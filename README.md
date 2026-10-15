# nalgene

A minimal, stack-agnostic agent workflow plugin for Cursor and Claude Code. Inspired by [pstack](https://github.com/cursor/plugins/tree/main/pstack), scoped down to one router, one gate, and four principles — no platform-specific skills yet.

## What's here

- **`nalgene-mode`** — the router. Give it a goal and a way to check it's done; it picks a playbook (`bug-fix`, `feature`, `refactor`), gates non-trivial changes through `openspec-gate`, and reports what it skipped and why.
- **`openspec-gate`** — drives `openspec`'s propose → validate → implement → archive loop. Called by `nalgene-mode` for any change that touches behavior, an API, or more than one module; invoke directly for anything else.
- **`nalgene-help`** — answers "which skill for X" without spending tokens doing work. Type `/nalgene-help <question>`.
- **`principle-*`** — one-rule skills you invoke by name mid-task to redirect the agent: `fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`.

## Install

**Claude Code:** add this directory as a plugin source (see Claude Code's plugin docs for the exact marketplace/local-path mechanism your version uses).

**Cursor:** `/add-plugin` pointing at this directory, or via Customize in the sidebar. Uses `.cursor-plugin/plugin.json`.

## Use

```text
/nalgene-mode the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

State the goal and a way to check it — don't hand-pick skills yourself. `nalgene-mode` routes to the right playbook and calls `openspec-gate` and the principles as the steps need them.

Mid-task, steer it by naming a principle directly: "apply prove-it-works, show me the real output."

## Status

v1: router + openspec gate + 4 principles, deliberately stack-agnostic. Platform skills (Android, iOS, web/Svelte, backend, Odoo) are intentionally not in here yet — add them as their own `skills/<name>/SKILL.md` once a real, repeated need shows up, rather than stubbing them in advance.
