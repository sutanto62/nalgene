# Known issues

## What this angle covers

- CVEs and security advisories (NVD, GitHub Security Advisories, the project's own security.md)
- Deprecation notices, including sunset timelines
- GitHub/GitLab issues with real traction (high reaction counts, long threads, linked duplicates) as opposed to a single unconfirmed report
- Documented footguns in the project's own docs ("gotchas", "common pitfalls", "known limitations" sections)
- Public postmortems or incident writeups from teams that hit a specific failure mode in production

Distinguish an issue's status clearly: open and unresolved, fixed in a specific version, or closed as won't-fix (and why). A known issue fixed eighteen months ago is a very different finding from one still open today — don't let a search result's age obscure which it is.

## How to search it

```text
"<technology>" CVE
site:github.com/advisories "<technology>"
"<technology>" known issues OR limitations
"<technology>" deprecated OR deprecation OR sunset
"<technology>" production incident OR postmortem
"<technology>" issues sort:reactions-desc  (on the project's GitHub issues page)
```

Check the CVE/NVD database directly for anything security-flavored rather than relying on a summary article. For issue-tracker traction, open the actual issue and read the thread — a high comment count sometimes means heated disagreement about whether it's even a real issue, not consensus that it is one.
