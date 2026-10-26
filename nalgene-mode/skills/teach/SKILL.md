---
name: teach
description: "Explain a subsystem or change plainly, opening with the project's high-level architecture so a new learner has somewhere to put the rest. Runs how and why and blends what they find into one explanation. Use for 'teach me this', 'help me understand X', 'explain this codebase/change to me'."
disable-model-invocation: true
---

# Teach

You explain what a thing is, how it works, and why it's built that way, in one plain account at the person's pace. The goal is that they understand it, not that you change anything.

This skill composes this plugin's `how` and `why` into one explanation, and opens with a step neither of those does on its own: ground the explanation in the project's stack before diving into the specific question, since a new learner has nothing to hang a subsystem explanation on until they know the shape of the whole. Every reply also runs through `plain-speech` (step 8) — AI-tell removal plus a plain-language pass that lowers vocabulary and sentence complexity for a reader with no technical background, since teaching should land for someone newer than a senior engineer, not just read as cleanly as one.

## 1. Detect the stack

Work out what you're teaching from language/framework markers in the project root — the same marker-based approach `worktree-cleanup`'s cache reference uses. See `references/stack-overviews.md` for the marker list and the overview each stack calls for.

If more than one marker matches, or none does clearly, say your best guess and ask before writing the architecture overview. Unlike `how`'s general-vs-Odoo routing, where a wrong guess is cheap to notice and redo, teach's opening overview sets the frame the rest of the explanation builds on — get it wrong and the whole lesson starts crooked.

## 2. Decide what they should walk away understanding

Choose from why they're asking (new to the codebase, about to change it, reviewing it, debugging it) and what they already know, both read from the conversation, not quizzed out of them. Skip what they plainly already know. Put the depth where their question is.

## 3. Open with the architecture overview

Before the specific question, ground the person in the project's shape using the overview `references/stack-overviews.md` describes for the detected stack: entry points, the core building blocks, and where this kind of question tends to live. Keep it short — a map to place the rest of the explanation on, not the explanation itself. Skip or shrink this step if the person already showed they know the architecture, e.g. a narrow follow-up mid-session.

## 4. Let how and why do the work, don't redo it

Read the code yourself to get oriented, then run `how` for how it works and `why` for why. Run them together and combine the results. Match the size to the question — both for a subsystem, maybe one is enough for a small change. Keep `why` narrow by default since its full sweep is slow; put the narrowing in the ask itself (a scoped question, git plus a source or two) so it records the skipped categories per its own contract, and widen it only when the reasons are the point.

## 5. Explain

Start with a plain definition: name the thing and say what it is in general terms, the way a senior engineer would say it out loud, with its common name if it has one. Tie it to the case in front of you ("in this project, X is used to...") and build from there: how it works, the deeper reasons, the edge cases. For each part, explain the idea so it clicks — the problem it solves and how it actually works. Walk through what happens as the system runs when that's what makes it land. Listing functions and constants is reference, not teaching.

Give the smallest complete answer first, a sentence or two, then stop. Add layers when they ask. Never a wall of text.

## 6. Keep it a conversation

Offer to go deeper or move on, and follow their lead. No quizzes, no asking them to say it back, no flagging which part is important or hard — just say it. When you'd pause, stop and let them respond. Running one-shot with no live human, deliver cleanly and put any offer to go deeper at the end.

## 7. Build pictures up, piece by piece

Open the diff, the code, or the debugger when that's the fastest way to land it. For anything with three or more moving parts, don't draw one diagram with everything at once — draw a short series instead, each one redrawing the last and adding a single part, so the reader watches the system assemble. A mermaid diagram fits a flow or structure where the labels carry the meaning. A single simple point needs no figure.

## 8. Write it through plain-speech

Before presenting anything from steps 3 or 5-7, run it through the **plain-speech** skill in full — not just the spirit of it, read its `SKILL.md` and apply both passes: strip AI-sounding tells, then simplify vocabulary and sentence structure. This applies to every reply this skill produces, not just a closing summary. Keep code references, file paths, and diagrams exact; plain-speech only touches prose.

Feed plain-speech the reader you settled on in step 2, not its own default. A senior engineer new to this one subsystem still wants plain-speech's AI-tell removal and sentence-level clarity, but not vocabulary flattened to a total beginner's level; someone genuinely new to the field wants both passes in full.

## Output format

- The architecture overview from step 3, once per teaching session, skipped if the person already has it
- The explanation itself (steps 5-7) — never a report about what you did or delivered
- Both run through `plain-speech` (step 8) before being shown

## Adding a new stack

Add a marker and its overview to `references/stack-overviews.md`. Step 1 already treats the stack list as open-ended; no change needed here.
