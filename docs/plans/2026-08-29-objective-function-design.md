# Design: The Objective Function for soundlikeme

Date: 2026-08-29
Status: accepted, being implemented

## The problem

We can tell when AI writing is bad. We cannot tell when it is *ours*. Everything in
`docs/prior-art.md` measures the first thing and asserts the second. stop-slop scores
"Authenticity" 1-10 with no reference text. unslop computes a stylometric silhouette but
never validates that the number tracks an author's own judgment.

Without a measure, every change to these skills is a guess. The banlist grows, the prose
gets flatter, and nobody can say whether it got better.

This is harder than a test suite because there is no exact target. But it is easier than it
first looks, because we can *manufacture* targets.

## The core move: reconstruction against held-out text

A published piece by the target author is ground truth for how that author writes. So:

1. Take a piece the voice profile was **not** built from.
2. Strip it to a **brief** — the argument as flat bullets: claims, evidence, quotes,
   examples, structure notes. Same content, no style.
3. Run `draft` from the brief with the profile loaded.
4. Compare the output against the real piece.

Content is held constant, so the only variable is voice. This is the closest thing to a
unit test available here.

The brief is the delicate part. If it leaks the author's phrasing, the eval is rigged. Rule:
briefs are bullets and fragments only, no sentence lifted from the original except material
explicitly marked as a quotation the author was always going to quote.

## The scoring problem, and the fix

A rubric score of 4.1/5 means nothing on its own. Fix it with two reference points scored
by the same judge on the same rubric:

- **Ceiling:** a *different* real piece by the same author, scored against the reference.
  This is how well one piece of a person's writing matches another piece of their writing.
  It is the realistic maximum. It will not be 5/5, and seeing that is the point.
- **Floor:** the same brief drafted by a competent model with **no** profile — good generic
  writing.

The objective function is then:

```
headroom_closed = (candidate - floor) / (ceiling - floor)
```

Reported per rubric dimension, not as a scalar. A single number tells you nothing
actionable; seven dimensions tell you *what* is off.

This also gives an honest failure mode. If `ceiling - floor` is small for a dimension, that
dimension does not discriminate this author and should be dropped for them.

## The control group nobody runs

**Ablation.** Same brief, run with the profile and without it. A blind judge picks which is
closer to the reference. If the profile does not win reliably, the profile is decorative and
we are shipping a placebo.

This is cheap, and it is the single most important test in the suite. Run it every time the
profile format changes.

## Not breaking things

Voice fidelity is only half. The other half is refusing to edit what is already fine.

- **Protection fixtures:** already-human prose, domain text where passive voice is correct,
  quoted material, code blocks, precise legal or technical wording. An edit to a protected
  span is a failure, full stop. Measures false-positive rate.
- **Detection fixtures:** genuinely slopped text. Failure to fix is a miss. Measures
  false-negative rate.
- **Tell floor:** any hard tell from `tells.md` surviving in output is an automatic fail.
  A gate, not an objective.

## Keeping the rubric honest

The rubric is a proxy. The ground truth is the author.

Each run records an **author verdict** on a three-point scale: *would publish as-is* /
*would edit lightly* / *would rewrite*. If the rubric improves and the verdict does not,
the rubric is wrong and gets changed. The rubric serves the verdict, never the reverse.

## Rubric dimensions

Seven, each 1-5 with written anchors, judged against the reference:

1. **Stance** — commits where the author commits, hedges where they hedge
2. **Argument shape** — order of moves, position-first vs build-up, paragraph length
3. **Rhythm** — sentence-length variance, fragments, clause density
4. **Lexicon** — vocabulary tier, contractions, spelling convention, jargon handling
5. **Signature moves** — the devices the profile names, used at roughly the profile's rate
6. **Concreteness** — evidence density, named things, numbers, examples
7. **Tells** — absence of AI patterns (gate: 1 here fails the run regardless of the rest)

## Judge protocol

- The judge is a **fresh subagent**. It sees the reference and the candidate. It does not
  see the skill, the profile, or which candidate came from where.
- Candidate order is shuffled for every comparison.
- The judge scores against the rubric anchors and quotes evidence for each score. A score
  without a quote is discarded.

## Practice authors

One profile overfits. The suite should carry two or three other authors with large, freely
readable, distinctive corpora — Paul Graham is the obvious first (enormous, very
distinctive, plainly available on the web). Freely available web corpora only; there is no
need to go anywhere murkier, and clean provenance keeps the repo shareable.

A technique that only works on Rufus is not a technique.

## What we are explicitly not building

No scripts, no CI, no stylometric computation. The suite is markdown: cases, a rubric, and
a runbook the agent executes with subagents. If the protocol proves itself and the manual
run becomes the bottleneck, automate it then. Automating first is how these projects die.

## Shape

```
evals/
  README.md              the protocol and how to run it
  rubric.md              seven dimensions with anchors
  cases/<case-id>/
    meta.md              author, register, provenance, what was held out
    brief.md             de-voiced input
    reference.md         the real published text
  results/
    YYYY-MM-DD-<run>.md  scores, headroom, author verdict
```

Invoked as `/soundlikeme eval`.

## Open risks

- The brief-stripping step is doing a lot of load-bearing work and could quietly leak style.
  Mitigation: briefs are reviewed once by hand and then frozen.
- Judge models may reward fluent generic prose over accurate voice matching. The ceiling
  measurement partly detects this: if a real second piece by the author scores *below* our
  generic floor, the judge is broken.
- Reconstruction measures drafting. It does not directly measure `polish`, where the input
  is already the user's own rough text. `polish` needs its own fixture family: a real piece,
  degraded into AI-ish prose, then restored. Deferred to v0.3.
