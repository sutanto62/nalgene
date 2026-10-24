# Plain language

Neither `unslop` nor `humanizer` does this pass — both assume a general but literate reader and focus on removing AI-sounding tells. This pass goes further: lower the vocabulary and sentence complexity so someone with no technical background and no prior context can follow it, whether or not any AI tell is present. Target register: a daily newspaper, not a textbook, a contract, or an internal engineering doc.

## Sentence structure

- **One idea per sentence.** If a sentence needs "and," "which," or a semicolon to hold two separate ideas, split it into two sentences.
- **Short sentences by default.** If it needs a second read to parse, it's too long or too nested. Cut clauses or split.
- **One level of nesting at most.** Avoid a parenthetical inside a parenthetical, or a clause inside a clause inside a clause.
- **Read it aloud.** If it doesn't sound like something a person would actually say to a friend, rewrite it until it does.

## Vocabulary

- **Common word over rare word, every time.** See `patterns.md` category D for specific swaps — this file covers the general rule, that file the examples.
- **No unexplained jargon or acronyms.** Define a term in plain words the first time it appears, or replace it with a description of the thing instead of its name. "The ORM" becomes "the tool that turns database rows into objects the code can use" — or is dropped entirely if the reader doesn't need to know it exists.
- **No assumed background.** Don't reference an internal term, a prior decision, or a technical standard without a one-line plain explanation the first time it comes up.
- **Concrete beats abstract.** A specific example lands faster than an abstract description of the same thing. Give the example.
- **Real numbers over vague magnitude words.** "Significantly faster" becomes the actual number or percentage, if it's known; if it isn't known, say that instead of implying precision with a vague intensifier.

## What this is not

Simplifying isn't dumbing down the facts or deleting a detail that changes the meaning. A technical fact that matters to the point stays — it just gets explained in words the reader already has, not removed. If a concept genuinely can't be made plain without losing what makes it true, say so and give the closest honest plain-language approximation rather than a misleading oversimplification.

## Quick check

Before finishing, ask: would someone with no background in this subject, reading this once, come away with an accurate understanding? If any sentence requires them to already know a term, decode an abbreviation, or re-read for structure, it's not done yet.
