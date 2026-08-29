---
date: 2026-08-29
title: The eval caught the skill deleting a voice marker
promote: true
---

Three runs today, each correcting the last, and the third one paid for the whole harness.

The first run scored a profile-written draft at 31 against a no-profile control at 24 and called that a win. The second run re-scored the identical control text and got 31 — a seven-point swing on byte-identical input, wider than the effect being measured. So the first result was retracted: not a finding, just a draw from a wide distribution. Absolute one-to-five scoring by a single judge cannot resolve anything at this scale.

The third run switched to forced-choice pairwise — two drafts, one question, run twice with the order swapped and a different judge each way. Clean and order-invariant. Profile-on beats no-profile 4-0, every judge at high confidence. The ablation holds after all, by a method that survives the noise. Giving judges the source brief as well as the reference also fixed the fabrication check: all four who saw the control flagged its invented specifics unprompted, and both profile-written drafts came back clean.

The useful part is what the judges picked on. Not rhetoric — micro-conventions. Spaced en dashes rather than em dashes, cited by three judges as the clearest single signal. Numerals rather than words. Single rather than double scare quotes. And one exclamation mark, which one draft kept and the other flattened to a period: *"the reference author's one burst of enthusiasm is exactly the tic a polisher removes."*

Which turned up a real bug in the skill. Its concision rules banned "Furthermore" and "Moreover" as filler transitions. The author being imitated uses both as genuine structural joints, and judges penalised the drafts that dropped them. The concision layer was deleting a voice signal, in a project whose entire premise is that voice survives the cleanup. Rule 12 now defers to the profile, and there is a new "Micro-conventions are voice" section covering dashes, connectives, quote marks, numerals and exclamations — each entry carrying the evidence that produced it.

No amount of re-reading the skill would have found that. It took a measurement.

See: [the pairwise run](https://github.com/rufuspollock/soundlikeme/blob/main/evals/results/2026-08-29-third-run-pairwise.md) and [what changed in the skill](https://github.com/rufuspollock/soundlikeme/blob/main/skills/soundlikeme/references/protections.md).
