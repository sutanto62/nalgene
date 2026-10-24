# AI writing patterns

Merged from pstack's `unslop` and blader/humanizer, deduplicated. Rule numbers are stable ids — a removed rule leaves a gap instead of renumbering everything after it. Scan for these, then rewrite; preserve meaning and the intended tone.

## A. Say it, don't stage it

1. **"Not just X, but Y."** State the point directly.
2. **Dramatic one-line closers.** "That's the real win." / "Let that sink in." / "No prior. No nostalgia." Cut them, or fold the one real claim buried inside into the sentence before it.
3. **Deep-sounding sayings standing in for a claim.** "At its core, what matters is...", "Symmetry is the language of trust." Replace with the specific claim.
4. **Staged run-ups.** "Let's dive in.", "Honestly? It depends..." Remove; start with the point.
5. **Arguing with nobody.** "This isn't mainly about X...", "A tempting approach would be..." Remove the unraised objection or fake alternative; keep any real claim hiding inside it.
6. **Inflated significance.** "marks a pivotal moment", "the future looks bright", "continues to thrive despite challenges". Keep the fact, drop the hype framing.

## B. Unverified or vague claims

7. **Vague attribution.** "Experts believe", "industry reports suggest", "some critics argue". Name the source or delete the claim.
8. **Borrowed authority.** "cited in NYT, BBC, FT...", "Experts agree that...". Name what was actually said and by whom, or drop it.
9. **Shallow -ing riders.** "highlighting...", "showcasing...", "underscoring...", "reflecting...", "fostering...". Delete, or replace with a real source.

## C. Forced rhythm and repetition

10. **Rule of three where the meaning doesn't call for three.** Use the natural count.
11. **Synonym cycling.** Protagonist, main character, central figure, hero, all for the same person in one paragraph. Pick one word, repeat it.
12. **Repeated sentence openings.** "She noted... She noted... She filed..." Vary the subject or merge the sentences.
13. **False ranges.** "from X to Y" where X and Y aren't points on a real scale. List the items directly instead.

## D. Vocabulary

14. **AI-flavored words.** Additionally, crucial, delve, enduring, enhance, fostering, garner, interplay, intricate, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore, vibrant. Plain word instead.
15. **Fancy stand-ins for "is"/"has".** "serves as", "stands as", "boasts", "features". Say "is" or "has".
16. **Abstract technical metaphors used as if plain.** Substrate, vector (as in "way" or "method"), locus, vantage, nexus, primitive as a noun, harness as metaphor, surface as in "API surface", bedrock, scaffolding as metaphor, modality, paradigm, north star, flywheel, ratchet as metaphor, gold-plating, evacuate for moving code. Use the concrete word a non-specialist would already know.
17. **Fancy synonyms for ordinary words.** Utilize → use, leverage → use, facilitate → help, numerous → many, "in the event that" → if.

## E. Punctuation and typography

18. **Em dash overuse.** Use periods or commas only — no em dashes, no en dashes, no hyphen standing in for a dash.
19. **Colon as a mid-sentence connector.** Fine before a list or example; not as a comparison crutch mid-sentence. Let the point stand on its own, or split the sentence.
20. **Curly quotation marks.** Use straight quotes.
21. **Hyphenating pairs that don't need it.** "the report is high-quality" → keep the hyphen only where it's standard directly before the noun.
22. **Decorative bold.** Don't bold every proper noun or acronym.
23. **Title-case or decorated headings, emojis in headings or bullets.** Sentence case, no decoration.

## F. Formatting structure

24. **Inline-header lists.** "**Performance:** Performance improved..." restates the label. Convert to prose — unless the bold lead-in ends in a period and is followed by genuinely new detail ("**Schema in TypeScript.** Tables live in one file."), which isn't a tell.
25. **A heading immediately repeated in the first sentence under it.** "## Performance" / "Speed matters." Let the heading do that job; start the sentence with actual content.

## G. Sentence construction

26. **Dense, backtrack-requiring sentences.** Split them. One idea per sentence.
27. **Passive voice with a knowable actor.** "queries are validated" → "the compiler validates queries". Passive is fine only when the actor is unknown or genuinely doesn't matter.
28. **Stacked qualifiers.** "could potentially possibly be argued that it might" → keep only what the source actually supports.
29. **A weak verb propped up by an adverb.** "runs quickly" → "is fast", or give the actual number. "Significantly improves" → the measured delta.
30. **Mannered prose.** Metaphor or flourish in place of a literal statement: "the plan holds it", "a dial worth turning", personified code, rhetorical fragments for effect. Say what it actually means.
31. **Over-compressed fragments.** Dropped articles, arrow notation, unexplained abbreviations: "Parser rejects bad date → exit 2, no write." Write the whole sentence: "The parser rejects a bad date, exits with code 2, and writes nothing."

## H. Chat and draft leftovers

32. **Chatbot phrases.** "I hope this helps!", "Let me know if...", "Of course!", "Certainly!" Remove.
33. **Sycophantic tone.** "Great question! You're absolutely right!" Respond to the substance instead.
34. **Knowledge-limit disclaimers dressed as findings.** "While details are limited in available sources, it appears..." State what's actually known, or cut the sentence.
35. **Writing about the document instead of the subject.** "This section covers...", "The table below compares...", "This function was added to replace...". Describe the subject directly; state a convention only when the reader genuinely can't see it another way.
36. **Re-explaining what the reader already knows before getting to the point.** Lead with the point; keep background only where the reader actually lacks it.

## I. Filler and hedging

37. **Filler phrases.** "In order to" → "To". "Due to the fact that" → "Because". Delete "It is important to note that".
38. **Generic, contentless conclusions.** "The future looks bright." State the actual next fact or plan instead.
