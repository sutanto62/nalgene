### Commit a change

Run whenever a commit message needs writing — mid-feature, or as part of **create-pr**'s own Commits step. Covers the message itself; **create-pr** still owns the worktree, the rebase, and the PR title/body.

**Title.** Conventional Commits, `type(scope): subject`, no trailing period. Type such as `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, or `perf`. Scope is the area touched. Subject is short, imperative, and names a real symbol when one carries the change.

**Body.** A one-minute briefing for someone who will read it with the diff already open, not a lab notebook — **principle-minimize-reader-load** applied to the commit log. It says why the change exists, what it deliberately leaves out, what it could break, and how you proved it works. Short, simple sentences, few identifiers, no walls of text. Keep the whole message — title and body together — under about 40 lines. Use `##` headings, not bold lead-ins, in this order, and drop a section with nothing to say:

- `## Why`: the problem and approach, 1–2 short sentences.
- `## Scope`: what it deliberately leaves out — a follow-up, a known gap. Skip only when there's genuinely nothing left out.
- `## Blast Radius`: what it could break and why that's acceptable, 1–2 sentences.
- `## Verification`: 1–3 one-line bullets, each a real run and its outcome.

A one-line bullet beats a paragraph wherever a list of short facts would do — a reader scans bullets, they have to parse prose.

**Scope of a single commit.** One logical change per commit. A commit that touches unrelated concerns splits into two; a commit whose message needs "and" to describe it usually should too.
