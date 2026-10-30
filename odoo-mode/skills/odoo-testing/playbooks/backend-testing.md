### Backend testing

General flow for writing and running Odoo Python backend tests, scoped to the module under test. Syntax reference: [Odoo test selection](https://www.odoo.com/documentation/20.0/developer/reference/backend/testing.html#test-selection).

1. Locate or create tests under the module's `tests/` directory, subclassing `odoo.tests.TransactionCase` or `HttpCase` — these get the implicit `standard` and `at_install` tags for free. A plain `unittest.TestCase` gets none of that automatically; tag it by hand or it silently never runs.
2. Tag every test class with the module's technical name (the directory name next to `__manifest__.py`) via `@tagged('module_name', ...)`, additive to the implicit tags — this is what lets the module's tests be selected in isolation later. Only drop `standard` (`@tagged('module_name', '-standard', ...)`) for a test that must be opted into explicitly, e.g. one that's slow or destructive.
3. Install or upgrade the module at least once (`-i module_name` or `-u module_name`) — Odoo only runs tests in installed modules. Skip this switch on later runs unless the module itself changed.
4. Run scoped to the module: `odoo-bin --test-tags module_name` (this implies `--test-enable`). This is the default way to run this project's tests — don't fall back to a plain `--test-enable` run or `-u all` just to "run everything"; that reruns every installed module's suite, not just the change under test.
5. Iterating on one failure: narrow further with `--test-tags module_name.method_name` (if the method name is unique) or `module_name:ClassName.method_name`.
6. Spanning more than one module, or excluding a known-slow test: comma-separate tags, prefix with `-` to subtract — e.g. `--test-tags module_a,module_b,-slow`.
7. Verify per **principle-prove-it-works**: a FAIL or ERROR anywhere in the scoped output is blocking. Re-run the same `--test-tags` command after a fix; don't widen the run just to double-check, and don't declare done against a subset of the output.
8. Report: the module(s)/tag(s) run, the exact `--test-tags` value used, and the pass/fail count.
