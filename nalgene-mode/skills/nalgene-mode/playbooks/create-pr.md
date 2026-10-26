### Create a PR

Run once a bug-fix, feature, refactor, or other playbook's work is implemented and verified. Opening the PR is not itself a gate — it ships what already passed one.

**Worktree.** Work from a git worktree off the repo's active integration branch: `demo` or `develop` if either exists locally or on `origin`, otherwise `main` or `master`. Fetch and rebase onto `origin/<base-branch>` before creating the worktree, so it starts caught up, and again right before opening the PR, so the diff under review is the diff that lands. Subagents inherit the worktree. Multiple agents on the same branch each get their own worktree, or fetch and reset to the remote branch between runs. Dirty branch with unrelated work: patch out, fresh worktree, apply. Snarled worktree: reset from the base branch, redo minimally.

**Commits.** Commit liberally. Rebase into small, ordered commits before opening the PR. Each commit is a future PR: landable, ordered to tell the story. Amend when the fix belongs in a just-made commit. New commit when separable. Commit bodies don't restate the subject.

**PR prep.** Write every PR title, description, and commit body with **technical-writing** — every layer except Diátaxis, since a PR isn't documentation. Then run the result through **plain-speech** in full, both passes (AI-tell removal, then vocabulary and sentence simplification) — not a lighter version of it.

**Titles.** Conventional Commits, `type(scope): subject`, no trailing period. Type such as `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, or `perf`. Scope is the area touched, such as `nalgene-mode` or a specific skill. Subject is short, imperative, and names a real symbol when one carries the change, for example `fix(create-pr): rebase onto origin before opening the PR`.

**Descriptions.** The PR body is a one-minute briefing for a reviewer who has the diff, not a lab notebook — this is **principle-minimize-reader-load** applied to the thing a human reads next, not just the diff itself. It says why the change exists, what it leaves out, what it could break, and how you proved it works. Short, simple sentences, few identifiers, no walls of text. The body becomes the squash commit body, so keep it under about 40 lines. Use `##` headings, not bold lead-ins, in this order, and drop a section with nothing to say:

- `## Why`: the problem and approach, 1–3 short sentences. No SHAs or rebase genealogy.
- `## What changed`: 1–3 short bullets. Name a real symbol or path only when it carries the change. Name both sides of a rename or retarget.
- `## Scope`: always. 1–3 short items on what it covers and what it deliberately leaves out (a follow-up, a known gap).
- `## Tradeoffs`: only rejected alternatives a reviewer would otherwise ask about. Skip when there was no real choice.
- `## Blast Radius`: 1–2 sentences on who or what it touches and why that's safe or risky.
- `## Verification`: 1–3 bullets, each a real run and its outcome. Perf: one primary number with its unit, before → after, with a link for the rest.

Attach videos or screenshots after the sections when they prove a claim. Full SHAs, metric tables, file-by-file checklists, and "CLEAN" verdicts go in a linked artifact, not the body.

**Forge.** Resolve the forge before the first PR operation and keep that choice through create, edit, view, and merge. `gh` is the default. If the repo already uses a stacking tool for its branch chain (Graphite's `gt`, or similar), use that tool for stack operations instead and record the choice.

**Built-in PR tool.** When the host provides one (Claude Code and Cursor both can), create, edit, retarget, and mark ready through it, not a forge CLI — a CLI-made PR misses what the tool tracks. Use the resolved forge for anything the tool doesn't cover.

**Size and stacks.** Prefer several narrow PRs to one large PR. A stack is a base-branch chain: the root PR targets the integration branch, each child branch rebases onto its parent's exact tip, and its PR targets the parent branch. As a parent merges, retarget its child onto the integration branch. Without a built-in PR tool: `gh pr create --base <parent-branch>` to open a child, `gh pr edit <pr> --base <parent-branch>` to retarget one. Branch from the integration branch only for independent work; rebase onto it before substantial stack work.

**Readiness.** Open every PR ready, never as a draft. A built-in PR tool can default to draft, so set `draft: false` on every creation call through it; with `gh`, omit `--draft`. If a PR still opens as a draft, mark it ready through the tool or `gh pr ready <number>`. Run `gh pr view <number>` before referring to a PR's status.

**After opening.** Post the URL and keep building — opening a PR doesn't start a watch loop. Finish the rest of the planned work first. Only monitor CI or review comments when asked, and do it with a plain poll (`gh pr checks <number>`, or the **loop** skill on an interval) rather than a dedicated babysit step — nalgene has no multi-agent ownership model to hand that loop to.

**Reply:** the PR URL (or each PR's URL for a stack), its base branch, and what verification ran before it opened.
