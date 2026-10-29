# nalgene in Claude Code

## Subagents

Claude Code's `Agent` tool takes `subagent_type`, `description`, `prompt`, `model`, and `isolation` — no call-time `readonly` or `tools` parameter. Tool access is fixed entirely by the invoked agent's static definition file (the `tools:` frontmatter line), not by anything passed at call time.

This is why `agents/nalgene-explorer.md` exists as its own bundled file with `tools: Read, Grep, Glob, Bash, WebFetch, WebSearch` in its frontmatter, instead of `how`/`why` calling a generic agent with a call-time `readonly: true` flag the way Cursor's `Task` tool supports. A call-time flag won't restrict tools on this host; it's silently ignored.

Adding a new subagent profile to nalgene (a write-capable implementer, say) means adding a new file under `agents/`, not a new flag at the call site. See [`../harness/cursor.md`](./cursor.md) for the Cursor side of this same split.

## Tools

| nalgene skill text | Claude Code |
|---|---|
| "spawn a `nalgene-explorer`" | `Agent` tool, `subagent_type: "nalgene-explorer"` |
| `openspec` commands (`openspec list`, `openspec new change`, ...) | `Bash` |
| `git`/`gh` commands in `why`'s code-anchor step | `Bash` |
| `git`/`gh` commands in **create-pr** | `Bash` |
| "open a todo list" with the playbook's steps | `TaskCreate`/`TaskUpdate` — one task per playbook step, `in_progress` → `completed`; a skipped step stays `completed` with its reason in the description instead of being deleted |

## Skills

`/nalgene-mode`, `/how`, `/why`, and the rest are invoked with the `Skill` tool using the plugin-qualified name (`nalgene-mode:nalgene-mode` — the plugin is also named `nalgene-mode`, same as its own router skill, so the qualified form repeats the word) when another installed plugin defines a same-named skill, or the bare name otherwise. `disable-model-invocation: true` on every nalgene-mode skill means none of these trigger automatically on their own — only an explicit `/name` or a direct ask routes to one.

A running skill's own steps naming a sibling skill (`nalgene-mode` step 3 saying to run `openspec-gate`, say) can't reach it through the `Skill` tool either: the tool only accepts a name from the model-facing skill listing or one the user typed as `/name`, and `disable-model-invocation: true` keeps a skill out of that listing — so a sibling named only in another skill's prose is an invalid `Skill` call, not a trigger. `Read` the sibling's `SKILL.md` off disk and follow it directly instead; `nalgene-mode`'s own Principles section and `teach`'s `plain-speech` step already do this. Cursor doesn't have this restriction — see [`../harness/cursor.md`](./cursor.md).
