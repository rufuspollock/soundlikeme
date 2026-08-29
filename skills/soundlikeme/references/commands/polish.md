# polish

The main event. Take a draft — the user's own rough text, dictation, or an AI draft of their
ideas — and make it read as though they wrote it well.

Load: [tells.md](../tells.md), [protections.md](../protections.md),
[concision.md](../concision.md), the voice profile,
[self-check.md](../self-check.md). Roughly 8.5k tokens including the router.

With `--deep`, also load [elements-of-style.md](../elements-of-style.md), about 18k more.
Use `--deep` for writing published under the user's name to readers who know their work. If
context is tight, hand the draft to a subagent along with that file and take back the
copyedit.

No profile? Say so, offer `extract`, and ask whether to proceed as `deslop` instead. Do not
invent a voice.

## Order of operations

Voice is the frame, not a coat of paint applied at the end.

1. **Read the whole draft.** Find the core point. If you cannot, ask — do not guess and
   polish around a hole.
2. **Read the profile.** Note the three to five signature moves most relevant to this piece
   and this register, and the markers you will check against.
3. **Establish genre and audience.** They gate half the rules. See
   [protections.md](../protections.md).
4. **Find defects.** Tells, filler, vagueness, tangled sentences, unclear passages. Mark each
   confirmed or protected with a reason.
5. **Repair, smallest edit first.** Only sentences with confirmed defects. Everything else
   byte-for-byte, in its original order and paragraphs.
6. **Second pass** over your own output: tells you introduced, transitions you recycled,
   paragraphs that got flatter, opinions you sanded down.
7. **Run [self-check.md](../self-check.md)**, all thirty items. Fix failures. Once.

## Voice before rules

Where the profile and the concision rules genuinely disagree, the profile wins. If the
author writes long clause-heavy sentences with parenthetical asides, that is the voice, not
a defect — do not chop them into Strunk-approved fragments. If they open with a personal
digression, that is character, not throat-clearing.

Most apparent conflicts are not real. "The profile says the author is discursive" does not
license "in order to".

Match the profile's markers, not just its adjectives. If mean sentence length is 24 words
and your output averages 12, you have written a different person, however clean it reads.

## The most common failure

Rewriting a distinctive rough draft into clean prose with no author in it. The output passes
every tell check, reads well, and is worthless.

Guard against it: after the edit, find the sentences only this person would have written. If
there are none left, you have destroyed the thing you were hired to protect. Revert and try
again with smaller edits.

## Output

The full polished text, then:

```markdown
## What changed

- [The two or three substantive changes, with why]
- [Anything you restructured, and why]
- [Anything you left alone that looked like a defect but was voice]
```

If you could not resolve something — a missing source, an unclear claim, a fact you needed —
ask about it after the text. Never invent it.
