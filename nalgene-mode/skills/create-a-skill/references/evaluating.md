# Evaluating a new skill (lightweight)

Full method: https://agentskills.io/skill-creation/evaluating-skills. This is the scaled-down version for skills this size — most are a few dozen lines of prose, not a file-producing tool. Reach for the full eval-set / benchmark.json / assertion-grading machinery only when the user explicitly wants that rigor: a skill with high variance, one other teams will depend on, or one producing an artifact worth checking programmatically.

## When to bother

Skip entirely for a principle skill — it's judged on whether it steers a conversation the right way, not on an artifact — or a one-shot voice/formatting skill with no objectively checkable output. Run the loop below for a skill with fixed steps that should produce the same shape of result every time: a scaffold, a gate, a validation pass, anything with a checkable "did it do the thing."

## The loop

1. Write 2-3 realistic prompts — the phrasing someone would actually type, not an abstract description of the task. Include at least one edge case or ambiguous phrasing.
2. Run each prompt twice: once with the new skill available, once without it (or against the previous version, if editing an existing skill). Use a subagent per run so neither sees the other's context.
3. Compare the two runs: did the with-skill run follow the steps you wrote, produce the output shape you specified, and skip what it should skip? Note concrete misses — "skipped the name/directory match check" beats "wasn't great."
4. When a run goes off the rails, fix the skill, not the test. A missed step usually means the instruction was ambiguous or missing its why, not that the model needs a harder MUST.
5. Stop once a clean run matches intent, or another pass isn't teaching you anything new.

No JSON eval files, no pass/fail assertion grading, no benchmark aggregation here — that's the full agentskills.io loop's territory, and it's overkill at this scale. If a future skill genuinely needs that level of rigor, read the full page and follow it as written rather than improvising a partial version of it.
