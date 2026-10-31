---
name: odoo-testing
description: "Odoo backend test scoping: tag test classes with the module's technical name and run with --test-tags <module_name> instead of the full suite. Use for '/odoo-testing', writing or running Odoo Python tests, or the verification step of any odoo-mode playbook on an Odoo project."
disable-model-invocation: true
---

# Odoo testing

Scopes Python backend test runs to the module actually under test, per Odoo's own [test-selection mechanism](https://www.odoo.com/documentation/20.0/developer/reference/backend/testing.html#test-selection) — tags decide what executes, not a full-suite run.

**Requires** the `nalgene-mode` plugin, same as `odoo-mode` — this skill assumes `odoo-mode`'s Detection has already confirmed an Odoo project.

## Two rules

1. **Tag with the module's technical name.** Every test class gets `@tagged('module_name', ...)` in addition to the `standard`/`at_install` tags Odoo adds automatically to `BaseCase` subclasses (`TransactionCase`, `HttpCase`). A bare `unittest.TestCase` gets none of those implicit tags — tag it explicitly or it never runs at all.
2. **Run with `--test-tags module_name`, never an unscoped run.** `--test-tags` implies `--test-enable` and defaults to `+standard` when not given — so a plain `--test-enable` run, or omitting `--test-tags` entirely, re-executes every installed module's `standard` suite, not just the module you changed. Always scope to the technical name from rule 1.

Full procedure — writing tests, install/upgrade order, narrowing to one failing test, combining modules — lives in `playbooks/backend-testing.md`; copy its steps into the todo list the way any `nalgene-mode` playbook is copied in.

## Why

An untagged or unscoped test run either silently skips the test you just wrote (a plain `unittest.TestCase`, no tags, never selected) or burns time re-running every other installed module's suite on every change. Tagging by module name turns "did my change break anything?" into one scoped command instead of a guess.
