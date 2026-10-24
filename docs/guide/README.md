# The nalgene guide

nalgene works best when you stop micromanaging the agent. State a goal and how you'll know it's done; `/nalgene-mode` picks the playbook, gates non-trivial work through `openspec-gate`, and runs `how`, `why`, and the principles as the steps need them. This guide teaches that habit with realistic prompts.

Here's what you'll learn:

1. [Set up nalgene](./01-setup.md). Install the plugin in Claude Code or Cursor.
2. [Route work through `/nalgene-mode`](./02-nalgene-mode.md). Give it a goal and watch it pick a playbook.
3. [Investigate before you touch code](./03-investigate.md). `/how`, `/why`, `/openspec-explore`, `/teach` for onboarding a new learner, and `/tech-advisory` for outside perspective on a technology.
4. [Steer with principle names](./04-steer-with-principles.md). The five names that redirect an agent mid-task, and how to add a new one.
5. [Gate non-trivial changes](./05-openspec-gate.md). The propose → validate → implement → archive loop, standalone or via the router.
6. [Recipes and pitfalls](./06-recipes-and-pitfalls.md). Prompts to copy and mistakes to skip.

Read the pages in order the first time. After that, each page stands alone.

When you're stuck, or can't tell which skill fits, type [`/nalgene-help`](../../skills/nalgene-help/SKILL.md) with your question:

```text
/nalgene-help which skill should I use to understand why this check exists?
```

It answers from a lookup table without spending tokens doing the work. It doesn't start anything — you send `/nalgene-mode` yourself when you're ready.

## If you only remember one thing

Give the agent a goal and a way to check it, in your own words:

```text
/nalgene-mode the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

You don't need to name a playbook or list skills. "repro first" and a checkable outcome are all the routing signal `/nalgene-mode` needs.

Running on an Odoo project? Nothing to set up separately — nalgene detects it from the touched paths and switches tracks automatically. See the Odoo notes on [page 2](./02-nalgene-mode.md#odoo-projects) and [page 3](./03-investigate.md#odoo-projects).

Next: [Set up nalgene](./01-setup.md).

---

Platform quirks (Claude Code vs. Cursor) live in [`../harness/`](../harness/) — read the one for your host once per session if you're authoring or editing a skill, not just using one.
