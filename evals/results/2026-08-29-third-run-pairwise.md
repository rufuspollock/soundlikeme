# Eval run: 2026-08-29 third-run, pairwise

Skill version: 0.2 · Case: `copyright-monopoly` · Method: forced-choice pairwise, six fresh Opus subagents, every comparison run in both orders with a different judge each way. Judges were given the reference, **the brief**, and two candidates; nothing else.

Run in response to [run 2](2026-08-29-second-run.md), which found absolute scoring too noisy to measure anything.

## Results

| Comparison | Order 1 | Order 2 | Result |
|---|---|---|---|
| Profile-on v1 vs no-profile floor | v1 (high) | v1 (high) | **v1, 2-0** |
| Profile-on v2 vs no-profile floor | v2 (high) | v2 (high) | **v2, 2-0** |
| Roughened v2 vs original v1 | v2 (high) | v2 (high) | **v2, 2-0** |

Every comparison order-invariant. Every judge high confidence.

**The ablation holds: 4-0 to profile-on.** Retracted in run 2 for lack of evidence, now established by a method that survives the noise. The profile does real work.

**The roughness hypothesis is confirmed: 2-0 to v2.** The instruction to deliberately overshoot the profile's texture beat the draft written without it, in both orders. Absolute scoring had called this a tie at 31-31.

## Fabrication check, now that it works

Giving judges the brief fixed the gate that was decorative in run 2. All four judges who saw the floor flagged it unprompted:

- "Mark Helprin" — the brief supplies only the surname
- "seventy years after death" — no term length anywhere in the brief
- "Antitrust lawyers have spent decades arguing" — the brief says only that antitrust argues over market definition
- "for a digital work, is very close to nothing" — digital works are not in the brief

Both profile-on drafts: clean, per every judge. The no-fabrication rule in `draft.md` is doing something measurable.

## The finding that changes the skill

Judges did not pick winners on rhetoric. They picked on the smallest mechanical habits, and named them consistently across six independent runs:

- **Dash convention.** The reference uses spaced en dashes (` – `). One draft converted them to em dashes and three separate judges called this out as the clearest single signal. One: "a different typographic personality."
- **Connectives.** The reference leans on "Furthermore", "Nevertheless", "i.e.", "etc". A judge marked their absence as a reason a draft read wrong.
- **The exclamation mark.** The reference has exactly one — "being sole supplier of some good!" The draft that kept it beat the draft that flattened it to a period. In a judge's words: *"The reference author's one burst of enthusiasm is exactly the tic a polisher removes."*
- **Numerals.** "over 3 years ago" versus "over three years ago", cited by two judges.
- **Single versus double scare quotes.** Cited by two judges.
- **The colophon.** Ending on housekeeping rather than a rhetorical landing, cited by four.

### The skill was destroying a real marker

`concision.md` rule 12 banned "Furthermore" and "Moreover" as filler transitions. The reference author uses both as genuine structural joints, and judges penalized their absence.

The skill's own concision layer was deleting a voice signal, in a repo whose entire premise is that voice survives the cleanup. Fixed: rule 12 now defers to the profile, and `protections.md` has a new "Micro-conventions are voice" section covering dashes, connectives, quote marks, numerals, exclamations, and self-referential parentheticals — each with the evidence from this run.

This is the best argument yet for the eval existing. No amount of reading the skill would have found it. It took a measurement.

## Changes made off the back of this run

| Change | File | Evidence |
|---|---|---|
| Rule 12 defers to the profile | `concision.md` | connectives finding |
| New "Micro-conventions are voice" section | `protections.md` | all six judges |
| "Aim past clean" and "match micro-conventions first" | `commands/draft.md` | v2 beat v1, 2-0 |
| Six new markers: en dash, connectives, single quotes, numerals, exclamation, self-referential parentheticals | `profiles/rufus-pollock.md` | all six judges |
| Pairwise promoted to primary method | `evals/README.md` | run 2's 7-point swing |
| Judges must receive the brief | `evals/README.md`, `rubric.md` | fabrication gate failure in run 2 |

## Limits

- One case, one author. Everything here could be a fact about this pair of essays.
- The v2-beats-v1 result validates the *instruction*, not the general principle. "Overshoot the texture" was tested once, on one draft, written by the same model that then had it judged.
- Judges were told the more polished piece is often the less faithful one. That is a true instruction that could also bias toward whichever draft looks rougher. Worth a control where the instruction is withheld.
- Nothing here has been checked against the author. That remains [#1](https://github.com/rufuspollock/soundlikeme/issues/1).
