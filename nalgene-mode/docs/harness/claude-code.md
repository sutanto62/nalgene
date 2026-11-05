# nalgene in Claude Code

Read this file once per session, before running any nalgene-family skill — `nalgene-mode` itself or a sibling plugin's router (`odoo-mode`, `web-mode`, `backend-mode`, or a later one). Whenever a skill's own steps name another skill, in the same plugin or a different one, translate that reference with the **Skills** section below instead of guessing how to invoke it.

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
| "isolate in a worktree first" (Non-negotiables) | `EnterWorktree` tool. Skip it — and say so — when the session is already isolated: a background job under `.claude/worktrees/`, or `cwd` already there. |

## Skills

`/nalgene-mode`, `/how`, `/why`, and the rest are invoked with the `Skill` tool using the plugin-qualified name (`nalgene-mode:nalgene-mode` — the plugin is also named `nalgene-mode`, same as its own router skill, so the qualified form repeats the word) when another installed plugin defines a same-named skill, or the bare name otherwise. `disable-model-invocation: true` on every nalgene-mode skill means none of these trigger automatically on their own — only an explicit `/name` or a direct ask routes to one.

A running skill's own steps naming another skill can't reach it through the `Skill` tool either — whether that's `nalgene-mode` step 3 saying to run `openspec-gate`, or a sibling plugin's router (`odoo-mode`, `web-mode`, `backend-mode`) saying to follow `nalgene-mode`'s playbook catalog or Principles section. The tool only accepts a name from the model-facing skill listing or one the user typed as `/name`, and `disable-model-invocation: true` keeps a skill out of that listing — so a skill named only in another skill's prose is an invalid `Skill` call, not a trigger, no matter which plugin either one lives in. `Read` the named skill's `SKILL.md` (or the specific playbook/reference file called out) off disk and follow it directly instead; `nalgene-mode`'s own Principles section and `teach`'s `plain-speech` step already do this, and every sibling plugin's router does the same for its references into `nalgene-mode`. The only two ways to reach a nalgene-family skill through the `Skill` tool itself are the user typing `/name` directly, or asking for it by name in a way the harness resolves to that command — everything else, including one playbook or router pointing at another, is a disk read, never a tool call. Cursor doesn't have this restriction — see [`../harness/cursor.md`](./cursor.md).
