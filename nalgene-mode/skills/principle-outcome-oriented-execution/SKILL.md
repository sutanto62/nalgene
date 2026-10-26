---
name: principle-outcome-oriented-execution
description: "Apply when planning a multi-step change, migration, or rewrite with explicit phases. Converge on the target shape - don't let a temporary compatibility state become permanent."
disable-model-invocation: true
---

# Outcome-oriented execution

State the end shape before the first commit. Each phase should move the codebase strictly closer to it, not create a second permanent way of doing things alongside the old one.

**Why:** a migration that stalls halfway leaves two code paths to maintain indefinitely — worse than doing nothing.

**Pattern:**
- Write the target state in one or two sentences before touching code.
- Each phase deletes something from the old path as it adds the new one. Don't accumulate parallel implementations.
- If you must stop mid-migration, record exactly what's left and why it's safe to pause there (an openspec change note or a tracked follow-up, not just the conversation).
- "Keep both for now, migrate later" needs an explicit owner and a tracked task, or it never happens.
