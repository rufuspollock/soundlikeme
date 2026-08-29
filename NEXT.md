# Next

What to do next in this repo, written so a fresh session (human or AI) can pick it up cold. Not dated — see `changelog/` for what's already been done.

## Where the eval stands

Three runs on 2026-08-29. Read them in order; each one corrects the last.

- [Run 1](evals/results/2026-08-29-first-run.md) — absolute rubric scoring. Reported profile-on beating the no-profile floor 31 to 24. **Retracted.**
- [Run 2](evals/results/2026-08-29-second-run.md) — better-matched ceiling, same texts. The identical floor text scored 31 instead of 24: a seven-point swing on byte-identical input, wider than the effect being measured. Absolute single-judge scoring cannot resolve anything at this scale.
- [Run 3](evals/results/2026-08-29-third-run-pairwise.md) — forced-choice pairwise, every comparison in both orders with a different judge. Clean and order-invariant. **Profile-on beats no-profile 4-0, all high confidence.** The ablation holds after all.

Pairwise is now the primary method; the rubric is for diagnosis only. Judges must be given the brief, or the fabrication gate cannot work — once they had it, all four flagged the floor's invented specifics ("Mark Helprin", "seventy years after death") unprompted, and both profile-on drafts came back clean.

**The run that paid for the whole harness:** judges picked winners on micro-conventions, not rhetoric — dash style, connectives, numerals, scare quotes, and one exclamation mark. And `concision.md` rule 12 was banning "Furthermore" and "Moreover" as filler while the reference author uses them as structural joints. The skill's concision layer was deleting a voice signal, in a repo built on the premise that voice survives the cleanup. Fixed, with a new "Micro-conventions are voice" section in `protections.md`.

Also confirmed: instructing `draft` to overshoot the profile's texture beat the draft written without that instruction, 2-0 in both orders. Now committed to `draft.md`.

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

#1 is the one that unblocks the most.

## For the AI (next session)

Read `AGENTS.md` first, then `docs/prior-art.md` before touching the banlist.

Roughly in order:

1. **Add a second author** — everything found so far could be a fact about one pair of essays. Paul Graham: enormous, distinctive, freely on the web. Build a profile, hold two pieces out, seed a case, re-run the pairwise ablation. This is now the highest-value work.
2. **Control for judge bias.** Run 3's judges were told "the more polished piece is often the less faithful one" — true, and possibly enough on its own to bias toward whichever draft looks rougher. Re-run one comparison with that sentence removed and see whether the verdict holds.
3. **Build `polish` fixtures.** The eval currently measures `draft` only. `polish` takes the user's own rough text, which reconstruction cannot simulate. The shape: take a real piece, degrade it into AI-ish prose (a separate pass, saved as the fixture input), then measure how much of the original the skill restores. Degrade-and-restore, with the original as the target.
4. **Build protection and detection fixtures.** Protection: already-human prose, scientific text where passive voice is correct, quoted material, code, precise legal wording — any edit to a protected span is a failure. Detection: genuinely slopped text where failure to fix is a miss. These measure the false-positive and false-negative rates that `protections.md` exists to control, and neither is measured today.
5. **Re-run the ablation after any profile-format change.** Profile on versus profile off, blind judge. It is the cheapest test here and the one most likely to produce an uncomfortable answer.

## Deliberately not doing

- **Deterministic scanner scripts.** unslop and avoid-ai-writing both ship them (regex and stylometry doing detection, the model doing judgment), and they are genuinely better for testability. Decided against for now: this repo is small, and a Python suite is the most likely thing to become the maintenance burden that kills it. Revisit only if manual eval runs become the bottleneck — that is the actual trigger, not general appeal.
- **Growing the banlist.** It has converged across all eight surveyed projects and avoid-ai-writing has 100KB of it under MIT. Adding entries is not progress here.
- **Publishing v0.2.** `SHARING.md` has a draft announcement, updated for the new structure. Hold it until the eval has actually run — announcing an eval harness with an empty results folder invites exactly the question there is no answer to yet.
