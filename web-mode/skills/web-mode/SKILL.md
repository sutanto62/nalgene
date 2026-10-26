---
name: web-mode
description: "Web/SPA agent style layered on nalgene-mode: reuse the app's shared API client and session wiring instead of duplicating it, routed through nalgene-mode's playbooks. Use for /web-mode, or nontrivial work on a frontend SPA with a generated or shared API client."
disable-model-invocation: true
---

# Web mode

Thin router over the `nalgene-mode` plugin's own `nalgene-mode` skill. Shared principles and playbook machinery live there — read them from it, don't restate them here.

**Requires** the `nalgene-mode` plugin. Check first: the `nalgene-mode` skill should be in your skill list. Missing → stop and tell the user to install it (`/plugin install nalgene-mode@nalgene` in Claude Code, `/add-plugin nalgene-mode` in Cursor).

## Sibling skills (this plugin)

| Skill | Job |
|---|---|
| `web-mode` | This router |
| `principle-spa-shared-client` | The web gate below |

## Shared machinery (do not restate)

Read these from the `nalgene-mode` plugin. Do not copy their contents into this file.

| Layer | Where |
|---|---|
| Shared mode (Principles index, non-negotiables, playbook catalog, todo-list convention) | `nalgene-mode`'s own `nalgene-mode` skill |
| Shared playbooks | `nalgene-mode/playbooks/<name>.md` |
| General principle leaves (`fix-root-causes`, `prove-it-works`, `minimize-reader-load`, `outcome-oriented-execution`, `report-dont-recommend`) | `nalgene-mode`'s `principle-*` skills |
| openspec gate | `nalgene-mode`'s `openspec-gate` skill |
| "How does X work" before changing something | `nalgene-mode`'s `how` skill |

## Non-negotiables

1. Open a todo list. Item 1: read `nalgene-mode`'s **Principles** section in full. Cite each principle you apply with the concrete choice it changed.
2. Match a `nalgene-mode` playbook (`feature`, `bug-fix`, `refactor`, `worktree-cleanup`). Copy its steps into the todo list before the web gate below. A skipped step stays with a one-line reason instead of being dropped.
3. Apply `nalgene-mode`'s own non-negotiables (openspec-gate before code on anything touching behavior or a public interface, **how** before a nontrivial change). Do not re-list them here.
4. Apply the **web gate** below wherever the playbook's own step would otherwise have you write a feature's data-fetching code, or touch anything auth/session-related, from scratch. It only fires when the project is in scope — see Detection.

### Web gate

- **Shared API client.** Apply **principle-spa-shared-client** (leaf, this plugin): reuse the app's existing client and session wiring instead of instantiating another one or reading the session token from a new place.
- In a matched **Feature** or **Bug fix** playbook, this gate opens the build order: follow `references/web-feature.md` instead of improvising one — the same way the sibling `odoo-mode` plugin's build order opens with `principle-odoo-reuse-extend`. In a **Refactor** consolidating the client or token handling, read the principle first; that kind of duplication is usually structural across the app, not a one-off to spot-fix.

### Detection

"SPA project" means the touched paths are inside a frontend app directory built on a SPA framework (package.json naming React/Vue/Angular/Svelte or similar) with a typed or generated client layer over a backend API. Not a SPA project → this plugin doesn't apply; run `nalgene-mode` directly instead of `/web-mode`.

## Report

Same as the matched `nalgene-mode` playbook's own report line, plus which client/session wiring was reused or, if none existed yet, added.
