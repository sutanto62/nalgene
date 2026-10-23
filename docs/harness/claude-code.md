# nalgene in Claude Code

## Subagents

Claude Code's `Agent` tool takes `subagent_type`, `description`, `prompt`, `model`, and `isolation` — no call-time `readonly` or `tools` parameter. Tool access is fixed entirely by the invoked agent's static definition file (the `tools:` frontmatter line), not by anything passed at call time.

This is why `agents/nalgene-explorer.md` exists as its own bundled file with `tools: Read, Grep, Glob, Bash, WebFetch, WebSearch` in its frontmatter, instead of `how`/`why` calling a generic agent with a `readonly: true` flag the way pstack — the plugin nalgene is adapted from — does under Cursor. A call-time flag won't restrict tools on this host; it's silently ignored.

Adding a new subagent profile to nalgene (a write-capable implementer, say) means adding a new file under `agents/`, not a new flag at the call site. See [`../harness/cursor.md`](./cursor.md) for the Cursor side of this same split.

## Tools

| nalgene skill text | Claude Code |
|---|---|
| "spawn a `nalgene-explorer`" | `Agent` tool, `subagent_type: "nalgene-explorer"` |
| `openspec` commands (`openspec list`, `openspec new change`, ...) | `Bash` |
| `git`/`gh` commands in `why`'s code-anchor step | `Bash` |

## Skills

`/nalgene-mode`, `/how`, `/why`, and the rest are invoked with the `Skill` tool using the plugin-qualified name (`nalgene:nalgene-mode`) when another installed plugin defines a same-named skill, or the bare name otherwise. `disable-model-invocation: true` on every nalgene skill means none of these trigger automatically on their own — only an explicit `/name` or a direct ask routes to one.
