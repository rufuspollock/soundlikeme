# Next

What to do next in this repo, written so a fresh session (human or AI) can pick it up cold. Not dated — see `changelog/` for what's already been done.

## Where the eval stands

It has run once — see [`evals/results/2026-08-29-first-run.md`](evals/results/2026-08-29-first-run.md). Ceiling 34/35, candidate 31, floor 24. The ablation came out positive: drafting with the profile beat drafting without it on all seven dimensions, so the profile is not decoration.

But only three of the seven dimensions had a ceiling-floor gap wider than a single point, and on those three the candidate closed half the gap. The four dimensions reporting "1.00 headroom closed" have a one-point range and are noise. The measurement works; it does not yet resolve much.

Three harness bugs were found and two are fixed (the rubric now gates on invented specifics, and tells judges to determine spelling convention from evidence in the reference rather than by impression — one judge credited the no-profile floor for using "defence" when the reference is American). The third is unfixed: the ceiling piece is 501 words against the reference's 969 and is a reading-note rather than a sustained argument, which probably caps the Rhythm dimension artificially. **Pick a better-matched ceiling piece and re-run** — that is the cheapest next improvement to the measurement.

The substantive finding: the candidate lost points on Rhythm and Signature moves for being *more polished and more evenly paced than Rufus actually is*. The profile names his parenthetical pile-ups and high sentence-length variance as markers, `protections.md` already says "flat is a tell too", both were loaded, and the draft still came out tidier than the man. Regression toward clean prose looks like a constant force rather than a missing rule. Worth trying: instruct `draft` to overshoot the profile's roughness deliberately, and measure whether that moves Rhythm.

## For Rufus

- **Give the verdict on the first run.** Read [`evals/results/2026-08-29-first-run/candidate-profile-on.md`](evals/results/2026-08-29-first-run/candidate-profile-on.md) against your real 2011 post and say: publish as-is, edit lightly, or rewrite. The rubric said 31/35. If you'd rewrite it, the rubric is wrong and gets changed — it serves your judgment, not the reverse.
- **Do the A/B calibration.** `/soundlikeme calibrate`. Ten rounds against the held-out 2011 and 2009 posts. The valuable part isn't which one you pick, it's *what gave it away* — those answers are profile edits nothing else will surface.
- **Sanity-check the profile.** `skills/soundlikeme/profiles/rufus-pollock.md`. It was generated, then extended with measured markers, and some of it will be wrong. The signature moves and the "does not do" section are where to look hardest. The spelling marker is now American throughout, per your correction.
- **Say whether the era gap matters.** The profile's samples run 2015–2018; both eval pieces are 2009–2011. If your voice changed over that span, the eval is measuring drift as well as skill failure, and we need a case closer to the sample era.
- **Dogfood it on something real** and record where it failed. A published piece polished with the skill, with notes on what you had to undo, is worth more than another eval case.

## For the AI (next session)

Read `AGENTS.md` first, then `docs/prior-art.md` before touching the banlist.

Roughly in order:

1. **Re-run with a better ceiling piece** and see whether the gaps widen (above). Then run a second case, because one case cannot tell us whether three usable dimensions is a rubric problem or a sample-size problem.
2. **Add a second author.** Paul Graham is the obvious choice — enormous, distinctive, freely on the web. Build a profile from a handful of essays, hold two out, seed a case. A technique that only works on one person is not a technique, and right now that is exactly what we have.
3. **Build `polish` fixtures.** The eval currently measures `draft` only. `polish` takes the user's own rough text, which reconstruction cannot simulate. The shape: take a real piece, degrade it into AI-ish prose (a separate pass, saved as the fixture input), then measure how much of the original the skill restores. Degrade-and-restore, with the original as the target.
4. **Build protection and detection fixtures.** Protection: already-human prose, scientific text where passive voice is correct, quoted material, code, precise legal wording — any edit to a protected span is a failure. Detection: genuinely slopped text where failure to fix is a miss. These measure the false-positive and false-negative rates that `protections.md` exists to control, and neither is measured today.
5. **Re-run the ablation after any profile-format change.** Profile on versus profile off, blind judge. It is the cheapest test here and the one most likely to produce an uncomfortable answer.

## Deliberately not doing

- **Deterministic scanner scripts.** unslop and avoid-ai-writing both ship them (regex and stylometry doing detection, the model doing judgment), and they are genuinely better for testability. Decided against for now: this repo is small, and a Python suite is the most likely thing to become the maintenance burden that kills it. Revisit only if manual eval runs become the bottleneck — that is the actual trigger, not general appeal.
- **Growing the banlist.** It has converged across all eight surveyed projects and avoid-ai-writing has 100KB of it under MIT. Adding entries is not progress here.
- **Publishing v0.2.** `SHARING.md` has a draft announcement, updated for the new structure. Hold it until the eval has actually run — announcing an eval harness with an empty results folder invites exactly the question there is no answer to yet.
