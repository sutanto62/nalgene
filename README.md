# nalgene

Agent workflow plugins for Cursor and Claude Code. Each plugin is a standalone directory at the repository root with its own `.cursor-plugin/plugin.json` (and `plugin.json` for Claude Code).

## Plugins

| `name` | Plugin | Description |
|:-------|:-------|:-------------|
| `nalgene-mode` | [nalgene-mode](nalgene-mode/) | Minimal, stack-agnostic agent workflow: openspec-gated router, `how`/`why`/`teach`/`tech-advisory`, and a handful of no-fluff principles. |
| `odoo-mode` | [odoo-mode](odoo-mode/) | Odoo agent style layered on `nalgene-mode`: reuse Community/Enterprise before scaffolding anything new, routed through its playbooks. |
| `web-mode` | [web-mode](web-mode/) | Web/SPA agent style layered on `nalgene-mode`: reuse the app's shared API client and session wiring instead of duplicating it, routed through its playbooks. |

`nalgene-mode` is the stack-agnostic core every other plugin here composes with. A stack- or style-specific layer (Odoo, web, Android, iOS, backend, a personal workflow) ships as its own sibling plugin that routes through `nalgene-mode`'s playbooks and layers its own gate on top — `odoo-mode` and `web-mode` are the first two. See [`nalgene-mode`'s guide, page 2](nalgene-mode/docs/guide/02-nalgene-mode.md#sibling-plugins) for when a new stack warrants a sibling plugin instead of growing `nalgene-mode` itself.

## Install

**Claude Code:** this repo is its own marketplace (`.claude-plugin/marketplace.json`, one entry per plugin above).

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene-mode@nalgene
/plugin install odoo-mode@nalgene
/plugin install web-mode@nalgene
```

(or run the equivalent `claude plugin marketplace add` / `claude plugin install` from the shell). Install `nalgene-mode` alone if you don't need a sibling stack plugin yet; `odoo-mode` and `web-mode` each require `nalgene-mode` to also be installed.

**Cursor:** `/add-plugin` pointing at `nalgene-mode/`, `odoo-mode/`, or `web-mode/` in this repo, or via Customize in the sidebar. The root `.cursor-plugin/marketplace.json` lists all three.

## Use

```text
/nalgene-mode the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

State the goal and a way to check it — don't hand-pick skills yourself. `nalgene-mode` routes to the right playbook and calls `openspec-gate` and the principles as the steps need them. Working on an Odoo module with `odoo-mode` installed, or a frontend SPA with `web-mode` installed? Use `/odoo-mode` or `/web-mode` instead — each routes through the same playbooks with one extra gate on top.

Mid-task, steer any of the three by naming a principle directly: "apply prove-it-works, show me the real output."

## Guide

[`nalgene-mode/docs/guide/`](nalgene-mode/docs/guide/README.md) walks through setup, routing, investigation, steering, and the gate with realistic prompts. [`nalgene-mode/docs/harness/`](nalgene-mode/docs/harness/) covers the Claude Code vs. Cursor differences relevant to authoring or editing a skill. `odoo-mode` and `web-mode` are each small enough to cover in their own README alone ([odoo-mode](odoo-mode/README.md), [web-mode](web-mode/README.md)).

## Adding a new plugin

Add a new top-level directory with its own `.cursor-plugin/plugin.json`, a Claude Code `plugin.json`, and a `README.md`, then list it in both root marketplace files (`.claude-plugin/marketplace.json`, `.cursor-plugin/marketplace.json`). If it's a stack- or style-specific layer on `nalgene-mode`, model it on `odoo-mode` or `web-mode`: a thin router skill that composes with `nalgene-mode`'s playbooks instead of restating them, plus whatever principle(s) that stack needs.
