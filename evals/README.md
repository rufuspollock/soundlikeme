# Evals

The objective function for `soundlikeme`. Design rationale is in [../docs/plans/2026-08-29-objective-function-design.md](../docs/plans/2026-08-29-objective-function-design.md); this file is how to run it.

Markdown only. No scripts, no CI. The agent executes the protocol using subagents.

## The idea in one paragraph

A published piece by the target author is ground truth for how that author writes. Strip one to a de-voiced brief, have the skill draft from the brief, and compare the draft to the real piece. Content is held constant, so the only variable is voice. Bracket the score with a ceiling (a different real piece by the same author) and a floor (the same brief drafted with no profile), and report the fraction of that headroom closed.

## Layout

```
evals/
  README.md              this file
  rubric.md              seven dimensions with anchors
  cases/<case-id>/
    meta.md              author, register, provenance, ceiling piece
    brief.md             de-voiced input
    reference.md         the real published text
  results/
    YYYY-MM-DD-<label>.md        the write-up
    YYYY-MM-DD-<label>/          the three scored texts, kept so a score can be checked
```

## Pairwise is the primary method

Absolute 1-5 scoring proved too noisy to use. In run 2 an identical floor text scored 24 and then 31 on two runs against the same reference — a seven-point swing on byte-identical input, wider than any effect we are trying to measure. Use it for diagnosis, never for a verdict.

Forced-choice comparison is robust to that noise and produced clean, order-invariant results on the same texts:

1. Take two candidates written from the same brief.
2. Give a fresh judge the reference, **the brief**, and both candidates as `text-1` and `text-2`.
3. Ask one question: which reads more like the same person who wrote the reference? Forced choice, no ties, plus confidence and quoted evidence.
4. Run it twice with the order swapped, using a different judge each time. A result that flips with order is not a result.

The judge must be told to judge voice, not quality, or it will reward good writing. It must also get the brief: without it the fabrication check cannot work, because the judge cannot tell an invented specific from one that was supplied.

Until run 4 the instruction also carried "the more polished piece is often the less faithful one". True, and suspected of tilting judges toward whichever draft looked rougher. Run 4 re-ran a run 3 comparison without it and got the same winner in both orders at high confidence, so the sentence is gone: one confound lighter, and the finding it might have produced survives without it. Do not put it back.

Both drafters must be fresh agents that have never seen the reference and are forbidden from going to look for it. A draft written by a session that has read the piece it is being compared against measures exposure, not the skill — run 3 had this problem and did not know it.

Run pairwise for verdicts. Run the rubric below when you want to know *which dimension* is off, and treat the numbers as indicative.

## Running a case

1. **Candidate.** `draft` from `brief.md` with the author's profile loaded.
2. **Floor.** `draft` from the same brief with no profile — a competent general writer.
3. **Ceiling.** The piece named as `ceiling` in `meta.md`, unmodified.
4. **Judge.** For each of the three, dispatch a fresh subagent with:
   - `reference.md`
   - `brief.md` — required; the fabrication gate is decorative without it
   - one candidate
   - `rubric.md`

   Nothing else. Not the skill, not the profile, not which candidate this is. Shuffle the order across runs so position cannot be learned.
5. **Collect.** Seven scores per candidate, each with an evidence quote. Discard any score with no quote — an unevidenced score is a guess.
6. **Compute.** Per dimension:

   ```
   headroom_closed = (candidate - floor) / (ceiling - floor)
   ```

   Report per dimension. Never collapse to a single number; a scalar hides which dimension moved and is the fastest way to fool yourself.
7. **Verdict.** Ask the author: would publish as-is / would edit lightly / would rewrite.

## Ablation

The control group, and the most important test here.

Same brief, two drafts — profile on, profile off. A blind judge picks which is closer to the reference. Run it whenever the profile format or the `draft` command changes.

If profile-on does not win reliably, the profile is doing nothing. That is a real finding. Report it plainly rather than explaining it away.

Run this pairwise, not by comparing absolute scores. As of 2026-08-30 it stands at 6-0 to profile-on across two authors, every comparison in both orders, all judges high confidence.

## Protection and detection fixtures

Voice fidelity is half the job. Not breaking things is the other half.

- **Protection:** already-human prose, scientific text where passive voice is correct, quoted material, code, precise legal wording, and an author's genuine habit that the tell list marks as a hard tell. An edit to a protected span is a failure regardless of anything else in the run.
- **Detection:** genuinely slopped text. A surviving hard tell is a miss.

