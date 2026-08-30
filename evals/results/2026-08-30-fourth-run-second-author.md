# Eval run: 2026-08-30 fourth run, second author

Skill version: 0.2 · Cases: `copyright-monopoly` (Rufus Pollock), `mean-people-fail` (Paul Graham, new) · Method: forced-choice pairwise, fresh subagents, every comparison run in both orders with a different judge each way.

Three things happened in this run. A second author was added, the ablation was re-run against him, and run 3's judge instruction was tested for bias. All three came back positive, which is the least interesting of the possible outcomes and has to be read with that in mind.

## 1. The ablation holds on a second author

The reason for doing this at all: everything the first three runs found could have been a fact about one pair of Rufus Pollock essays rather than about voice-matching. So: a Paul Graham profile built from seven essays (2014-2022), two more held out, and one of those held-out pieces — *Mean People Fail*, November 2014 — turned into an eval case.

| Comparison | Order 1 | Order 2 | Result |
|---|---|---|---|
| Profile-on vs no-profile floor, `mean-people-fail` | profile-on (high) | profile-on (high) | **profile-on, 2-0** |

Order-invariant, both judges high confidence.

**The ablation now stands at 6-0 to profile-on across two authors.** That is the result this run existed to get, and it is the first evidence in the repo that the profile mechanism generalizes past the person it was built for.

### The methodology was tightened first

Run 3's candidate drafts were written inside the same session that had read the reference. That is a contamination this repo had not named. It was fixed here before the comparison ran:

- Both drafters were fresh subagents that had never seen the reference and were forbidden from going to look for it. One got the profile, the `draft` command, and the tells/protections/concision/self-check references; the other got the brief and an explicit instruction to write as itself, with no profile and no style guide.
- Only the brief was shared.

A first profile-on draft written by the session author — who *had* read the reference — is kept in this run's directory as `candidate-contaminated.md` and was **not judged**. Its marker profile matches the reference almost exactly (mean sentence length 14.4 against 14.3, four footnotes against four, 16 paragraphs against 17), which is precisely why it cannot be used: those numbers measure exposure, not the skill.

### What the judges quoted

The pattern from run 3 repeated on a different author, with different conventions:

- **The opening apparatus.** Both judges led with it. The profile-on draft opens on a bare `November 2014` and then a flat first-person observation. The floor invented a title (`Why Meanness Stopped Working`) and a hook ("Here is something I've noticed and can't stop noticing"). The reference has no title inside the text and no hook.
- **Note format.** A bare `Notes` heading with `[1]` markers, against the floor's markdown footnote syntax, stacked callouts, horizontal rules and an italicized colophon. One judge: "decoration the reference has none of."
- **Spelling convention.** The floor wrote "practised"; the reference is American throughout. Cited by one judge unprompted.
- **Whether an image gets unpacked.** The reference states its baggage-scanner comparison in one sentence and moves on. The profile-on draft did the same. The floor extended it into a two-clause conceit and then explained it. Both judges named this.
- **Question-then-answer.** "Why would that be? I can think of three reasons" against the reference's "Why? I think there are several reasons." Cited by both.

Not one judge picked on argument, structure at the section level, or quality of prose. Six of six judgments across two authors have now turned on mechanical surface habits.

### Fabrication

The profile-on draft: clean, per both judges. The floor invented one specific — a title the brief does not supply. Milder than the floor's four inventions in run 3, and both judges flagged it independently.

## 2. Judge bias: the run 3 instruction was not driving the result

Run 3's judges were told "the more polished piece is often the less faithful one". True, and possibly enough on its own to tilt a judge toward whichever draft looks rougher. The run 3 write-up flagged this as an open weakness.

Re-ran the `copyright-monopoly` profile-on-vs-floor comparison on the same two texts with that sentence removed and "judge voice, not quality" kept:

| Comparison | Order 1 | Order 2 | Result |
|---|---|---|---|
| Profile-on v1 vs no-profile floor, instruction removed | profile-on (high) | profile-on (high) | **profile-on, 2-0** |

**The verdict holds.** Same winner, both orders, both high confidence, same evidence classes cited — the colophon, the footnote apparatus, question-shaped section headings, single scare quotes, "whether this actually true is unclear to me" as a hedge rather than an assertion. Run 3's ablation result does not depend on that sentence.

One judge, without being told to prefer roughness, volunteered the opposite as a point *against* its own winner: "text-1 is the more polished piece of writing." That is the shape you want — the judge noticing polish and declining to reward it.

The bias-removed prompt is now the standard. It is one sentence shorter and one confound lighter, and the finding it was suspected of producing survives without it.

## 3. False-positive and false-negative rates, measured for the first time

`protections.md` exists to control the false-positive rate and nobody had ever measured it. Twelve fixtures, `deslop` run against each by a separate fresh subagent — one agent per fixture, because an agent that has just cleaned a slopped blog post arrives at a protected text primed to edit.

