# Evals

The objective function for `soundlikeme`. Design rationale is in
[../docs/plans/2026-08-29-objective-function-design.md](../docs/plans/2026-08-29-objective-function-design.md);
this file is how to run it.

Markdown only. No scripts, no CI. The agent executes the protocol using subagents.

## The idea in one paragraph

A published piece by the target author is ground truth for how that author writes. Strip one
to a de-voiced brief, have the skill draft from the brief, and compare the draft to the real
piece. Content is held constant, so the only variable is voice. Bracket the score with a
ceiling (a different real piece by the same author) and a floor (the same brief drafted with
no profile), and report the fraction of that headroom closed.

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
    YYYY-MM-DD-<label>.md
```

## Running a case

1. **Candidate.** `draft` from `brief.md` with the author's profile loaded.
2. **Floor.** `draft` from the same brief with no profile — a competent general writer.
3. **Ceiling.** The piece named as `ceiling` in `meta.md`, unmodified.
4. **Judge.** For each of the three, dispatch a fresh subagent with:
   - `reference.md`
   - one candidate
   - `rubric.md`

   Nothing else. Not the skill, not the profile, not which candidate this is. Shuffle the
   order across runs so position cannot be learned.
5. **Collect.** Seven scores per candidate, each with an evidence quote. Discard any score
   with no quote — an unevidenced score is a guess.
6. **Compute.** Per dimension:

   ```
   headroom_closed = (candidate - floor) / (ceiling - floor)
   ```

   Report per dimension. Never collapse to a single number; a scalar hides which dimension
   moved and is the fastest way to fool yourself.
7. **Verdict.** Ask the author: would publish as-is / would edit lightly / would rewrite.

## Ablation

The control group, and the most important test here.

Same brief, two drafts — profile on, profile off. A blind judge picks which is closer to the
reference. Run it whenever the profile format or the `draft` command changes.

If profile-on does not win reliably, the profile is doing nothing. That is a real finding.
Report it plainly rather than explaining it away.

## Protection and detection fixtures

Voice fidelity is half the job. Not breaking things is the other half.

- **Protection:** already-human prose, scientific text where passive voice is correct, quoted
  material, code, precise legal wording. An edit to a protected span is a failure regardless
  of anything else in the run.
- **Detection:** genuinely slopped text. A surviving hard tell is a miss.

Every new tell added to `tells.md` should arrive with a protection fixture in the same
change. A tell without a protection is how a banlist becomes a blunt instrument.

## Writing a brief

The delicate step. A brief that leaks the author's phrasing rigs the eval in our favour,
which is worse than having no eval at all.

Rules:

- Bullets and fragments. No complete sentences from the original.
- Keep: the claims, the evidence, the examples, named sources, quotations the author was
  always going to quote, and the order of the argument.
- Drop: phrasing, rhythm, transitions, asides, the opening move, the closing move, emphasis.
- Write it flat and slightly clinical. If a bullet sounds good, it is leaking.

Draft the brief, then read it cold and ask whether you could tell who wrote the original.
If yes, flatten it further. Once reviewed by hand, freeze it — a brief edited between runs
invalidates comparison with earlier results.

## Adding a case

1. Pick a piece the profile was **not** built from. Check the profile's `held_out`
   frontmatter.
2. Save the real text verbatim as `reference.md`.
3. Write `brief.md` by the rules above.
4. Name a **ceiling** piece in `meta.md` — another real piece by the same author, similar
   register and length, also not in the profile.
5. Record provenance and register in `meta.md`.

## Practice authors

One profile overfits. Carry two or three other authors with large, freely readable,
distinctive corpora. Paul Graham is the obvious first: enormous, very distinctive, plainly
available on the web. Freely available web corpora only — clean provenance keeps the repo
shareable and there is no shortage of it.

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

- Reconstruction measures `draft`. It does not directly measure `polish`, whose input is the
  user's own rough text. `polish` needs its own fixtures — a real piece degraded into AI-ish
  prose, then restored. Not built yet.
- Judge models may reward fluent generic prose over accurate voice matching. The ceiling
  measurement partly catches this: if a real second piece by the author scores below the
  generic floor, the judge is broken.
- Seven dimensions scored by one judge on one reference is a noisy signal. Treat a
  single-point change as noise; look for movement across cases.
