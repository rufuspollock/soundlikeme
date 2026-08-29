# Eval run: 2026-08-29 second-run

Skill version: 0.2 · Case: `copyright-monopoly` · Judge: Opus, four fresh subagents, one per text, blind to labels and to each other · Rubric: revised after run 1

Two changes from [run 1](2026-08-29-first-run.md): a properly length-matched ceiling piece, and a fourth text testing whether telling `draft` to deliberately overshoot the profile's roughness fixes the over-polish finding.

New artifacts in [`2026-08-29-second-run/`](2026-08-29-second-run/); the candidate-v1 and floor texts are unchanged from run 1 and live in that run's folder.

## Results

| Text | Stance | Arg shape | Rhythm | Lexicon | Sig moves | Concrete | Tells | Total |
|---|---|---|---|---|---|---|---|---|
| Ceiling (*Optimal Size of Nations*, real Pollock) | 4 | 4 | 4 | 5 | 5 | 5 | 5 | **32** |
| Candidate v1 (profile-on, unchanged from run 1) | 5 | 4 | 5 | 4 | 4 | 5 | 4 | **31** |
| Candidate v2 (profile-on, roughness overshoot) | 5 | 4 | 4 | 5 | 4 | 5 | 4 | **31** |
| Floor (no profile, unchanged from run 1) | 5 | 4 | 5 | 4 | 4 | 5 | 4 | **31** |

Headroom closed: not computable in any meaningful way. The ceiling-floor gap is one point in total, and zero or negative on five of seven dimensions.

## This run invalidates run 1's headline

The floor text and the candidate-v1 text are **byte-identical to the ones scored in run 1**. Same reference, same rubric bar two clarifications.

| Text | Run 1 | Run 2 | Swing |
|---|---|---|---|
| Floor | 24 | 31 | **+7** |
| Candidate v1 | 31 | 31 | 0 |

A seven-point swing on identical input. Run 1 reported that the profile beat the control by seven points; run 2, on the same texts, finds no gap at all. The difference between those two runs is not the skill. It is which judge instance happened to score the floor.

**Absolute 1-5 scoring by a single judge is too noisy to measure anything at this effect size.** The run-1 result was not a finding; it was a draw from a distribution wide enough to contain both outcomes. Everything in that write-up that leans on the 24 should be read as retracted.

## The invented-specifics gate cannot work as designed

Run 1 added a gate: specifics not in the source material score Concreteness 1. In run 2 the floor still scored Concreteness 5, and the judge said so explicitly — that "Mark Helprin" and "seventy years after death" are "within the shared source material (the reference names Helprin and discusses term extension), not inventions."

The judge was reasoning correctly from what it had. The flaw is structural: **judges see the reference, not the brief.** The reference does mention Helprin and term extension, so anything in that neighborhood looks sourced. Only the brief shows that no first name and no term length were ever supplied.

Fix: judges must be given `brief.md` alongside the reference. Applied in the pairwise run that follows this one.

## The roughness experiment is inconclusive

Candidate v2 was written under an instruction to deliberately overshoot the profile's roughness — more crowded parentheticals, more sentence-length sprawl, less tidy sectioning — on the theory that regression toward clean prose is a standing force and the draft should aim past the target.

It scored 31, the same as v1. It traded a Rhythm point for a Lexicon point.

The judge's comment on v2 is worth keeping though, because it names a new failure the instruction introduced: the added authorial nudges ("this is the part that gets underweighted", "here is where the argument usually goes off the rails") are "slightly beyond the reference's rate". Aiming for roughness produced *interjection*, not the reference's actual texture, which is parenthetical qualification. Roughness is not one dial.

Given the noise established above, this comparison needs the pairwise method before anything is concluded. The instruction has **not** been committed to `draft.md`.

## What the ceiling tells us

The better-matched ceiling scored 32, below the 34 that the badly-matched one scored in run 1 — the opposite of the expected direction. Its Argument shape lost a point for the bulleted Bad/Good enumeration, and Stance lost one for being "slightly more tentative overall than the reference's polemic."

That is a real observation about Pollock rather than a harness fault: two of his own essays differ enough in structure and force that one scores 4 against the other. It puts a hard ceiling on what any voice profile can achieve here, and it argues the rubric's top anchors are calibrated too tight — if the author's own work rarely scores 5, a candidate matching him will look worse than it is.

## Conclusions

1. **Retract run 1's ablation claim.** It has not been established that the profile beats the no-profile control. It has not been refuted either.
2. **Absolute scoring is the wrong instrument** at this effect size. Moving to pairwise forced choice with order-swapped judges.
3. **Give judges the brief**, or the fabrication gate is decorative.
4. **The rubric's anchors are too tight** — the author's own second essay scores 32/35 against his first.
5. The roughness hypothesis is untested, not disproved.
