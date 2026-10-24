# nalgene in Cursor

## Subagents

Cursor's `Task` tool accepts `readonly: true`, `is_background: true`, and a `model` override inline on any call, even against the generic built-in `generalPurpose` subagent — no dedicated agent file needed to get a read-only delegate.

nalgene still bundles `agents/nalgene-explorer.md` rather than relying on an inline `readonly: true` call, so the same skill text (`how`, `why`) works unchanged on both hosts. See [`../harness/claude-code.md`](./claude-code.md) for why the inline flag alone wouldn't be enough there — the bundled file is the lowest common denominator, not a Cursor-specific workaround.

## Tools

| nalgene skill text | Cursor |
|---|---|
| "spawn a `nalgene-explorer`" | `Task` tool, `subagent_type: "nalgene-explorer"`, `readonly: true` |
| `openspec`/`git`/`gh` commands | shell execution |
| "open a todo list" with the playbook's steps | Cursor's native todo list, same as pstack's `poteto-mode` uses |

## Skills

Cursor skills are invoked with `/name` the same way as Claude Code. nalgene doesn't currently use any Cursor-only feature (Automations, Bugbot, cloud agents) the way pstack does, so there's nothing host-specific to skip when following a skill's steps here.
