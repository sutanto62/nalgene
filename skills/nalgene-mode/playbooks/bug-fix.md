### Bug fix

Reproduce before you fix. A fix with no repro is a guess.

1. Reproduce the bug yourself. If it won't reproduce directly, narrow the trigger or add instrumentation until it does.
2. Find the root cause per **principle-fix-root-causes** — binary-search candidate causes with runtime evidence, don't guess, don't patch where the symptom surfaced. Odoo project → `../references/odoo-feature.md` names which build step (security, computed fields, constraints, ...) the bug likely sits in.
3. Decide if the fix needs **openspec-gate**: it does if it changes documented behavior or a public interface. Most bug fixes don't — say so and skip it.
4. Implement the smallest fix the evidence justifies. No "might help" guards alongside it.
5. Verify per **principle-prove-it-works**: rerun the original repro, confirm it now passes. "Probably fixed" is not verification.
6. Report: what was broken, the root cause, the fix, and the before/after repro output.
