# Official sources

## What this angle covers

- The project's own release notes, changelogs, and CHANGELOG.md
- Official documentation, including migration guides and "known limitations" sections
- Maintainer statements in the project's own issue tracker, discussions, or roadmap
- Standards-body publications (W3C, IETF, ECMA, etc.) for anything that's a spec, not just a library
- GitHub/GitLab releases and tags pages, for exact version history and dates
- Vendor blogs that are the project's own publication (the React team's blog about React, not a third party writing about React)

This is the most trustworthy angle — the closest thing to ground truth about what a technology actually does and when it changed. Treat findings here as the default source for the Documented tier.

## How to search it

```text
"<technology>" changelog OR release notes <year>
site:github.com <org>/<repo> releases
"<technology>" official documentation migration guide
"<technology>" roadmap OR "coming soon" maintainer
```

Check the project's own repository directly when it's public: `CHANGELOG.md`, `releases` page, pinned issues, `ROADMAP.md`. Look at the actual diff or release date, not a secondhand summary of it.

For anything spec-related (web platform features, protocols, language proposals), check the standards body directly (TC39 proposals, W3C working drafts, IETF RFCs) rather than a blog's interpretation of where the proposal stands.
