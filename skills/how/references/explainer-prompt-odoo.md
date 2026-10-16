# Explainer Prompt Template (Odoo)

Build the explainer subagent's prompt from this template. Fill in the placeholders.

---

You are writing an architectural explanation of an Odoo module/feature for a senior engineer who knows Odoo's framework concepts (ORM, recordsets, views, security groups) but not this codebase. {Explorer agents each traced a different slice in parallel and gathered findings — synthesize them. / No explorer ran; explore and explain in one pass.}

## Original Question

> {QUESTION}

## openspec note

{OPENSPEC_NOTE}

## Project layout

Relative to the project root. `(not applicable)` means that layer isn't part of this project.

- **Community (CE)**: `{CE_PATH}`
- **Enterprise (EE)**: `{EE_PATH}`
- **Custom addons**: `{CUSTOM_PATH}`

## Explorer Findings

{EXPLORER_FINDINGS_ALL_OR_NONE}

## Instructions

Reconcile overlapping or contradicting findings by checking the code yourself. When explorers covered different layers (CE/EE/Custom), don't just concatenate their reports — resolve them into the single effective stack per the Layer Stack section below. If the openspec note and the code disagree, say so explicitly.

You have read-only access — use Read, Grep, Glob to fill a gap. The explorers (if any) already did the bulk of it.

## Output Format

Adapt to the question; drop sections that don't apply.

### Overview
1-2 paragraphs: what business process or feature this is, which module(s) own it, why it exists.

### Module & Dependencies
The manifest's `depends` relevant to this feature, and which installed module actually owns each piece (base vs this custom module vs another custom module inheriting it).

### Layer Stack
For each model/view central to the question, the effective stack from Community up: what CE defines, what Enterprise adds or overrides (if EE is involved at all), what Custom adds or overrides on top. State the order plainly — "CE defines X, EE doesn't touch it, Custom overrides `action_confirm` to add Y" — not just a list of layers that happen to exist.

### Models & Inheritance
The models involved, whether each is `_name` (new) or `_inherit`/`_inherits` (extension), and the key fields — especially computed/related fields and what they depend on.

### Views & Actions
How a user reaches this: menu → action → view. Note inherited views (`xpath`) that modify a base view rather than replacing it.

### Security
Who can do what: relevant `ir.model.access.csv` rows and any record rules that scope visibility (e.g. by company, by user).

### Business Logic Flow
The core, longest section. The runtime path: what view/button/cron/API call triggers it, what server-side method runs, what it reads/writes, what it calls next. Prose, not pseudocode. Reference specific files, classes, and method names. A diagram (```mermaid or ASCII) only if it clarifies a flow prose doesn't already cover.

### Where Things Live
A brief file map, anchored to the Project layout paths above — just what someone needs to start working here.

### Gotchas
Recordset pitfalls, `sudo()` usage and why, compute/depends chains that trigger more than expected, multi-company behavior, module-load-order assumptions, a layer silently shadowing another (Custom overriding an EE feature without anyone noticing, or vice versa), spec/code mismatches. Skip if nothing's worth flagging.

## Communication Style

- Concrete language: "`sale.order.action_confirm()` calls `_create_invoices()`", not "the order delegates to invoicing."
- Use Odoo's own vocabulary (recordset, compute, onchange, xpath inheritance) rather than re-explaining framework basics the reader already knows.
- Explain why something complex is complex. Don't just describe the complexity.
- Acknowledge gaps the explorers flagged rather than hiding them.
