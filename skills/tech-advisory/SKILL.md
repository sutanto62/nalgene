---
name: tech-advisory
description: "Give an outside, no-judgment perspective on a technology, library, stack, design pattern, algorithm, or AI model: recent news, known issues, notable alternatives, and community sentiment, via live web research. Routes the ask — a general goal, an issue already hit, or a decision between options — into a playbook that picks the research angles. Never recommends adopting or replacing anything — presents the landscape with dates and sources, and flags where this project's own scale or constraints might not fit. Use for 'what's new in X', 'any known issues with Y', 'what's the broader landscape for Z', or 'what are people doing differently for this kind of problem'."
disable-model-invocation: true
---

# Tech advisory

Give a broader, outside point of view on a piece of technology — recent news, known issues, notable alternatives, community sentiment — the way an advisor would, not a recommendation engine. The person decides what to do with it. This skill's job stops at presenting the landscape accurately, dated and sourced, and naming where this project's own scale or constraints might make something that looks appealing in the abstract not actually fit.

This isn't `how` or `why` pointed at the internet instead of the codebase — it researches the outside world, not this repository. It doesn't conclude anything, the same way `why` never concludes a motivation it can't cite: no "you should switch", no "the better choice is", no ranking a winner. Research and report, nothing else. Invoked on its own — `teach` stays a pure explanation of what exists, this is a separate ask about what else is out there.

Every reply also runs through `plain-speech` (step 7), the same pass `teach` uses — AI-tell removal plus a vocabulary and sentence-structure simplification — since a landscape report only does its job if whoever asked can actually read it. Unlike `teach`, this skill has no step that profiles the reader, so plain-speech applies its own default (a reader with no technical background) unless the conversation already names a different audience.

Every investigator and the synthesizer spawn as `subagent_type: nalgene-explorer` (read-only) — same discipline as `how`/`why`. This skill only gathers and reports; it never writes code or files, so a writing-capable agent is never the right choice here.

## Playbooks

- A general goal — "what's new with X", "what's the landscape for Y" — no specific pain point yet → `playbooks/landscape.md`.
- An issue you've actually hit — something broken, confusing, or limiting — and the question is whether it's known or documented → `playbooks/known-issue.md`.
- A problem you're solving by deciding between options — "what else is out there", "how do other teams solve this" → `playbooks/compare-options.md`.
- None of these fit → default to `playbooks/landscape.md` and say so.

## Step 1. Understand the goal, issue, or problem

Restate what's driving the ask in one sentence — the same way `nalgene-mode` restates a goal before picking its own playbook. The topic is usually a technology, library, framework, design pattern, algorithm, or AI model/technique, named directly or implied by what's being discussed. If vague, make your best guess from conversation context and state it so the person can redirect.

If the question embeds an assumption ("should we move to X, I assume it's faster?"), treat the assumption as one thing to check, not something to confirm. See the hype trap in `references/epistemics.md`.

## Step 2. Pick the playbook

Match against the table above. Say which one and why. Each playbook names its own angle set, in priority order — this replaces a single fixed roster, since a known-issue question needs a different mix of research than a landscape question does.

## Step 3. Ground it in the project, lightly, if relevant

If the topic concerns something this project actually uses or could use, do a lightweight check of the project's language/framework — the same marker approach `teach` uses (see `../teach/references/stack-overviews.md` for the marker list; don't duplicate it here) — plus any obvious scale signals (repo size, deployment config, team size if evident from CODEOWNERS or git history). This grounds the scale-fit section in Step 6 in something real instead of a generic "it depends." Skip this entirely for a topic with no bearing on this specific project (a general "what's new in X" question with no project tie-in).

## Step 4. Research

Spawn the angles the matched playbook names, each `nalgene-explorer` with `references/investigator-prompt.md` filled in plus the angle's own playbook from `references/angles/` (indexed in `references/angle-playbook.md`).

**Cap at 3 concurrent investigators, queued.** Spawn the first 3 angles from the playbook's priority order in a single batch. If the playbook names more than 3 (only `compare-options` does today), the rest wait in a queue — as soon as a running investigator returns, pull the next queued angle and spawn it, keeping at most 3 investigators in flight at any time. Never spawn more than 3 investigator agents at once, no matter how many angles a playbook — current or future — names.

**The synthesizer always runs last.** Don't spawn it until every investigator has returned, including every angle that spent time in the queue. Synthesizing against partial results produces a landscape report with holes the person can't see were ever there.

Each investigator gathers evidence and reports it — it does not conclude, recommend, or rank. See the prompt template for the exact discipline.

## Step 5. Synthesize

Spawn one `nalgene-explorer` with `references/synthesizer-prompt.md` filled in, plus:

1. The investigator findings
2. The project context from Step 3, if gathered
3. The user's original question
4. `references/epistemics.md` in full

## Step 6. Present

Present the synthesizer's output. Light edits for clarity are fine. Don't rewrite the confidence language, and don't quietly turn a "some teams report" into a recommendation — the hedge is the point, same as it is in `why`.

## Step 7. Write it through plain-speech

Before showing the synthesizer's output, run it through the **plain-speech** skill in full — not just the spirit of it, read its `SKILL.md` and apply both passes: strip AI-sounding tells, then simplify vocabulary and sentence structure. Every URL, date, version number, and source name stays exact — plain-speech only touches prose, and its own verify step already checks that nothing factual drifted.

Don't let simplification blur the epistemic tiers `references/epistemics.md` requires. "Documented" still needs to read as more certain than "Reported sentiment" after the rewrite — plain-speech's job is clearer words, not flatter confidence.

## Output format

The structure in `references/synthesizer-prompt.md`: The Question, What's Established, What's Emerging or Contested, Known Issues, Alternatives Worth Knowing About (if researched), Where This Project's Scale or Constraints Might Not Fit, Sources Consulted, Confidence Summary. Adapt as needed; drop sections with nothing to say. Run through `plain-speech` (step 7) before being shown.

## Common failure modes to avoid

- **Hype bias.** Trending in search results or social sentiment isn't evidence of merit. Report the trend as a trend.
- **Staleness.** "Recent" expires fast. Date everything; don't let a two-year-old post pass as current news.
- **Forcing fit.** The most interesting finding is often the one that doesn't apply here — say so explicitly rather than letting an exciting result imply it's actionable for this project.
- **Collapsing sentiment into fact.** "People are moving to X" is a claim about sentiment, not a technical fact about X. Keep them in separate sections.
- **Sliding into a recommendation.** Even a hedged one ("you'd probably benefit from...") crosses the line this skill exists not to cross. Describe tradeoffs; don't resolve them for the person.
- **Spawning every angle at once.** A playbook naming more than 3 angles still caps at 3 concurrent investigators — queue the rest, don't spawn them all because they'll "finish eventually anyway."

## Adding a new angle

Add a `references/angles/<name>.md` playbook adapted from `references/angles/official-sources.md`, add a row to `references/angle-playbook.md`, and add it to whichever goal/issue/problem playbook(s) it fits, in priority order. The investigator/synthesizer loop and the cap-and-queue in Step 4 don't change — they already treat a playbook's angle list as open-ended.

## Adding a new playbook

A playbook is one file, `playbooks/<name>.md`: the goal/issue/problem shape it matches, and its angle set in priority order (the order angles get queued in if the set exceeds 3). Add a routing line to the Playbooks table above. A set of 3 or fewer angles never triggers the queue; nothing else needs to change — Step 4's cap-and-queue logic already treats every playbook's angle list the same way.
