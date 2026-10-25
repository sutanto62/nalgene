# 3. Investigate before you touch code

Two skills, two different questions:

- **`/how`** — "how does X work", before changing something, or "where should this live". Explains architecture and runtime flow at onboarding depth.
- **`/why`** — "why does X work this way", design rationale, regressions, historical motivation. Takes "what it does" as given — run `how` first if you don't already know.

```text
/how how does the invite-expiry job decide which rows to touch?
```

```text
/why why does the invite expiry job batch in groups of 50 instead of doing them all at once?
```

Both check **openspec-explore** first — what's already documented beats re-deriving it from code. Both spawn read-only `nalgene-explorer` subagents (never a writing-capable agent), so an investigation can't accidentally become an edit.

`why` has a fixed two-source roster: source control (`git`, `gh`) and openspec documents. It never skips source control; it only skips openspec with a written reason ("no `openspec/` directory", or a provable "predates openspec"). Its answer separates **What We Found** from **What We Can Reasonably Infer** — don't read "appears to" and "because" as the same claim; the synthesizer chose the word deliberately.

## Odoo projects

`how` has an Odoo track: explorers walk Community → Enterprise → Custom in that order, since a later layer can override an earlier one and getting the order wrong gives a wrong answer. `why` doesn't have one yet — its general track covers Odoo questions until a real question needs layer-aware history.

## Teaching a new learner

**`/teach`** composes `how` and `why` into one plain explanation for someone who wants to actually understand something, not change it — "teach me this", "help me understand X".

```text
/teach how does the invite-expiry job decide which rows to touch, and why does it batch in 50s?
```

It opens with a short architecture overview for the project's detected stack (Odoo, Android, iOS, Node.js, Go, and a few others — see `skills/teach/references/stack-overviews.md`) before the specific explanation, so a new learner has a map to place the rest on. If the stack is ambiguous, it asks rather than guessing — a wrong opening frame costs more here than a wrong guess in `how`'s own routing does.

Every reply `teach` produces runs through `plain-speech` before you see it, so it reads plainly and without AI-sounding tells even if you never invoke `plain-speech` yourself — it uses the reader `teach` already identified, not a generic default.

## Getting outside perspective

**`/tech-advisory`** answers a different question from the other three: not "what does this codebase do", but "what's going on in the wider world around this technology" — recent news, known issues, alternatives, community sentiment.

```text
/tech-advisory what's the current landscape around the job queue library this project uses — any known issues, and what are people using instead these days?
```

It researches live (always a real web search pass, never answered from memory alone) and never recommends anything — no "you should switch", no ranked winner. It reports what's documented, what's sentiment versus fact, and explicitly flags where this project's own scale or constraints might make something that looks appealing in the abstract not actually apply. Invoke it on its own — it doesn't run as part of `teach`, since "understand what exists" and "what else is out there" are different asks with different costs (the second always pays for live research) — but like `teach`, every reply runs through `plain-speech` before you see it.

Next: [Steer with principle names](./04-steer-with-principles.md).
