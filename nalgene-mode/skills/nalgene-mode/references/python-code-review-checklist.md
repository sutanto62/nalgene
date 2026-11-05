# Python code review checklist

Invoked by [`playbooks/code-review.md`](../playbooks/code-review.md) when the diff's detected language is Python; usable standalone otherwise.

Composite of [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html), [PEP 8](https://peps.python.org/pep-0008/), [PEP 20](https://peps.python.org/pep-0020/), and [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/stable-en/02-checklist/05-checklist). No single source covers all three pillars — this merges them. Check against the file's actual Python floor (`python_requires`) before flagging an idiom the codebase can't use yet.

## Security & robustness

- [ ] No bare `except:` or catch-all `except Exception` that swallows the error — catch the specific exception, or re-raise/isolate it [Google]
- [ ] No mutable object (`[]`, `{}`) as a default argument value — use `None` as the sentinel [Google]
- [ ] No hardcoded secret, API key, or credential in code or committed config — read through the project's secret manager or env mechanism [OWASP]
- [ ] Every externally-supplied input (path, query, body, header, event payload) is validated/sanitized at the boundary — length, range, type, allowed enum/state, ownership of the resource acted on [OWASP]
- [ ] Every query uses parameterized queries or prepared statements — no string-built or f-string-interpolated SQL fragment, even behind an ORM [OWASP]
- [ ] Output is encoded/escaped for its destination context (HTML, SQL, shell, log) before it's emitted [OWASP]
- [ ] Authentication and the specific authorization decision ("can this identity act on this resource") are enforced before a handler does anything — not just "is logged in"; deny by default [OWASP]
- [ ] No secret, token, password, or full PII field in a log line — mask or redact it [OWASP]
- [ ] Crypto uses a vetted library and a current algorithm — no hand-rolled crypto, no MD5/SHA1 for security purposes [OWASP]
- [ ] An unexpected error path rejects/denies rather than defaulting to allow [OWASP]
- [ ] New third-party dependency is checked for known vulnerabilities (`pip-audit`, `safety`) and pinned before it's added [OWASP]

## Pythonic idioms & performance

- [ ] No `+`/`+=` string accumulation inside a loop — use `''.join()` or `io.StringIO` [Google]
- [ ] Resource cleanup uses a context manager (`with`), not manual try/finally [PEP 8]
- [ ] Type check uses `isinstance()`, not `type(x) == Foo` [Google]
- [ ] `None`/singleton comparison uses `is`/`is not`, never `==` [PEP 8]
- [ ] No reinvented stdlib — `itertools`/`collections`/`functools` checked before a hand-rolled equivalent [PEP 20: one obvious way]
- [ ] One pattern per problem across the file/module — not several ways to do the same thing because each was added separately [PEP 20: one obvious way]
- [ ] Deep nesting is flattened with an early return or guard clause, not stacked if/else [PEP 20: flat is better than nested]
- [ ] No implicit type coercion, mutable global state, or monkeypatching relied on for control flow — explicit beats implicit [PEP 20]
- [ ] A comprehension/generator expression replaces a manual accumulation loop only where it stays readable on one line — a comprehension that needs re-reading twice is not an improvement [PEP 20 vs. readability tension, judgment call]
- [ ] No repeated attribute/global/dict lookup inside a hot loop — bind to a local first

## Style & readability

- [ ] Indentation is 4 spaces, no tabs [PEP 8]
- [ ] Line length matches the project's configured limit consistently (PEP 8 default 79; many projects relax to 88–100 — check the project's linter config, not the PEP default blindly) [PEP 8]
- [ ] Imports are one per line, grouped stdlib → third-party → local, each group alphabetized, absolute over relative [PEP 8]
- [ ] Naming follows `snake_case` functions/variables, `PascalCase` classes, `UPPER_CASE` constants, leading underscore for internal-use names [PEP 8]
- [ ] Whitespace: no space before a call/index bracket; space around binary operators; none around `=` in keyword args/defaults [PEP 8]
- [ ] A function returns consistently — not a bare `return` in one branch and `return value` in another [PEP 8]
- [ ] A comment says why, not what — if removing it wouldn't confuse a future reader, it shouldn't be there
- [ ] Where this checklist conflicts with the file's existing convention, the file's convention wins — consistency within context beats the letter of the guide [PEP 8: a foolish consistency]
- [ ] Naming and structure make intent obvious without a comment — explicit beats implicit [PEP 20]

## Out of scope here

Enforce the Security & robustness and Style & readability pillars with tooling, not just eyes: `bandit` for the security AST scan, `ruff`/`pyupgrade --pyXY-plus` pinned to the project's `python_requires` floor for idiom and style. A human review catches intent; a linter catches drift.
