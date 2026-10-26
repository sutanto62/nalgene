### Authoring or updating a skill

**openspec-gate** doesn't apply — this repo has no `openspec/` of its own to spec against. `openspec-gate` is a skill nalgene offers to target projects, not one it runs on itself.

1. Call **create-a-skill** — it captures intent, places the skill in the right plugin, writes the SKILL.md in house voice, validates it against the Agent Skills spec, evaluates it, and registers it for discovery. It stops there; opening the PR isn't its job.
2. Once that's clean, open the PR per **create-pr** — the validation and eval results from step 1 are the `## Verification` section.

**Reply:** create-a-skill's summary, the validation and eval results, and the PR URL.
