# Agent Skills spec checklist

Source: https://agentskills.io/specification. Walk every box; don't skip one because it looks obviously fine — the `name`/`description` constraints fail silently in some harnesses when violated.

## Directory

- [ ] Skill lives at `<plugin>/skills/<name>/SKILL.md`
- [ ] Only `scripts/`, `references/`, `assets/` used as optional subdirs — no other required layout
- [ ] Any file this skill links to is referenced by a relative path, one level deep from `SKILL.md` (no reference chains through a reference file into another)

## `name`

- [ ] 1-64 characters
- [ ] lowercase unicode alphanumeric and hyphens only
- [ ] no leading or trailing hyphen, no `--`
- [ ] matches the parent directory name exactly

## `description`

- [ ] 1-1024 characters, non-empty
- [ ] states both what the skill does and when to use it
- [ ] names concrete trigger phrases or keywords, not just a category label — "Helps with PDFs" fails this, "Extracts text from PDFs... use when the user mentions PDFs, forms, or document extraction" passes

## Optional fields — only check the ones actually present

- [ ] `license` — short name, or points at a bundled LICENSE file
- [ ] `compatibility` — ≤500 chars, present only if the skill has a real environment requirement (a tool, a runtime, network access); most skills in this repo don't need it
- [ ] `metadata` — string-to-string map only
- [ ] `allowed-tools` — space-separated tool list; experimental, only if the skill needs pre-approved tools

## Body

- [ ] SKILL.md body under the spec's 500-line ceiling — this repo's own skills run far shorter (10-80 lines); past ~100 lines, push detail into `references/` instead
- [ ] No format requirement beyond that from the spec itself, but match this repo's voice: imperative steps, a `**Why:**` only where the rule would otherwise be confusing, no restating what the frontmatter already says

## Automated check

If the `skills-ref` CLI is available (https://github.com/agentskills/agentskills/tree/main/skills-ref), run:

```
skills-ref validate <path-to-skill>
```

If it isn't installed, walk the checklist above by hand and say so in the report — don't claim an automated validation that didn't happen.
