---
name: how
description: "Use for 'how does X work', code walkthroughs before changing something, and placement/ownership/layering questions ('where should this live', 'which package owns this'). Explains subsystem architecture and runtime flow at onboarding depth."
disable-model-invocation: true
---

# How

Explore the codebase to answer "how does X work?" at the level a senior engineer needs to start working in a subsystem confidently — not so little it's vague, not so much it reads like annotated source.

Every spawn uses `subagent_type: nalgene-explorer` (read-only, general-purpose). Never substitute a writing-capable agent for this — the explainer only needs to read.

## 0. Check openspec first

Run **openspec-explore** for the question. If it finds a spec, its account of intended behavior is part of the explainer's input, alongside what the explorer(s) find in code. If code and spec disagree, that's a Gotcha, not something to quietly pick one side of.

## 1. Pick a track

- **Odoo**: the question names Odoo, or the touched paths look like an Odoo module (`__manifest__.py`, `models/`, `views/*.xml`, `security/ir.model.access.csv`, `odoo.addons.*`). Use `references/explorer-prompt-odoo.md` and `references/explainer-prompt-odoo.md`.
- **General**: everything else. Use `references/explorer-prompt.md` and `references/explainer-prompt.md`.

Ambiguous → general track. Wrong track is cheap to notice and redo; don't stall picking.

**Odoo track only:** resolve `{CE_PATH}`, `{EE_PATH}`, `{CUSTOM_PATH}` yourself, once, before spawning anything — relative to the project root. Check for an obvious, already-known layout first (a core Odoo checkout, a directory literally named `enterprise`, a project-/company-prefixed addons directory); if more than one candidate fits a slot or none does, ask once rather than guessing. No Enterprise checkout at all → `{EE_PATH}` is `(not applicable)`. Fill these same three values into every explorer and the explainer this run — don't make each subagent rediscover them.

## 2. Assess complexity

- **Simple** (single module, small utility, a narrow "how does function X work"): no explorers. Spawn one `nalgene-explorer` with the track's explainer prompt, explore-and-explain in one pass, no explorer-findings section. Go to step 5.
- **Complex** (a subsystem spanning multiple files/services, a cross-cutting feature, a full architectural overview): go to step 3.

When in doubt, take the simple path.

## 3. Explore (complex only)

Two ways to decompose, depending on what the track's explorer prompt defines:

- **Independent angles** (the general track): 2-4 arbitrary slices of the same subsystem, with no ordering between them. Spawn all explorers in parallel.
- **Ordered layers/sources** (a framework track that names them, e.g. Odoo's Community → Enterprise → Custom): one explorer per layer, each tagging its findings with the layer it covered. Later layers can override earlier ones, so the explainer needs to know which is which — spawning them in parallel is fine as long as every finding carries its layer tag; don't let the explainer guess which layer a finding came from.

Either way, spawn all explorers in a single batch, each an `nalgene-explorer` with the track's explorer prompt and its angle or layer filled in.

## 4. Synthesize (complex only)

Once every explorer returns, spawn one `nalgene-explorer` with the track's explainer prompt, openspec-explore's note, and every explorer's findings filled in.

## 5. Present

Present the explainer's output. Light edits for clarity or conversation context are fine. Don't substantially rewrite it.

## Output format

Sections from the track's explainer prompt, dropping any that don't apply.

## Adding a new track

A track is a routing line here plus an `explorer-prompt-<name>.md` / `explainer-prompt-<name>.md` pair. Two shapes so far:

- **Angle-based** (general): no inherent order between slices — any subsystem that isn't better described as layers.
- **Layer-based** (Odoo): the framework has a known, ordered stack where a later source can override an earlier one, and getting the order wrong gives a wrong answer. Odoo's is Community → Enterprise → Custom addons. A backend track would likely take this shape too — API contract (OpenAPI/AsyncAPI spec) first, establishing the documented behavior, then the implementation repo second, checked against it — not yet built.

Pick angle-based by default; reach for layer-based only when the framework genuinely has override layers, and name them explicitly in the explorer prompt's "Exploration order" section the way `explorer-prompt-odoo.md` does.
