# 1. Set up nalgene

## Install

**Claude Code** — this repo is its own marketplace:

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene@nalgene
```

Or the shell equivalent: `claude plugin marketplace add sutanto62/nalgene` / `claude plugin install nalgene@nalgene`.

**Cursor** — `/add-plugin` pointing at this repo, or add it via Customize in the sidebar. Cursor reads `.cursor-plugin/plugin.json`; Claude Code reads `.claude-plugin/marketplace.json`. Both point at the same skill files — nothing under `skills/` is duplicated per host.

## What you get

- `nalgene-mode` — the router. Start here for any real task.
- `how` / `why` — read-only investigation.
- `openspec-gate` / `openspec-explore` — the spec gate and its read-only lookup.
- `nalgene-help` — "which skill for X", no work done.
- Four `principle-*` skills you invoke by name mid-task.
- `agents/nalgene-explorer` — the bundled read-only subagent `how` and `why` share.

No model picker and no rules file to write — nalgene doesn't route by model yet. If that changes, this page is where it'll be documented.

## Odoo projects

Nothing to install separately. `nalgene-mode`'s playbooks and `how`'s explorer/explainer pair both detect an Odoo project from the touched paths (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) or from the question naming Odoo, and switch tracks automatically.

Next: [Route work through `/nalgene-mode`](./02-nalgene-mode.md).