| Family | Fixtures | Spans | Result |
|---|---|---|---|
| Protection (no-op is correct) | 7 | 61 protected spans | **0 altered. False-positive rate 0%.** Every output byte-identical to its input |
| Detection (slop must be repaired) | 4 | 66 planted tells | **63 repaired. Detection rate 95%** |
| Mixed (both, same document) | 1 | 8 must-fix, 15 must-not-touch | **8 of 8 repaired, 0 of 15 touched** |

### What the protection pass got right

The two fixtures built as regression tests both held.

`protection/human-prose` carries the Pollock micro-conventions — "Furthermore" and "Nevertheless" as structural joints, spaced dashes, single scare quotes, "over 3 years", one exclamation mark. Untouched, and the agent's own note names each one as a protection rather than as something it did not notice.

`protection/self-answered-questions` is the interesting one. The self-answered rhetorical question is listed in `tells.md` as a **hard** tell, which fires on sight, one instance being enough. It is also Paul Graham's principal structural device, at roughly one question every two hundred words. This is the same class of bug run 3 found in `concision.md` — a general rule about clean prose eating a specific author's real signal — and the fixture is a live test of whether `protections.md` catches it. It did: no-op, all four questions intact.

### The three misses, all in one fixture

All three false negatives are in `detection/corporate-density`:

- `The data suggests` survived. The agent protected it as genre-appropriate. It is the literal example given for inanimate agency in `tells.md`, in a document saturated enough for a cluster tell to fire.
- `the system understands user intent` became `the system infers user intent`. The verb changed and the inanimate agency did not.
- `stakeholders` survived — but this one is the fixture's fault, not the skill's. "stakeholders" is not in `tells.md` at all; it is in the Pollock profile's "does not do" list, and I wrote it into the expectations from memory. Excluding it, detection is 63 of 65, 97%.

### Do not read the 0% as a 0% rate

Seven of seven protection fixtures returned byte-identical output, which is the cleanest possible result and should be treated as suspicious rather than reassuring. Three explanations, and this run cannot separate them:

1. `protections.md` genuinely works.
2. The fixtures are too easy. Each one is *uniformly* protected, so an agent can pass it by adopting a disposition — "this is scientific writing, hands off" — without ever discriminating span by span.
3. The agent was told it was running `deslop`, and `deslop.md` presses hard on no-op being a valid and common result. The instruction may be doing the work rather than the protections.

Explanation 2 is the one worth acting on, and `protection/mixed` was written for it mid-run: real slop and protected spans in the same document, so no disposition can pass it. Eight hard tells to remove, and fifteen protected spans sitting between them — the author's own spaced dashes and "Furthermore"/"Nevertheless" joints, a passive-voice methods paragraph, a vendor blockquote that is pure slop and therefore evidence, contract wording, a code identifier, and one exclamation mark.

It passed: all eight repaired, none of the fifteen touched. The vendor blockquote is the striking one — "delve into the intricate tapestry of operational excellence" survived intact three paragraphs after the same pass deleted "In today's fast-paced digital landscape" from the opening line. That is discrimination, not disposition, and it is the strongest evidence in the run that `protections.md` is doing real work.

One fixture is one fixture. But it removes explanation 2 as the obvious reading of the 0%, which is what it was for.

### A pattern worth naming: deslop deletes where it should repair

Not a false positive by the fixture's own scoring, but visible in two detection outputs and worth recording.

- `detection/social-post` lost its final line entirely. The tell there is a binary contrast, and `tells.md` says to state Y — rewrite it as the claim — not to cut the sentence. The post's payoff went with it.
- `detection/saturated-blog` lost its whole closing paragraph rather than the tells inside it, and dropped a claim along with the weasel attribution that carried it.

`deslop.md` says the pass does not restructure and does not improve. Deleting a sentence because it contains a repairable pattern is a third thing, and neither the command file nor `protections.md` currently names it. Filed as [#7](https://github.com/rufuspollock/soundlikeme/issues/7).

## Limits

- Two authors is better than one and is not many. Both are male anglophone essayists writing argumentative nonfiction, both profiles were built by the same model, and both cases are single pieces.
- The Paul Graham profile has not been checked by Paul Graham, and never will be. Everything the Pollock profile gets from [#1](https://github.com/rufuspollock/soundlikeme/issues/1) — an author saying whether the profile is right — is structurally unavailable here. A second author buys generalization and cannot buy ground truth.
- The judges, the drafters and the profile-builder are all the same model family. A judge that shares a prior with the drafter may be rewarding the drafter's habits rather than the author's.
- The detection fixtures were written by the same person who wrote the expectations, one day after reading `tells.md`. They test the list against itself. Real slopped text found in the wild would be a better fixture and nobody has collected any.
- The floor draft ran long — 1,396 words against the reference's 1,141 and the profile-on draft's 1,265. Length is not what the judges cited, but it is an uncontrolled difference.
