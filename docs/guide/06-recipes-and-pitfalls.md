# 6. Recipes and pitfalls

## Recipes

Bug fix:

```text
/nalgene-mode users report the export duplicates rows on retry. repro first, root-cause it, fix, verify.
```

Feature, Odoo project:

```text
/nalgene-mode add a "resend invite" model with its own security rules and a resend button on the team settings form. done when a resent invite gets a new expiry and the old link 410s.
```

Refactor:

```text
/nalgene-mode collapse the three near-identical webhook handlers into one, no behavior change. done when the same test suite passes unmodified.
```

Just need the history:

```text
/why why does this handler retry exactly 3 times instead of using exponential backoff?
```

Which skill fits:

```text
/nalgene-help I want to restructure the billing module across three files, what do I run?
```

Rewrite a draft for a non-technical reader:

```text
/plain-speech rewrite this release-notes draft so a customer with no engineering background understands it in one read
```

## Pitfalls

- **Hand-picking skills instead of stating the goal.** `nalgene-mode` routes better than a guessed skill name; give it the goal and the check instead.
- **Skipping the gate silently.** A skip with no stated reason is indistinguishable from a missed gate later. Always say why.
- **Reading `why`'s "appears to" as "because".** The synthesizer separates evidence from inference on purpose — don't collapse them when you relay the answer.
- **Asking `why` a "what does this do" question.** That's `how`'s job; `why` assumes the behavior is already known and investigates motivation.
- **Declaring success without running anything.** `prove-it-works` exists because "should work now" isn't a verification step.
- **Starting the fix before the repro exists.** A fix with no repro is a guess, even if it looks right.
- **Scaffolding a new Odoo model before checking CE/EE.** `odoo-reuse-extend` exists because Community or Enterprise often already has a model, mixin, or view worth extending — check before you duplicate it.

Back to the [guide index](./README.md).
