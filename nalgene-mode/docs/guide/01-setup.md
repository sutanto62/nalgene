# 1. Set up nalgene-mode

## Install

**Claude Code** — the parent repo is its own marketplace, listing this plugin alongside its siblings `odoo-mode` and `web-mode`:

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene-mode@nalgene
```

Or the shell equivalent: `claude plugin marketplace add sutanto62/nalgene` / `claude plugin install nalgene-mode@nalgene`.

**Cursor** — `/add-plugin` pointing at this plugin's own directory (`nalgene-mode/` in the parent repo), or add it via Customize in the sidebar. Cursor reads this directory's `.cursor-plugin/plugin.json`; Claude Code reads the parent repo's `.claude-plugin/marketplace.json`. Both point at the same skill files — nothing under `skills/` is duplicated per host.

## What you get

- `nalgene-mode` — the router. Start here for any real task.
- `how` / `why` — read-only investigation.
- `openspec-gate` / `openspec-explore` — the spec gate and its read-only lookup.
- `nalgene-help` — "which skill for X", no work done.
- Four `principle-*` skills you invoke by name mid-task.
- `agents/nalgene-explorer` — the bundled read-only subagent `how` and `why` share.

No model picker and no rules file to write — nalgene-mode doesn't route by model yet. If that changes, this page is where it'll be documented.

## Odoo projects

`how`'s explorer/explainer pair detects an Odoo project automatically from the touched paths (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`) or from the question naming Odoo — nothing to install for that. The reuse-and-extend gate and build order for actually *building* an Odoo feature live in the sibling `odoo-mode` plugin instead; install it and use `/odoo-mode` for that work.

Next: [Route work through `/nalgene-mode`](./02-nalgene-mode.md).
