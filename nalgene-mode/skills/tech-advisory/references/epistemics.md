# Epistemics

How to reason about confidence when evidence is the live internet — abundant, uneven, fast-moving, and full of marketing dressed as consensus — and how to report it without turning a trend into a verdict. The internet shows what's being said about a technology, not necessarily what's true or what's right for this project. Pretending otherwise produces confident-sounding advice that's really just repeating whoever shouted loudest this month.

## Confidence tiers

Every claim in the output must sit in one tier. The tier sets its section and its phrasing.

### 1. Documented

An official, authoritative source states it directly: release notes, official docs, a CVE/security advisory database entry, an RFC or standards-body publication, a maintainer's own statement in the project's issue tracker or roadmap. Dated and attributable to the source, not a summary of it.

Phrasing: confident, present tense, cited with date. "As of {date}, {source} states X."

### 2. Corroborated

Multiple independent, credible sources converge on the same claim without one copying the other. For example, the changelog and a separate, unaffiliated technical writeup both describe the same behavior change, or two unrelated postmortems report the same failure mode.

Phrasing: confident but attributed. "Multiple independent sources ({list}) report X."

### 3. Reported sentiment

Developer forums, social media, conference talks, surveys, practitioner blog posts. Real signal about what people think and feel, not a fact about the technology itself. Always phrase as sentiment, never promote it to a technical claim.

Phrasing: "Practitioners on {forum/platform} are reporting X" or "Sentiment on {platform} leans toward X, with {counter-view} also present." Never "X is better" derived from this tier alone.

### 4. Speculative / early signal

A single source, an early announcement, a rumor, a pre-release claim, a roadmap item that hasn't shipped. Worth surfacing since it may matter later, but mark it as unconfirmed.

Phrasing: "One early report suggests X, unconfirmed elsewhere" or "{Project} has announced plans to X; not yet released as of {date}."

### 5. Unknown / not found

Searched and found nothing current or credible. Say so, and say what was searched.

Phrasing: "We searched {queries/sources} and found no current, credible information on X."

## The hype trap

A technology trending in search results, GitHub stars, or social sentiment is not automatically better than the incumbent — popularity is a fact about adoption and marketing reach, not about fitness for any particular problem. Report a trend as a trend (tier 3), never let it slide into an implied verdict about merit. The louder and more recent something is, the more likely it is to be over-represented relative to its actual track record, since track records take time to accumulate and hype doesn't wait for them.

## The grass-is-greener trap

A question often embeds a hope: "should we move to X, I assume it's faster/simpler/more modern?" Don't simply confirm it. Treat the embedded assumption as one claim to check against the evidence, not a conclusion to validate. If the evidence supports it, say so with sources. If it doesn't, or if it's more mixed than the premise assumed, say that instead.

## No recommendation, ever

This is **principle-report-dont-recommend**, applied to live research specifically. This skill's output never tells the person what to do. Not "you should adopt X," not "the better choice is Y," not a ranked list with a winner, not a hedged nudge ("you'd probably benefit from..."). Describe what each option is, what's documented about it, what issues it has, what people say about it, and where it does or doesn't fit this project's apparent scale and constraints. Stop there. The person weighs tradeoffs against context this skill doesn't have — team size, timeline, risk appetite, sunk cost, political reality — and reaching a verdict anyway would be acting on incomplete information while sounding authoritative about it.

## Scale and context fit

A pattern, library, or technique's write-ups almost always describe the context it was built for or proven in — often a scale, team size, or problem shape very different from this project's. Naming that gap explicitly isn't a recommendation against something; it's reporting a structural fact the person needs to weigh. Do this using the lightweight project context gathered in Step 2, not generically: "this pattern's documented benefits center on coordinating many independently-deployed services; this project's architecture is a single deployable, so the coordination problem it solves may not be present here" beats "this might not fit depending on your scale."

## Phrasing guide

**Confidence words** ("is", "shows", "confirms") belong only to Documented and Corroborated claims, cited immediately adjacent.

**Hedges** signal sentiment or speculation, not fact. Use them deliberately in those tiers: "reports suggest", "sentiment leans toward", "one source claims", "unconfirmed as of {date}".

**Words to avoid:**

- "obviously better" / "clearly superior" — nothing in this skill's evidence base supports superiority claims; it supports descriptions of tradeoffs
- "everyone is moving to" — almost always an overstatement of tier-3 evidence; name who specifically, and how many, if that's knowable
- "you should" / "we recommend" / "the right call is" — this skill does not make calls, see No recommendation above
- "future-proof" / "cutting-edge" as a selling point stated as fact rather than attributed sentiment
- "I think" / "I believe" — report what the research found, not a personal opinion

## Dates are load-bearing

Every claim needs a date, and every date needs to be checked against today, not assumed current because it appeared in a recent search result — search results surface old content constantly. A blog post from two years ago describing "the latest approach" is not current; say when it's from and let the person judge whether it's since been superseded.

## When sources disagree

Surface the disagreement, with both sides cited. Don't pick the one that tells a tidier story, and don't average them into a mushy middle that represents neither. If official docs say one thing and strong community sentiment reports a different real-world experience, both are worth reporting — they're often both true at different scales or in different configurations.

## Before finalizing

Before the synthesizer delivers the output, it checks every claim: it sits in one tier, has a citation with a date where the tier requires one, and uses tier-appropriate phrasing. No sentence recommends a course of action, even gently. The scale/fit section (if project context was gathered) names the specific structural mismatch, not a generic hedge. If every single finding came back glowing with no caveat, contested claim, or known issue anywhere, that's suspicious — technology writeups almost never agree on everything; check whether the investigators searched only one kind of source.
