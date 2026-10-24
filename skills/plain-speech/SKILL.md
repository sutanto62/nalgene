---
name: plain-speech
description: "Rewrite text so anyone can read it once and understand it, with no AI-sounding tells left in it. Short sentences, common words, no unexplained jargon — works for a non-technical reader, not just a technical one. Use for '/plain-speech', 'make this sound human', 'simplify this for a general reader', or before sharing prose with a broad audience."
disable-model-invocation: true
---

# Plain speech

The goal is a reader with no special background reading this once and understanding it, without it sounding like an LLM wrote it. Readability for a human is the whole point — not passing an AI detector, not sounding clever, not preserving a specialist's nuance at the cost of a plain reader's understanding.

Combined from two sources: a numbered catalog of AI writing tells to scan for and fix, and [blader/humanizer](https://github.com/blader/humanizer), a draft → critique → final workflow built on Wikipedia's "Signs of AI writing" guide, with a no-fabrication rule and voice matching. This skill merges their pattern catalogs into `references/patterns.md`, keeps humanizer's verify-before-returning discipline, and adds what neither source does on its own: `references/plain-language.md`, a pass that actively lowers vocabulary and sentence complexity for a reader who isn't a specialist, independent of whether any AI tell is present at all.

## Scope

Only prose. Leave code, data, config, frontmatter, and link targets untouched. Editing a file means touching only its prose — identifiers, quoted code, and technical values stay exact.

## Step 1. Know the reader

Default: a motivated adult with no technical background and no context beyond what's on the page. If the person names a different audience (a specific team, a technical reviewer), use that instead — plain speech still applies, but the vocabulary ceiling moves with it.

If a writing sample is given for voice matching, follow its rhythm and word choice where that doesn't conflict with Step 3 — a distinctive turn of phrase can survive; a word the target reader wouldn't know can't, sample or not.

## Step 2. Strip AI tells

Scan for the patterns in `references/patterns.md`. Fix each one found. A pattern the list doesn't name but that still reads like an LLM wrote it is still worth fixing — the list is a floor, not a ceiling.

## Step 3. Simplify for the reader

Independent of Step 2 — a sentence can be free of every AI tell and still be too dense or jargon-heavy for the target reader. Apply `references/plain-language.md`: short sentences, one idea each, common words, no unexplained jargon or acronyms.

## Step 4. Verify

Before returning the rewrite:

- Every name, number, date, quote, and citation still matches the source. Nothing invented. If a sentence needs a fact that's missing, ask instead of guessing.
- Meaning is unchanged. A simpler sentence that says something subtly different is a bug, not a simplification.
- Re-read the result as the target reader would, cold. If it lands in one pass, it's done. If a sentence needs a second read, go back to Step 2 or 3.

## Output

For a short pasted passage: show the rewrite directly. For anything non-trivial, or on request, show the work the way humanizer does — the first rewrite, a short note on what still read as artificial or dense and why, then the final version — so the person sees what changed and why, not just the end state.

For a file: edit it in place, prose only.

## Adding a pattern

Add it to `references/patterns.md` under the category it fits, or a new category if none fits. Keep numbering stable once assigned — a removed rule leaves a gap rather than renumbering everything after it.
