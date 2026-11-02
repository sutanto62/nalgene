---
name: principle-backend-security
description: "Apply when adding or touching a request/event boundary, an authn/authz decision, a secret, or a third-party dependency in a backend service. Validate at the edge, enforce the specific least-privilege decision, never hardcode a secret, use parameterized queries, and check new dependencies for known vulnerabilities."
disable-model-invocation: true
---

# Backend: security at the boundary

A backend service is the actual trust boundary — it's the thing an external caller gets direct access to, so an assumption it makes about who's calling and what they're allowed to do is the real attack surface, not a code-quality nit to clean up later.

**Why:** a generated contract type (OpenAPI/AsyncAPI) checks shape, not intent — it confirms a field is a string, not that the string is a safe length, a valid state transition, or owned by the caller. An authorization check that only confirms "is logged in" instead of "is this identity allowed to act on this specific resource" is the gap between a normal user and one who can read or modify someone else's data by changing an ID in the request. A hardcoded secret or a vulnerable dependency sits quietly until it's the one thing an attacker needed.

**Pattern:**
- Validate and sanitize every externally-supplied input — path, query, body, headers, event payload — at the boundary, beyond what the generated contract type already checks: string length, numeric range, allowed enum/state transition, ownership of the resource being acted on.
- Enforce authentication and the specific authorization decision before a handler or event consumer does anything with the request: not just "is this caller logged in" but "is this identity allowed to act on this specific resource." Deny by default; keep the check declarative (middleware/policy), not scattered inline across handlers where one can be missed.
- Never hardcode a secret, API key, or credential in code or committed config. Read secrets through the project's secret manager or env mechanism; never log a secret, token, or a full PII field — mask or redact it.
- Use parameterized queries or prepared statements always, regardless of ORM or query-builder choice. A string-built query is an injection risk even behind an ORM, if a raw fragment is concatenated anywhere in the call chain.
- Before adding a new third-party dependency, check it for known vulnerabilities with the project's existing scanner (e.g. `govulncheck`, `npm audit`/`osv-scanner`) and pin it to a scanned version; re-check when bumping a version that touches request handling, auth, or crypto.
- Grant the service's own database credentials or service account the least privilege it actually needs — not a shared admin/superuser credential reused across services.
- Name the finding even when the answer is "no new attack surface introduced here." That's a finding, not silence.
