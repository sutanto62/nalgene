### Code review

Detects the language in scope, then opens the matching checklist instead of listing per-language rules here — drop a new `<language>-code-review-checklist.md` into `references/` and a line in step 2 to extend it. This playbook doesn't touch code, so **openspec-gate** never applies to it — same reasoning as `worktree-cleanup`.

1. **Detect the language.** Check the file extensions touched by the diff and the nearest manifest (`pyproject.toml`/`requirements.txt`/`setup.py`/`setup.cfg` → Python). A diff spanning more than one language reviews each file against its own checklist, not a blended pass.
2. **Open the matching checklist.**
   - Python → `references/python-code-review-checklist.md`. Walk all three pillars — Security & robustness, Pythonic idioms & performance, Style & readability — against the diff.
   - No checklist for the detected language yet → say so explicitly, fall back to step 4 alone, and note that a `<language>-code-review-checklist.md` would close the gap.
3. **Apply the process standard regardless of language** ([Google eng-practices](https://google.github.io/eng-practices/review/reviewer/standard.html)):
   - Approve once the change definitely improves overall code health — not on reaching perfection.
   - Prefix a non-blocking polish point with `Nit:`; it shouldn't hold up an otherwise-sound change.
   - Read every line assigned; ask instead of guessing at intent.
   - Judge the change against the surrounding file/system, not the diff in isolation.
   - Call out what's done well, not only what's wrong.
4. **Cover what a language checklist doesn't**, under these dimensions regardless of language: design (does this belong here, is the timing right), functionality (edge cases, does it serve the actual caller), complexity (over-engineered for a hypothetical future need), tests (would this fail if the code broke), documentation (does a README/guide need updating alongside the code).
5. **Report:** language(s) detected, which checklist(s) applied (or the fallback), findings grouped by pillar/dimension with `Nit:` separated from blocking, and anything skipped with a one-line reason.
