### Authoring or updating a skill

1. Call **create-a-skill** — it captures intent, places the skill in the right plugin, writes the SKILL.md in house voice, validates it against the Agent Skills spec, evaluates it, and (its own last step) opens the PR via **create-pr**.
2. Confirm that PR: check its URL, and that the validation and evaluation results it reported actually back what the PR body claims. Don't run **create-pr** again here — step 1 already did, and a second pass would try to commit and open a PR against a branch that's already clean and already has one open.

**Reply:** create-a-skill's summary, the validation and eval results, and the PR URL.
