---
name: principle-prove-it-works
description: "Apply before reporting any claim of success, a fix, or a measured number. Run it and show the real output - a green build is not proof."
disable-model-invocation: true
---

# Prove it works

A claim of "it works" is worth nothing until something ran and showed it. Tests passing is evidence for the paths the tests cover, not for the behavior you actually touched.

**Why:** a confident, wrong report costs more than a slow, honest one — it gets merged, shipped, and discovered in production.

**Pattern:**
- Bug fix: paste the failing repro, then the same repro passing after the fix.
- Feature: exercise the real path — run the app, hit the endpoint, render the screen. Don't infer behavior from the diff.
- Performance claim: measure before and after on the same input, state the method. A plausible number with no run behind it doesn't ship.
- "Should work now" and "this looks correct" are not verification. Either you ran it, or say you didn't.