These live in [fixtures/](fixtures/), which has its own README covering how to run and score them. As of 2026-08-30: zero false positives on 76 protected spans, 63 of 66 planted tells caught.

Every new tell added to `tells.md` should arrive with a protection case in `protections.md` and a fixture here, in the same change. A tell without a protection is how a banlist becomes a blunt instrument; a tell nobody has measured a false-positive rate for is a guess.

## Polish fixtures

Reconstruction measures `draft` only. `polish` gets its own family, built by degrade-and-restore: take a real piece, degrade it into bland AI-ish prose in a separate pass, freeze that as the input, then measure how much of the original the skill puts back. See [fixtures/polish/README.md](fixtures/polish/README.md).

## Writing a brief

The delicate step. A brief that leaks the author's phrasing rigs the eval in our favor, which is worse than having no eval at all.

Rules:

- Bullets and fragments. No complete sentences from the original.
- Keep: the claims, the evidence, the examples, named sources, quotations the author was always going to quote, and the order of the argument.
- Drop: phrasing, rhythm, transitions, asides, the opening move, the closing move, emphasis.
- Write it flat and slightly clinical. If a bullet sounds good, it is leaking.

Draft the brief, then read it cold and ask whether you could tell who wrote the original. If yes, flatten it further. Once reviewed by hand, freeze it — a brief edited between runs invalidates comparison with earlier results.

## Adding a case

1. Pick a piece the profile was **not** built from. Check the profile's `held_out` frontmatter.
2. Save the real text verbatim as `reference.md`.
3. Write `brief.md` by the rules above.
4. Name a **ceiling** piece in `meta.md` — another real piece by the same author, similar register and length, also not in the profile.
5. Record provenance and register in `meta.md`.

## Practice authors

One profile overfits. Carry two or three other authors with large, freely readable, distinctive corpora. Freely available web corpora only — clean provenance keeps the repo shareable and there is no shortage of it.

Paul Graham was the first, added 2026-08-30: seven essays in the profile, three held out, the `mean-people-fail` case seeded from one of them. He was the right choice because his micro-conventions are close to the inverse of Rufus Pollock's — short sentences against long, rare spaced em dashes against constant spaced en dashes, double scare quotes against single, numbers as words against numerals, zero "Furthermore" in 13,800 words against it being a structural joint. Two profiles that push a drafter in opposite directions on the same dials are worth more than two that agree.

The third should break the pattern rather than extend it. Both current authors are male anglophone essayists writing argumentative nonfiction, and both profiles were built by the same model. Where the profile mechanism stops working is more informative than another win.

A technique that only works on one author is not a technique.

## Result template

```markdown
# Eval run: YYYY-MM-DD <label>

Skill version: <version>  ·  Cases: <ids>  ·  Judge: <model>

## <case-id>

| Dimension | Floor | Candidate | Ceiling | Headroom closed |
|---|---|---|---|---|
| Stance | | | | |
| Argument shape | | | | |
| Rhythm | | | | |
| Lexicon | | | | |
| Signature moves | | | | |
| Concreteness | | | | |
| Tells | | | | |

Ablation: profile-on won / lost / tied, N of M rounds

Author verdict: publish as-is / edit lightly / rewrite

### What the judge quoted

- [dimension]: "[quote]" — [judge's reasoning, one line]

### Notes

- [What changed since the last run, and whether it moved anything]
- [Dimensions that did not discriminate]
- [Anything suggesting the judge or the rubric is at fault]
```

## Known limits

- Reconstruction measures `draft`. `polish` has its own fixtures now, but they measure restoration of a degraded published piece, which already has the author's structure in it. Whether `polish` improves genuinely rough writing without flattening it is still unmeasured and may need the author in the loop.
- Judge models may reward fluent generic prose over accurate voice matching. The ceiling measurement partly catches this: if a real second piece by the author scores below the generic floor, the judge is broken.
- Seven dimensions scored by one judge on one reference is a noisy signal. Treat a single-point change as noise; look for movement across cases.
- The drafters, the judges and the profile-builder are all the same model family. A judge sharing a prior with the drafter may be rewarding the drafter's habits rather than the author's. Nothing here rules this out, and it is the largest unexamined threat to every result in the folder.
