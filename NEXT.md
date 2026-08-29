# Next

What to do next in this repo, written so a fresh session (human or AI) can pick it up cold. Not dated — see `changelog/` for what's already been done.

## The one thing blocking everything else

**Run the eval.** `evals/` has a protocol, a rubric, and one seeded case, and has never been executed. Until it runs, every claim in this repo is assertion — including the claim that the voice profile does anything at all.

Run `/soundlikeme eval copyright-monopoly` and produce the first file in `evals/results/`. Three drafts (candidate, floor, ceiling), a fresh judge subagent per comparison, scores with evidence quotes, headroom closed per dimension.

Expect the first run to be about debugging the harness, not about the skill. Things likely to break:

- The brief may leak Rufus's phrasing, which would flatter the candidate. Read it cold first and ask whether you could tell who wrote the original.
- The judge may reward fluent generic prose over accurate voice matching. The tell is the ceiling scoring *below* the floor on some dimension — that means the judge is broken, not that generic writing beat the author.
- Some dimensions may not discriminate at all (ceiling ≈ floor). Those should be dropped for this author rather than reported as noise.

Then get Rufus's verdict — publish as-is / edit lightly / rewrite — and check it against the rubric. If the rubric says the draft is good and he says he'd rewrite it, the rubric is wrong and gets changed. It serves his judgment, not the reverse.

## For Rufus

- **Do the A/B calibration.** `/soundlikeme calibrate`. Ten rounds against the held-out 2011 and 2009 posts. The valuable part isn't which one you pick, it's *what gave it away* — those answers are profile edits nothing else will surface.
- **Sanity-check the profile.** `skills/soundlikeme/profiles/rufus-pollock.md`. It was generated, then extended with measured markers, and some of it will be wrong. The signature moves and the "does not do" section are where to look hardest. The spelling marker is now American throughout, per your correction.
- **Say whether the era gap matters.** The profile's samples run 2015–2018; both eval pieces are 2009–2011. If your voice changed over that span, the eval is measuring drift as well as skill failure, and we need a case closer to the sample era.
- **Dogfood it on something real** and record where it failed. A published piece polished with the skill, with notes on what you had to undo, is worth more than another eval case.

## For the AI (next session)

Read `AGENTS.md` first, then `docs/prior-art.md` before touching the banlist.

Roughly in order:

1. **Run the eval** (above). Nothing else here is worth doing first.
2. **Add a second author.** Paul Graham is the obvious choice — enormous, distinctive, freely on the web. Build a profile from a handful of essays, hold two out, seed a case. A technique that only works on one person is not a technique, and right now that is exactly what we have.
3. **Build `polish` fixtures.** The eval currently measures `draft` only. `polish` takes the user's own rough text, which reconstruction cannot simulate. The shape: take a real piece, degrade it into AI-ish prose (a separate pass, saved as the fixture input), then measure how much of the original the skill restores. Degrade-and-restore, with the original as the target.
4. **Build protection and detection fixtures.** Protection: already-human prose, scientific text where passive voice is correct, quoted material, code, precise legal wording — any edit to a protected span is a failure. Detection: genuinely slopped text where failure to fix is a miss. These measure the false-positive and false-negative rates that `protections.md` exists to control, and neither is measured today.
5. **Re-run the ablation after any profile-format change.** Profile on versus profile off, blind judge. It is the cheapest test here and the one most likely to produce an uncomfortable answer.

## Deliberately not doing

- **Deterministic scanner scripts.** unslop and avoid-ai-writing both ship them (regex and stylometry doing detection, the model doing judgment), and they are genuinely better for testability. Decided against for now: this repo is small, and a Python suite is the most likely thing to become the maintenance burden that kills it. Revisit only if manual eval runs become the bottleneck — that is the actual trigger, not general appeal.
- **Growing the banlist.** It has converged across all eight surveyed projects and avoid-ai-writing has 100KB of it under MIT. Adding entries is not progress here.
- **Publishing v0.2.** `SHARING.md` has a draft announcement, updated for the new structure. Hold it until the eval has actually run — announcing an eval harness with an empty results folder invites exactly the question there is no answer to yet.
