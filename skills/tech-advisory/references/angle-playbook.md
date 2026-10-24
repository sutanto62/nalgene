# Angle playbooks

`tech-advisory` spawns one investigator per angle in its roster, each reading the angle-specific playbook below.

| Angle | Playbook | Always spawned? |
|---|---|---|
| Official sources | [`official-sources.md`](./angles/official-sources.md) | Yes |
| Known issues | [`known-issues.md`](./angles/known-issues.md) | Yes |
| Community sentiment | [`community-sentiment.md`](./angles/community-sentiment.md) | Yes |
| Alternatives | [`alternatives.md`](./angles/alternatives.md) | Only for a comparative question |

## Adding an angle

Write a new `angles/<name>.md` modeled on `official-sources.md`: what the angle covers, concrete search strategies and sites to check, and what it's uniquely good at surfacing. Add a row here and a line in `tech-advisory`'s SKILL.md Step 3 roster. The investigator/synthesizer prompts don't need to change — they already treat the roster as a list.
