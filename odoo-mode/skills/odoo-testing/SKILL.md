---
name: odoo-testing
description: "Odoo test playbooks for Python backend (`@tagged`/`--test-tags`), JavaScript unit (Hoot `describe`/`test`/`expect`), and integration (HttpCase tours). Use for '/odoo-testing', writing or running any kind of Odoo test, or the verification step of any odoo-mode playbook on an Odoo project."
disable-model-invocation: true
---

# Odoo testing

Three playbooks, one per Odoo test kind, each scoping its own run instead of re-executing everything — per Odoo's [testing reference](https://www.odoo.com/documentation/20.0/developer/reference/backend/testing.html) and [frontend unit testing reference](https://www.odoo.com/documentation/20.0/developer/reference/frontend/unit_testing/hoot.html).

**Requires** the `nalgene-mode` plugin, same as `odoo-mode` — this skill assumes `odoo-mode`'s Detection has already confirmed an Odoo project.

## Pick the playbook

| Testing... | Playbook |
|---|---|
| Python business logic, models, constraints, access rights | `playbooks/backend-testing.md` |
| A JS unit (component, service, util function) in isolation, no real server | `playbooks/js-testing.md` |
| A full click-through flow against a real running server (web client + backend together) | `playbooks/integration-testing.md` |

A single feature usually needs more than one kind — backend tests for the logic, a tour for the flow that exercises it end-to-end. Don't substitute one for the other: Hoot mocks the server, so it never proves a real end-to-end path; a tour is slow and browser-bound, so it's the wrong tool for a pure JS function's logic.

Copy the matching playbook's steps into the todo list the way any `nalgene-mode` playbook is copied in.

## Why

Running the wrong scope wastes the same two ways regardless of test kind: too narrow and the test you just wrote never gets selected (an untagged Python `unittest.TestCase`, a Hoot file left out of `web.assets_unit_tests`, a tour file left out of `web.assets_tests`); too wide and every change re-executes every installed module's suite, or every test in the project, instead of just what you touched. Each playbook exists to keep that scope tight for its own kind of test.
