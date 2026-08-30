# Next

What to do next in this repo, written so a fresh session (human or AI) can pick it up cold. Not dated — see `changelog/` for what's already been done.

## Where the eval stands

Four runs. Read them in order; each one corrects the last.

- [Run 1](evals/results/2026-08-29-first-run.md) — absolute rubric scoring. Reported profile-on beating the no-profile floor 31 to 24. **Retracted.**
- [Run 2](evals/results/2026-08-29-second-run.md) — better-matched ceiling, same texts. The identical floor text scored 31 instead of 24: a seven-point swing on byte-identical input, wider than the effect being measured. Absolute single-judge scoring cannot resolve anything at this scale.
- [Run 3](evals/results/2026-08-29-third-run-pairwise.md) — forced-choice pairwise, every comparison in both orders with a different judge. Clean and order-invariant. Profile-on beat no-profile 4-0.
- [Run 4](evals/results/2026-08-30-fourth-run-second-author.md) — a second author, the judge-bias control, and the first measurement of the false-positive and false-negative rates.

Pairwise is the primary method; the rubric is for diagnosis only. Judges must be given the brief, or the fabrication gate cannot work.

**The ablation stands at 6-0 to profile-on, across two authors, every comparison order-invariant and every judge high confidence.** Run 4 added Paul Graham — seven essays in the profile, three held out, one held-out piece seeded as the `mean-people-fail` case — and the profile beat the no-profile floor 2-0 there too. That is the first evidence that the mechanism generalizes past the person it was built for.

**Judges pick winners on micro-conventions, not rhetoric.** Six of six judgments across two authors turned on mechanical surface habits: dash style, connectives, numerals, scare-quote marks, an exclamation mark, footnote apparatus, whether an image gets unpacked, spelling convention. Not one turned on argument or structure. This is the most robust finding in the repo.

**The finding that paid for the harness:** `concision.md` rule 12 was banning "Furthermore" and "Moreover" as filler while the reference author uses them as structural joints. The skill's concision layer was deleting a voice signal, in a repo built on the premise that voice survives the cleanup. Fixed, with a "Micro-conventions are voice" section in `protections.md`, and now guarded by a regression fixture.

**Run 4's methodology fixes.** Run 3's candidate drafts were written by a session that had read the reference — a contamination the repo had not named. Run 4 used fresh subagents for both drafters, neither having seen the reference. The judge instruction "the more polished piece is often the less faithful one" was suspected of biasing toward roughness; removing it changed nothing, so the bias-removed prompt is now standard.

**Rates, measured for the first time.** `protections.md` exists to control the false-positive rate and nobody had ever measured it. Thirteen fixtures: **false-positive rate 0% on 76 protected spans, detection 63 of 66**. Including the hard `protection/mixed` fixture, where slop and protected spans share a document so a blanket disposition cannot pass it — eight repairs made, fifteen protected spans untouched. Read the 0% carefully; run 4's write-up says why.

## Needs you — tracked as issues

These are blocked on human judgment and cannot be done by an agent. Each has the detail in the issue; this is the index.

| # | What | Why it's blocked on you |
|---|---|---|
| [#1](https://github.com/rufuspollock/soundlikeme/issues/1) | Verdict on the first eval run — publish as-is / edit lightly / rewrite | You are the ground truth the rubric is a proxy for. If the rubric says 31/35 and you'd rewrite it, the rubric changes. |
| [#2](https://github.com/rufuspollock/soundlikeme/issues/2) | Run the A/B calibration | Only you can say which paragraph is yours, and more importantly *what gave it away*. |
| [#3](https://github.com/rufuspollock/soundlikeme/issues/3) | Sanity-check the profile's signature moves | Models hallucinate one or two patterns and miss the most important one. You'll spot both in a minute. |
| [#4](https://github.com/rufuspollock/soundlikeme/issues/4) | Does the 2009–2011 era gap invalidate the cases? | Requires knowing whether your voice changed between then and 2015–2018. |
| [#5](https://github.com/rufuspollock/soundlikeme/issues/5) | Dogfood on something real, record what you undid | The undo list is where `protections.md` is wrong. |
| [#6](https://github.com/rufuspollock/soundlikeme/issues/6) | When to publish the v0.2 announcement | Your call on timing. |
| [#7](https://github.com/rufuspollock/soundlikeme/issues/7) | `deslop` deletes where it should repair | Three defensible rules stack into a pass that throws away content. How aggressive the default should be is your call, and either direction has a cost. |

#1 is still the one that unblocks the most. #7 is the one run 4 found.

## For the AI (next session)

Read `AGENTS.md` first, then `docs/prior-art.md` before touching the banlist.

Roughly in order:

1. **A third author, and a different kind of one.** Both current authors are male anglophone essayists writing argumentative nonfiction, and both profiles were built by the same model. Two is enough to show the mechanism is not author-specific and not enough to show what it fails on. The useful third is one that breaks the pattern — a different register, a different first language, a technical writer, a fiction writer. Where the profile stops working is more informative than another win.
2. **Harder protection fixtures.** Seven of seven uniform fixtures returned byte-identical output, which is the cleanest possible result and should be read as a warning about the fixtures rather than a verdict on the skill. `protection/mixed` is the model: slop and protected spans in one document, so a disposition cannot pass it. Write three or four more like it, and stop writing uniform ones.
3. **Real slopped text.** The detection fixtures were written by the same session that wrote the expectations, one day after reading `tells.md`. They test the list against itself. Collect actual published slop — press releases, LinkedIn, content-farm posts — and score against that instead.
4. **Finish the polish measurement.** `evals/fixtures/polish/being-a-noob/` has the degrade-and-restore shape working end to end for one piece. What is missing is the second comparison: polished output against the *original*, not just against the degraded input. Beating a deliberately de-voiced text is a floor, and the interesting number is how much of the gap closed.
5. **A different judge model.** The drafters, the judges and the profile-builder are all the same model family. A judge sharing a prior with the drafter may be rewarding the drafter's habits rather than the author's. Nothing in the repo currently rules this out, and it is the largest unexamined threat to every result in it.
6. **Re-run the ablation after any profile-format change.** Profile on versus profile off, blind judge, both orders. Cheapest test here and the one most likely to produce an uncomfortable answer.

## Deliberately not doing

- **Deterministic scanner scripts.** unslop and avoid-ai-writing both ship them (regex and stylometry doing detection, the model doing judgment), and they are genuinely better for testability. Decided against for now: this repo is small, and a Python suite is the most likely thing to become the maintenance burden that kills it. Revisit only if manual eval runs become the bottleneck — that is the actual trigger, not general appeal.
- **Growing the banlist.** It has converged across all eight surveyed projects and avoid-ai-writing has 100KB of it under MIT. Adding entries is not progress here. Adding a fixture for an entry that already exists is.
- **Publishing v0.2.** `SHARING.md` has a draft announcement, updated for the new structure. The original reason to hold it — an empty results folder — no longer applies, but [#6](https://github.com/rufuspollock/soundlikeme/issues/6) is still your call.
