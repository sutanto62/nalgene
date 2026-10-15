---
name: principle-minimize-reader-load
description: "Apply when writing or reviewing a diff. Favor the version a reviewer can hold in their head - fewer layers, less hidden state, no comments that restate the code."
disable-model-invocation: true
---

# Minimize reader load

Code is read far more than it's written. Every layer, abstraction, or piece of hidden state the reader must track is a cost paid on every future read.

**Why:** this is the no-fluff half of the stack — write less, not more, to be safe.

**Pattern:**
- Prefer the smallest change that solves the actual problem. Three similar lines beat a premature abstraction.
- Collapse a wrapper with exactly one caller into its call site.
- No comment that only restates what the next line already says. A comment earns its place by explaining a non-obvious why.
- Don't add validation, fallbacks, or config for a case that can't happen here. Trust internal invariants; guard only at real boundaries (user input, external APIs).
- If a function needs a paragraph of comments to justify its shape, the shape is wrong — fix the shape, not the comment.
