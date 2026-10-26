### Worktree cleanup

Deletes git worktrees and local build caches, not application code. **openspec-gate** doesn't apply — there's no behavior to spec. The risk here is irreversible data loss, not behavior drift, so the gates below are the review.

1. Audit. List every worktree from `git worktree list --porcelain` — never hand-typed, since a worktree can live outside the usual sibling-folder convention. For each, record size (`du -sh`), last commit date, merge state against the integration branch (`origin/HEAD` unless the user names another base), and local changes (`git -C <path> status --porcelain`). Classify: `safe` (merged, clean), `wip:N` (N uncommitted tracked changes), `scratch:N` (untracked files only), `active` (unmerged branch, still moving).
2. The bucket is advice, not permission. Cross-check against what the user says is still in progress — ask about anything ambiguous rather than assuming silence means abandoned.
3. Check for live use before deleting. A clean worktree can still have a dev server, emulator, or shell rooted in it. `lsof +D <path>` catches a running process git state can't see; when in doubt, ask.
4. Pause on irreversible loss. `wip` and `active` worktrees pause for a decision — show the diff. A clean worktree is recoverable from its branch; uncommitted work is not. `scratch` (untracked-only) is safe to drop once you've named the files. `safe` proceeds without asking.
5. Prune the confirmed set. Per path: `git worktree remove --force <path>`. If the directory survives on ignored build artifacts, `rm -rf` it, then `git worktree prune`. Branch refs survive removal, so no commits are lost.
6. Reclaim ecosystem caches and simulators for the confirmed set, per `../references/worktree-caches.md` — Odoo, Android, iOS, Node.js, and Golang each leave different build artifacts and device images behind that `git worktree remove` doesn't touch. Clear only what that reference marks safe per-worktree; shared/global caches need separate confirmation since clearing them slows down every other worktree's next build.

**Reply:** disk usage before and after (`df -h /`), which worktrees were pruned and which caches were cleared, and a one-line reason for each one held back (in-use, or what uncommitted work is at stake).
