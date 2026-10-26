# Source playbooks

`why` spawns one investigator per source in its roster, each reading the source-specific playbook below plus the cross-cutting one when it applies.

| Source | Playbook | Always available? |
|---|---|---|
| Source control | [`code-archaeology.md`](./sources/code-archaeology.md) | Yes — git + `gh` |
| Openspec documents | [`openspec.md`](./sources/openspec.md) | Only if the project has an `openspec/` directory |

Cross-cutting:

- [`incident-postmortem.md`](./sources/incident-postmortem.md). Add to the relevant investigator(s) if the target code looks defensive (null checks, retry, timeout, rate limit, feature flag, egress guard, OOM handler).

## Adding a source

This plugin deliberately ships with two. If a project adds a real third source (an issue tracker, chat export, observability tool — anything with its own query surface), write a new `sources/<name>.md` modeled on `code-archaeology.md`: what the source contains, concrete query/command examples, and what it's uniquely good at surfacing. Add a row here and a line in `why`'s SKILL.md Step 4 roster. The investigator/synthesizer prompts don't need to change — they already treat the roster as a list.
