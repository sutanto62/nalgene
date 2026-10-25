---
name: tech-advisory
description: "Give an outside, no-judgment perspective on a technology, library, stack, design pattern, algorithm, or AI model: recent news, known issues, notable alternatives, and community sentiment, via live web research. Never recommends adopting or replacing anything — presents the landscape with dates and sources, and flags where this project's own scale or constraints might not fit. Use for 'what's new in X', 'any known issues with Y', 'what's the broader landscape for Z', or 'what are people doing differently for this kind of problem'."
disable-model-invocation: true
---

# Tech advisory

Give a broader, outside point of view on a piece of technology — recent news, known issues, notable alternatives, community sentiment — the way an advisor would, not a recommendation engine. The person decides what to do with it. This skill's job stops at presenting the landscape accurately, dated and sourced, and naming where this project's own scale or constraints might make something that looks appealing in the abstract not actually fit.

This isn't `how` or `why` pointed at the internet instead of the codebase — it researches the outside world, not this repository. It doesn't conclude anything, the same way `why` never concludes a motivation it can't cite: no "you should switch", no "the better choice is", no ranking a winner. Research and report, nothing else. Invoked on its own — `teach` stays a pure explanation of what exists, this is a separate ask about what else is out there.

Every reply also runs through `plain-speech` (step 6), the same pass `teach` uses — AI-tell removal plus a vocabulary and sentence-structure simplification — since a landscape report only does its job if whoever asked can actually read it. Unlike `teach`, this skill has no step that profiles the reader, so plain-speech applies its own default (a reader with no technical background) unless the conversation already names a different audience.

Every investigator and the synthesizer spawn as `subagent_type: nalgene-explorer` (read-only) — same discipline as `how`/`why`. This skill only gathers and reports; it never writes code or files, so a writing-capable agent is never the right choice here.

## Step 1. Understand the topic

The topic is usually a technology, library, framework, design pattern, algorithm, or AI model/technique, named directly or implied by what's being discussed. If vague, make your best guess from conversation context and state it so the person can redirect.

If the question embeds an assumption ("should we move to X, I assume it's faster?"), treat the assumption as one thing to check, not something to confirm. See the hype trap in `references/epistemics.md`.

## Step 2. Ground it in the project, lightly, if relevant

If the topic concerns something this project actually uses or could use, do a lightweight check of the project's language/framework — the same marker approach `teach` uses (see `../teach/references/stack-overviews.md` for the marker list; don't duplicate it here) — plus any obvious scale signals (repo size, deployment config, team size if evident from CODEOWNERS or git history). This grounds the scale-fit section in Step 5 in something real instead of a generic "it depends." Skip this entirely for a topic with no bearing on this specific project (a general "what's new in X" question with no project tie-in).

## Step 3. Research

Spawn investigators in a single batch so they run concurrently, each `nalgene-explorer` with `references/investigator-prompt.md` filled in, plus one angle from `references/angle-playbook.md`:

- **Official sources** (always): `references/angles/official-sources.md` — release notes, changelogs, official docs, standards bodies, maintainer statements.
- **Known issues** (always): `references/angles/known-issues.md` — security advisories, deprecations, open issues with real traction, documented footguns.
- **Community sentiment** (always): `references/angles/community-sentiment.md` — what practitioners are actually saying, explicitly labeled as sentiment, not fact.
- **Alternatives** (only for a comparative question — "what else is out there for X", "how are other teams solving this"): `references/angles/alternatives.md` — competing or adjacent approaches, same no-ranking framing.

Each investigator gathers evidence and reports it — it does not conclude, recommend, or rank. See the prompt template for the exact discipline.

## Step 4. Synthesize

Spawn one `nalgene-explorer` with `references/synthesizer-prompt.md` filled in, plus:

1. The investigator findings
2. The project context from Step 2, if gathered
3. The user's original question
4. `references/epistemics.md` in full

## Step 5. Present

Present the synthesizer's output. Light edits for clarity are fine. Don't rewrite the confidence language, and don't quietly turn a "some teams report" into a recommendation — the hedge is the point, same as it is in `why`.

## Step 6. Write it through plain-speech

Before showing the synthesizer's output, run it through the **plain-speech** skill in full — not just the spirit of it, read its `SKILL.md` and apply both passes: strip AI-sounding tells, then simplify vocabulary and sentence structure. Every URL, date, version number, and source name stays exact — plain-speech only touches prose, and its own verify step already checks that nothing factual drifted.

Don't let simplification blur the epistemic tiers `references/epistemics.md` requires. "Documented" still needs to read as more certain than "Reported sentiment" after the rewrite — plain-speech's job is clearer words, not flatter confidence.

## Output format

The structure in `references/synthesizer-prompt.md`: The Question, What's Established, What's Emerging or Contested, Known Issues, Alternatives Worth Knowing About (if researched), Where This Project's Scale or Constraints Might Not Fit, Sources Consulted, Confidence Summary. Adapt as needed; drop sections with nothing to say. Run through `plain-speech` (step 6) before being shown.

## Common failure modes to avoid

- **Hype bias.** Trending in search results or social sentiment isn't evidence of merit. Report the trend as a trend.
- **Staleness.** "Recent" expires fast. Date everything; don't let a two-year-old post pass as current news.
- **Forcing fit.** The most interesting finding is often the one that doesn't apply here — say so explicitly rather than letting an exciting result imply it's actionable for this project.
- **Collapsing sentiment into fact.** "People are moving to X" is a claim about sentiment, not a technical fact about X. Keep them in separate sections.
- **Sliding into a recommendation.** Even a hedged one ("you'd probably benefit from...") crosses the line this skill exists not to cross. Describe tradeoffs; don't resolve them for the person.

## Adding a new angle

Add a `references/angles/<name>.md` playbook adapted from `references/angles/official-sources.md`, and add a row to `references/angle-playbook.md` plus a line in Step 3's roster. The investigator/synthesizer loop doesn't change.
