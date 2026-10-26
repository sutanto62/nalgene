# web-mode

Web/SPA agent style layered on [nalgene-mode](../nalgene-mode/). nalgene-mode does the shared work: router, openspec gate, core principles, playbook machinery. This plugin adds the one gate this stack needs — reuse the app's shared API client and session wiring instead of duplicating it — and the build order for a feature in that shape.

## Install

**Claude Code:**

```text
/plugin marketplace add sutanto62/nalgene
/plugin install nalgene-mode@nalgene
/plugin install web-mode@nalgene
```

**Cursor:** `/add-plugin` pointing at `web-mode/` in this repo, or via Customize in the sidebar. Uses `.cursor-plugin/plugin.json`.

This plugin requires `nalgene-mode` to be installed alongside it — its `web-mode` skill routes through `nalgene-mode`'s own playbooks rather than restating them.

## Skills

| Skill | Use it when |
|---|---|
| [`/web-mode`](./skills/web-mode/SKILL.md) | Default entry for nontrivial work on a frontend SPA with a generated or shared API client. Routes through `nalgene-mode`'s playbooks with the web gate on top. |
| [`principle-spa-shared-client`](./skills/principle-spa-shared-client/SKILL.md) | Referenced by `web-mode`. Reuse the app's API client and session wiring instead of another client instantiation or storage-based token read. |

## What this plugin's web-mode skill adds over nalgene-mode

- **One extra gate.** Before a feature's data-fetching code, or anything auth/session-related, check for a shared client module and the app's single session store before adding another copy of either.
- **A build order for this shape.** `skills/web-mode/references/web-feature.md` sequences a new SPA feature (shared-client check → place the feature → confirm the contract → split data from presentation → ...), opened by the gate above. Bug fixes and refactors read the same reference to locate the likely layer or justify consolidating the client/token handling.

## Not shipped here

- Principles, playbooks, the openspec gate, `how`/`why`/`teach`/`tech-advisory`, and the todo-list convention ship in `nalgene-mode`.

## License

MIT
