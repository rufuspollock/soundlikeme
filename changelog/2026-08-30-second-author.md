---
date: 2026-08-30
title: A second author, and the first false-positive numbers
promote: true
---

Everything the eval had found so far could have been a fact about two Rufus Pollock essays rather than about voice-matching. So today it got a second author: a Paul Graham profile built from seven essays, three held out, and one of those turned into a new eval case.

The ablation holds. Profile-on beat the no-profile floor 2-0 on the new case, both orders, both judges at high confidence, which puts it at 6-0 across two authors. The judges picked winners the same way they did last time — on the mechanical surface habits, not the argument. Where Pollock's tell was spaced en dashes and "Furthermore" as a structural joint, Graham's is a bare date line, a plain `Notes` heading, American spelling, and the discipline of stating an analogy once and moving on rather than unpacking it. Six of six judgments across two authors have now turned on that layer and none on rhetoric.

Two methodology problems got closed on the way. Run 3's drafts were written by a session that had already read the reference, a contamination the repo had not named; both drafters this time were fresh agents that had never seen it. And run 3's judges were told "the more polished piece is often the less faithful one", which is true but might have been doing the work on its own. Removing the sentence changed nothing — same winner, both orders, high confidence — so it is gone from the standard prompt.

The bigger gap was that `protections.md` exists to control the false-positive rate and nobody had ever measured it. Thirteen fixtures now do: text where the right answer is to change almost nothing, and text that is genuinely slopped. **Zero false positives on 76 protected spans, 63 of 66 planted tells caught.** The one that matters is the fixture where slop and protected spans share a document, so no blanket disposition can pass it — a vendor blockquote of pure marketing slop survived intact three paragraphs after the same pass deleted "In today's fast-paced digital landscape" from the opening line.

A clean sheet is the least interesting outcome available, and the two places the run found something wrong are worth more than the wins. `deslop` deletes where the rules ask it to repair: three individually defensible rules stack into a pass that removed a post's payoff along with the binary contrast inside it. And `polish`, measured for the first time by degrading a real essay and restoring it, put back the contractions and stripped the invented section headers but still could not stop itself softening the author's abrupt ending — the original closes "the more you feel like a noob, the better", the restored version closes "the better off you are likely to be". Both judges named it without being asked. The profile lists that habit explicitly, and the profile lost.

See: [the fourth run](https://github.com/rufuspollock/soundlikeme/blob/main/evals/results/2026-08-30-fourth-run-second-author.md) and [the fixtures](https://github.com/rufuspollock/soundlikeme/blob/main/evals/fixtures/README.md).
