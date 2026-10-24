# Worktree cache reference

Per-ecosystem build artifacts, caches, and emulators/simulators that `playbooks/worktree-cleanup.md` step 6 clears. Detect the ecosystem from files in the worktree root (a worktree can match more than one — apply every section that matches). Skip a section entirely if its marker isn't present.

Split every entry into **per-worktree** (safe to clear the moment that worktree is pruned, regenerates on next build) and **shared** (lives outside any single worktree, speeds up every worktree's next build, needs separate confirmation before clearing since it slows down everyone else's next build too).

1. **Odoo** — marker: `__manifest__.py`, `odoo-bin`, or a `requirements.txt` with `odoo` in it.
   - Per-worktree: a dedicated `.venv`/`venv` if one exists per worktree (usually the single biggest cost), `__pycache__`/`*.pyc` under addons.
   - Shared: pip's cache (`pip cache purge`) and any base virtualenv shared across worktrees — don't touch these per worktree-remove.

2. **Android** — marker: `build.gradle`/`build.gradle.kts`, `gradlew`, or `AndroidManifest.xml`.
   - Per-worktree: `.gradle/` and `build/` under the project and each module.
   - Shared: `~/.gradle/caches` — clear with `./gradlew cleanBuildCache` or by deleting the versioned subfolder, only with explicit confirmation. Emulators: `avdmanager list avd` to find stale AVDs, `avdmanager delete avd -n <name>` for ones no worktree needs; `emulator -delete-snapshot` for bloated snapshots on AVDs you keep.

3. **iOS** — marker: `*.xcodeproj`, `*.xcworkspace`, or `Podfile`.
   - Per-worktree: `Pods/` if pods are vendored locally.
   - Shared: `~/Library/Developer/Xcode/DerivedData` (safe to clear entirely, Xcode rebuilds), `~/Library/Developer/Xcode/iOS DeviceSupport` (old disconnected device versions), CocoaPods cache. Simulators: `xcrun simctl --set testing delete all` (XCTest clones), `xcrun simctl delete unavailable`, then `xcrun simctl runtime list` and `runtime delete <id>` for runtimes no project targets.

4. **Node.js** — marker: `package.json`.
   - Per-worktree: `node_modules/` (biggest single cost, safe to delete and reinstall), build output (`.next/`, `dist/`, `build/`).
   - Shared: the package manager's content-addressed store — `npm cache verify`/`clean`, `pnpm store prune`, `yarn cache clean` — only on explicit request, since it serves every worktree's installs.

5. **Golang** — marker: `go.mod`.
   - Per-worktree: `go clean -cache` inside the module clears that worktree's local build cache; a `vendor/` directory if `go mod vendor` was used.
   - Shared: `$GOPATH/pkg/mod` (module cache) — `go clean -modcache` forces every worktree to re-download its dependencies on next build. Only with explicit confirmation under real disk pressure.

**Rule of thumb:** clear per-worktree items as part of the routine prune in step 6. Clear shared items at most once per cleanup pass, only after naming them to the user and getting a yes — they're not reclaimed by removing any single worktree, and clearing them costs every other worktree a slower next build.
