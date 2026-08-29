# eval

Measure whether this skill actually works, and whether it is getting better.

The full protocol lives in `evals/README.md` at the repo root. Read it before running.
This file is the entry point.

## Why

Everything else in this skill is assertion. `eval` is the only part that can be wrong in a
way you can detect. Without it, the banlist grows, the prose gets flatter, and nobody can
say whether it improved.

## What it measures

A held-out piece by the author is the target. A de-voiced brief of that piece is the input.
The skill drafts from the brief. A blind judge scores the draft against the real piece on
seven dimensions.

The score alone means nothing, so two more runs bracket it:

- **Ceiling** — a different real piece by the same author, scored against the reference.
  How well one piece of a person's writing matches another. The realistic maximum.
- **Floor** — the same brief drafted with no profile. Good generic writing.

The number that matters is the fraction of the floor-to-ceiling headroom closed, reported
per dimension.

## Running it

```
/soundlikeme eval                    all cases
/soundlikeme eval <case-id>          one case
/soundlikeme eval --ablation         profile vs no profile only
```

1. Read `evals/README.md` and `evals/rubric.md`.
2. For each case: draft from `brief.md` with the profile; draft again without it; take the
   ceiling piece named in `meta.md`.
3. Dispatch a **fresh judge subagent** per comparison. It sees the reference and one
   candidate, and the rubric. It does not see this skill, the profile, or which candidate is
   which. Shuffle order.
4. Collect scores with their evidence quotes. Discard any score without a quote.
5. Compute headroom closed per dimension.
6. Write `evals/results/YYYY-MM-DD-<label>.md` using the template in `evals/README.md`.
7. Ask the user for the **author verdict**: would publish as-is / would edit lightly / would
   rewrite. Record it.

## Reading the result

- **Ablation is the important one.** If drafting with the profile does not beat drafting
  without it, the profile is decorative. Report this bluntly; it is the finding that matters
  most and the one there is most temptation to bury.
- **A dimension where ceiling and floor are close** does not discriminate this author. Say
  so and consider dropping it for them.
- **Ceiling below floor on any dimension** means the judge is broken, not that generic
  writing beat the author. Investigate the judge before believing anything else in the run.
- **Rubric up, verdict flat** means the rubric is wrong. Change the rubric. It serves the
  author's judgement, never the reverse.

## Adding a case

See `evals/README.md`. Briefs are the delicate part: bullets and fragments only, no sentence
lifted from the original except material the author was always going to quote. A brief that
leaks phrasing rigs the eval in our favour, which is worse than having no eval.
