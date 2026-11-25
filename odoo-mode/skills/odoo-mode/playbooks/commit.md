### Commit message (Odoo)

Odoo's own [git guidelines](https://www.odoo.com/documentation/20.0/contributing/development/git_guidelines.html) for the commit message. Supersedes `nalgene-mode`'s `playbooks/commit.md` on the **title** only — its body shape (one-minute briefing, ~40-line budget, `##` headings, one-line bullets) still applies; don't restate it here.

**Title.** `[TAG] module: short imperative sentence`, ideally under 50 characters. Check it reads as a real sentence once completed as "if applied, this commit will `<subject>`" — `[IMP] base: prevent to archive users linked to active partners` passes that test.

**Module.** The technical directory name (e.g. `sale`, `x_my_module`), not the display name in the manifest — a display name can change, the technical one doesn't. Several modules touched → list them, or write `various` for a genuine cross-module change. Otherwise split into one commit per module; a commit spanning unrelated modules is hard to revert cleanly later.

**Tag.** One of:

- `[FIX]` a bug fix — stable branches mostly, also a bug just introduced on the dev branch.
- `[REF]` a heavy rewrite of an existing feature.
- `[ADD]` a new module.
- `[REM]` removed resources — dead code, a view, a whole module.
- `[REV]` reverts a commit that caused problems or isn't wanted.
- `[MOV]` a file moved with `git mv` and no content change, or code relocated from one file to another.
- `[REL]` a new major or minor stable release.
- `[IMP]` an incremental improvement — the default tag for most dev-branch work.
- `[MERGE]` a merge: a forward-port, or the integration commit for a multi-commit feature.
- `[CLA]` signing the Odoo Individual Contributor License.
- `[I18N]` a translation-file change.
- `[PERF]` a performance patch.
- `[CLN]` code cleanup.
- `[LINT]` a linting pass.

**Body.** Lead with WHY the change exists — the task's purpose, not "I was asked to." The diff already shows WHAT changed; explain that only when a technical choice needs justifying. Close with any references, one per line: `task-123` (related task), `opw-123` (related ticket), `Fixes #123` / `Closes #123` (closes an issue or PR on GitHub).

**Example.**

```
[FIX] account: remove frenglish

## Why
A prior commit mixed French and English labels on the same view,
confusing non-French-speaking accountants.

## Verification
- Ran the account_move form in en_US and fr_FR, labels now match the UI locale.

Closes #22793
Fixes #22769
```
