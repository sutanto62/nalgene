### Authoring or updating a skill

1. Call **create-a-skill** — it captures intent, places the skill in the right plugin, writes the SKILL.md in house voice, validates it against the Agent Skills spec, and evaluates it. It stops there; opening the PR isn't its job.
2. Once that's clean, open the PR per **create-pr**. Give it a conventional-commit message and a PR body stating what the skill does plus the validation and eval results from step 1.

**Reply:** create-a-skill's summary, the validation and eval results, and the PR URL.
