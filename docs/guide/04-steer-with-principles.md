# 4. Steer with principle names

Four one-rule skills you invoke by name mid-task, in plain language, to redirect the agent without restating the whole instruction:

| Say this | To get |
|---|---|
| "apply fix-root-causes" | trace the symptom back to the first wrong decision, don't patch where it surfaced |
| "apply prove-it-works" | run it and show the real output, not "should work now" |
| "apply minimize-reader-load" | the smallest diff a reviewer can hold in their head, no padding |
| "apply outcome-oriented-execution" | name the target shape of a multi-step change and converge on it, no permanent halfway state |

```text
apply prove-it-works before you report this as fixed
```

`nalgene-mode` already applies these on your behalf as non-negotiables per playbook step. Naming one explicitly is for steering mid-task when you see the agent drifting — patching a symptom, claiming success without running anything, padding a diff, stalling a migration halfway — not a prerequisite to invoking the others.

Next: [Gate non-trivial changes](./05-openspec-gate.md).
