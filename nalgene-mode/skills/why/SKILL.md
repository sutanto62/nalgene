---
name: why
description: "Use for 'why does X work this way', 'why we picked Y', design rationale, regressions, or historical motivation. Investigates source control history and openspec documents in parallel, then returns a cited, confidence-tiered read on decisions and tradeoffs. Use how for what the code does and how it works."
disable-model-invocation: true
---

# Why

Investigate the motivation and intent behind code. The companion **how** skill answers what the code does and how it works. `why` answers what forces led to its shape.

This plugin's `why` has a fixed, two-source roster — no MCP discovery step, unlike the upstream version this is adapted from:

1. **Source control** — git history and `gh` (PRs, review comments). Always available in a git repo.
2. **Openspec documents** — change proposals and specs under `openspec/`, if the project uses openspec. A proposal's own "Why" section is often the most direct evidence this skill will find.

Every investigator and the synthesizer spawn as `subagent_type: nalgene-explorer` (read-only). Never substitute a writing-capable agent — investigating history needs no writes, and a read-only agent can't accidentally act on what it finds.

## Step 1. Understand the target and the question

The **target** is usually a chunk of code, a pattern, or a named design decision. The **question** is a design rationale, a tradeoff, a motivating edge case, an external constraint, dead code, or a broad history sweep.

If the target is vague, make your best guess from conversation context and state it briefly so the user can redirect.

**If you don't already know what the target code currently does**, run `how` first. Don't re-derive behavior here — `why` takes "what it does" as given and investigates "what made it that way." Skip this if the current behavior is already established in the conversation.

## Step 2. Establish the code anchor

Build this inline, yourself, before spawning anything — it's locating the target, not explaining it:

- File paths and line ranges, key symbols (functions, classes, constants)
- `git log --oneline -20 -- <file>` — recent commits, PR numbers visible in subjects (pattern `(#1234)`)
- `git blame -L <start>,<end> <file>` — last-touch commits for the lines in question
- `git log -1 --format=%B <commit>` on any commit worth expanding, to pull the full body and ticket IDs
- `gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews` for any substantive commit

This anchor goes to every investigator and to the synthesizer. Don't make each subagent rediscover it.

## Step 3. Check openspec availability

Run `openspec list` once. If it errors or the project has no `openspec/` directory, the openspec investigator is skipped — record that as a gap ("project doesn't use openspec"), not as a silent omission. Otherwise it's always spawned alongside source control; openspec is a fixed source for this plugin, not an optional MCP.

## Step 4. Spawn investigators

Spawn in a single batch so they run concurrently, each `nalgene-explorer` with `references/investigator-prompt.md` filled in, plus:

- **Source control** (always): `references/sources/code-archaeology.md`
- **Openspec** (if available per step 3): `references/sources/openspec.md`
- **Incident/postmortem** (cross-cutting, add to whichever investigator(s) apply): `references/sources/incident-postmortem.md`, if the target looks defensive (null checks, retry logic, timeout handling, rate limiting, feature flags, egress guards, OOM handlers)

Each investigator gathers evidence and reports it — it does not conclude. See the prompt template for the exact discipline.

### Skipping a source

Only with an explicit, written justification that goes in the final Sources Consulted:

- **Openspec not in use in this project.** Flag it as a gap: "Openspec documents skipped. No `openspec/` directory found."
- **Provably irrelevant**, a high bar: "Openspec documents skipped. This change predates the project's adoption of openspec, confirmed by `openspec list` showing no specs covering this area and the target's git history predating the `openspec/` directory's own first commit."

Source control is never skipped — it's the one guaranteed source.

If the target is a single-commit trivial case where the PR description already contains the complete answer, you may answer inline only after confirming both available category searches would be redundant. Say so explicitly. This should be rare.

## Step 5. Synthesize

Spawn one `nalgene-explorer` with `references/synthesizer-prompt.md` filled in, plus:

1. The investigator findings, including any null results and any skipped source with its justification
2. The code anchor
3. The user's original question
4. `references/epistemics.md` in full

## Step 6. Present

Present the synthesizer's output. Light edits for clarity or conversation context are fine. **Do not rewrite the confidence language** — "appears to" and "because" are not interchangeable, and the synthesizer chose deliberately.

## Output format

The structure in `references/synthesizer-prompt.md`: The Question, The Code in Question, What We Found, What We Can Reasonably Infer, Competing Hypotheses, What We Don't Know, Sources Consulted, Confidence Summary. Adapt as needed, but keep the confidence separation intact, and keep Sources Consulted as one line per source, including skipped ones with their reason.

After Sources Consulted, if the user's question is a precursor to changing this code, convert the lineage findings into a Preserve / Change / Avoid / Risk constraint set for planning the change.

## Common failure modes to avoid

- **Recency bias.** The most recent commit isn't necessarily authoritative — the current shape is often the accretion of many earlier decisions. Trace back.
- **Treating code as evidence for its own intent.** "The function retries 3 times" is a fact about the code, not a reason it retries 3 times. The reason lives in a commit message, PR, or spec — or it's unknown.
- **Re-deriving "what it does" instead of deferring to `how`.** That duplicates work this plugin already has a skill for, and `why`'s investigators are tuned for history, not behavior tracing.

## Adding a new source

When another evidence category becomes available for a project (issue tracker, chat, observability, error tracking, analytics), add a `references/sources/<name>.md` playbook adapted from the pattern in `references/sources/code-archaeology.md`, add a row to `references/source-playbook.md`, and add a line to Step 4's roster here. The investigator/synthesizer loop doesn't change — it already treats the source roster as a list, not a fixed pair.

## Odoo

No Odoo-specific track yet. `how` has one (Community → Enterprise → Custom, since load order decides which layer's behavior wins); `why`'s general track is enough for now. If the layering ever matters for a `why` question — e.g. tracing which layer's commit introduced a specific override and why — add a `references/sources/code-archaeology-odoo.md` that searches git history across the resolved `{CE_PATH}`/`{EE_PATH}`/`{CUSTOM_PATH}` the same way `how` resolves them, tagging findings by layer. Don't build it speculatively; wait for a real question that needs it.
