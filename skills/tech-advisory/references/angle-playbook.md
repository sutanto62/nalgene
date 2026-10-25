# Angle playbooks

`tech-advisory` spawns one investigator per angle named by the matched goal/issue/problem playbook (`../playbooks/<name>.md`), each investigator reading the angle-specific playbook below. Which angles run, and in what priority, is the playbook's call, not a fixed roster — see `../playbooks/` for the three today.

| Angle | Playbook | Used by |
|---|---|---|
| Official sources | [`official-sources.md`](./angles/official-sources.md) | `landscape`, `known-issue`, `compare-options` |
| Known issues | [`known-issues.md`](./angles/known-issues.md) | `landscape`, `known-issue`, `compare-options` |
| Community sentiment | [`community-sentiment.md`](./angles/community-sentiment.md) | `landscape`, `known-issue`, `compare-options` |
| Alternatives | [`alternatives.md`](./angles/alternatives.md) | `compare-options` |

## Adding an angle

Write a new `angles/<name>.md` modeled on `official-sources.md`: what the angle covers, concrete search strategies and sites to check, and what it's uniquely good at surfacing. Add a row here, and add it to whichever playbook(s) under `../playbooks/` it fits, in priority order. The investigator/synthesizer prompts and the cap-and-queue logic in `tech-advisory`'s SKILL.md Step 4 don't need to change — they already treat a playbook's angle list as open-ended.
