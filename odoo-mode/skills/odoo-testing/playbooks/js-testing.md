### JavaScript unit testing (Hoot)

Flow for writing and running Odoo's JS unit tests in isolation from any real server, per Odoo's [HOOT reference](https://www.odoo.com/documentation/20.0/developer/reference/frontend/unit_testing/hoot.html).

1. Put the test file under the module's `static/tests/` directory, named `*.test.js` (e.g. `static/tests/my_widget.test.js`). This glob is what manifests sweep into the `web.assets_unit_tests` bundle — nothing extra to register once the path matches, beyond the module being installed.
2. Keep tour files out of this tree, or exclude them explicitly: a module's own `web.assets_unit_tests` entry is conventionally the whole `static/tests/**/*` glob minus `('remove', '<module>/static/tests/tours/**/*')` — tours belong to `playbooks/integration-testing.md`'s bundle instead, and must never double-load into both.
3. Import `test`, `describe`, `expect` from `@odoo/hoot` (not `@odoo/hoot-dom`, the DOM-helpers-only half that's also usable from tours). Each file is already wrapped in an implicit `describe` block named after its own path — don't add an outer `describe` unless you need a sub-suite for tagging or organization.
4. Write the test body with `expect(...).toBe(...)` and friends, mocking time/network/DOM interaction through Hoot's own helpers (`mockDate`, `mockFetch`, `click`, `queryAll`, `waitFor`, ...) instead of reaching for `fetch`/`setTimeout`/manual DOM APIs directly.
5. Run by opening `/web/tests` in the installed database — tests execute automatically and report to both the console and the on-page GUI.
6. Narrow the run with URL query params instead of editing files: `?filter=<text>` (matches full suite+test name or tags), `?tag=<name>` (case-insensitive), `?headless` (skip the GUI), `?debugTest=<text>` (filter + pause in debug mode on failure).
7. Verify per **principle-prove-it-works**: a red entry in the GUI, or a non-zero failure count logged to the console, is blocking — a `?filter=`/`?tag=` value only narrows which tests run, it never makes a failure inside that narrowed set any less blocking.
8. Report: the file(s) added/changed, the `?filter=`/`?tag=` value used to verify, and the pass/fail count.
