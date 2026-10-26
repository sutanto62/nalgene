# Openspec Documents

## What this source contains

Openspec's `openspec/` tree is this plugin's stand-in for long-form design docs — but unlike most doc tools, its proposals have a standard shape that makes rationale easy to find instead of buried in prose:

- **Active changes** (`openspec/changes/<name>/`): a `proposal.md` whose own structure usually separates *what* changes from *why* — the clearest Direct evidence this source can produce — plus spec deltas and a task breakdown showing what was deliberately in and out of scope.
- **Archived changes** (`openspec/changes/archive/<date>-<name>/` or similar): the same, but for work that already shipped and whose rationale is now historical. The archive is often the richest part of this source — it's a dated log of past decisions, not just the current spec.
- **Specs** (`openspec/specs/<capability>/`): the current, authoritative behavior per capability, folded in from archived changes. A spec states *what's true now*, not why — but comparing it against the target code tells you whether the implementation matches the documented intent, and a mismatch is itself a finding (flag it, don't silently pick a side).

## How to search it

```bash
# See what exists at all
openspec list
openspec list --specs

# Read a specific change's proposal (the "why" is usually here)
openspec show <change-name>

# Read a spec (current, intended behavior)
openspec show <spec-name> --type spec

# Full dashboard if nothing obviously matches the question
openspec view
```

If a command errors or returns nothing, fall back to reading the tree directly:

```bash
# List archived changes by date — a timeline of past decisions
ls openspec/changes/archive/

# Grep proposals for the target's name/symbol
rg -l -i '<symbol or feature name>' openspec/
```

Read each matching proposal in full, not just its title — the "why" is often in a paragraph under a `## Why` or `## Problem` heading, or in a rejected-alternatives note, not the one-line summary. For an archived change, also check whether its tasks were all completed as proposed; a task that was dropped or changed mid-flight is worth surfacing as a contradiction between stated and actual rationale.

## What this source is best at

Documented, pre-code design rationale — written down *before* it became a diff, which is rarer and often more complete than what survives into a commit message. Strongest for questions about why a feature was shaped the way it was, what alternatives were considered and rejected, and what constraints (compliance, a dependency, a prior incident) motivated the work. Weakest for small, undocumented changes that never went through the propose step — git is the floor for those.
