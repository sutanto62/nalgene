# Epistemics

How to reason about confidence when evidence is historical, fragmentary, and sometimes contradictory, and how to tell the user without flattening it into false certainty. Code shows what it does, not *why it exists*. The why lives in commits, PRs, and specs — all incomplete, biased, and sometimes missing. Pretending otherwise produces confident-sounding guesses that mislead the user.

## Confidence tiers

Every claim in the output must sit in one tier. The tier sets its section and its phrasing.

### 1. Direct

An explicit, textual citation answers the question: something an author actually *wrote* that says why, not "the code does X so the author must have wanted X." For example, a PR description "this fixes the bug where users with >1000 items couldn't paginate", a code comment "// clamp to 100 because the upstream API rejects larger values", an openspec proposal's "Why" section "we chose option A over option B because we need persistence across restarts", or a review comment from the author explaining the choice.

Phrasing: confident, present tense, cited. "This exists because X."

### 2. Supported

Multiple pieces of indirect evidence converge on one conclusion. No single source states it explicitly, but the pattern across sources makes it likely. For example, the PR title says "improve performance" and the surrounding commits all touch the same hot path and add benchmarks. Or an archived openspec change's task breakdown and the commits that closed it both center on the same constraint.

Phrasing: confident but clearly derived, citing each item. "The evidence points strongly to X: [the specific pieces]."

### 3. Inferred

A reasonable reading of the context that nothing explicitly supports. It is *your interpretation*, not a fact from the record. For example, the PR doesn't say why, but the fix was merged the same day it was opened, so it was likely a hotfix. Or the function name suggests retry logic, and its retry count of 3 matches the team's 3-retry convention elsewhere in the codebase.

Phrasing: hedged, with the chain explicit. "Given A and B, C seems likely because D."

### 4. Speculative

A plausible hypothesis on thin evidence, where other explanations fit equally well. Worth presenting, since the user may know which is right, but mark it as a guess. For example, "This might be a workaround for a dependency bug that's since been fixed, but we found no contemporary evidence of that."

Phrasing: "One possibility is X, but we have no direct evidence." Usually lives in Competing Hypotheses, alongside other possibilities.

### 5. Unknown

You looked and couldn't find out. A valid, important outcome. Document it.

Phrasing: "We searched X and Y and found no evidence of why." Be specific about *what* you searched. "We couldn't find out" is less useful than "We searched git history for the 5 commits touching this file since 2023 and openspec for any change mentioning this module; neither surfaced a rationale."

## Phrasing guide

**Confidence words** imply Direct or Supported evidence and need a citation immediately adjacent. Never use them for inferences: "because", "the reason is", "was designed to" (claims author intent), "fixes" / "addresses" / "solves" (claims the change achieved its goal), "the team decided" (claims a group decision happened).

**Hedges** signal interpretation, not report. Use them liberally in What We Can Reasonably Infer: "appears to", "seems to", "likely", "suggests", "is consistent with", "one reading is", "plausibly", "may have been", "the evidence points toward".

**Words to avoid:**

- "obviously". If it were obvious, the user wouldn't be asking
- "clearly". Almost always precedes a claim that isn't clear
- "of course". Same
- "just" (as in "it's just X for performance"). Dismissive and usually hides uncertainty
- "I think" / "I believe". You're synthesizing evidence, not giving a personal opinion. Use "the evidence suggests" instead.

## Avoid rationalization

Code that "makes sense" today may have been written for reasons that no longer apply, or that were wrong at the time. Don't retrofit a clean rationale onto messy history. Don't assume the author did the "right" thing and work backward to justify it. Don't assume a consistent pattern was intentional when it might be copy-paste. Don't turn an absence of evidence into evidence of absence ("no commit mentions security, so it must not have been a concern").

## The sycophancy trap

Users often embed a hypothesis: "Why do we do it this way, I assume it's for performance?" Don't simply confirm it. The guess is a prompt for investigation, not a conclusion to validate. Treat it as one candidate among others and check the evidence independently. If the evidence supports it, say so with citations. If not, say so, and present what the evidence *does* support.

## When evidence contradicts

Surface both sources in the output, with citations. Don't pick the one that fits a tidier narrative. For example, an archived openspec proposal says "we need this for compliance" and the merge PR says "cleaning up tech debt in this area". Both may be true (the proposal motivated the work, the PR gave the author's framing), or one may be stale. Let the user make the call.

## When evidence is missing

An honest "we don't know" is one of the most valuable outputs. The user learns the answer isn't in the obvious places, that they'll need to ask a human, or that they can decide the question isn't worth pursuing further. Failing to mark a gap, and instead filling it with a confident guess, actively harms the user, because they'll act on the guess. Name each gap concretely: the question, the sources searched, what you searched for in each, and what you found (nothing, or only tangential material).

## Before finalizing

Before the synthesizer delivers the output, it checks every claim in "What We Found" and "What We Can Reasonably Infer". Each claim has a citation or moves to Inferred or Hypotheses. Its phrasing matches its tier. It doesn't treat code as evidence for its own intent (if it does, remove or reclassify it). The output has a "What We Don't Know" section. If no gaps are mentioned, that's suspicious. Either the evidence was unusually complete or something is being swept under the rug.
