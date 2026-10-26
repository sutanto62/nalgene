---
name: principle-fix-root-causes
description: "Apply when debugging. Trace a symptom back to the first place the system went wrong and fix it there. A guard is a fix only when the state it handles is legitimate."
disable-model-invocation: true
---

# Fix root causes

Fix the fault, not the place it surfaced. The symptom is where a wrong state became visible. The root cause is the first place the system did something it shouldn't, or failed to handle a state it should.

**Why:** a symptom fix hides the evidence and leaves the fault free to resurface elsewhere. Each one leaves a workaround for a bug nobody named.

**Pattern:**
- Reproduce first.
- Ask "why" at each step until the answer is a wrong decision in the code, not a consequence of an earlier one.
- Ask whether the state that triggered the symptom is legitimate. If the system may be in that state, the code that broke on it is the root cause — handle it there. If the state should never exist, find what produced it; a check at the break point just hides the bug.
- When stuck, instrument and read the actual state. Don't guess.
- Check for the pattern, not just the instance — find every place with the same fault.
- If a workaround needs a paragraph of comment to justify it, the code is wrong. Fix the code, not the comment.
