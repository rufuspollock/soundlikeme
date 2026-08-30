# Polish fixture: being-a-noob

| Field | Value |
|---|---|
| Author | Paul Graham |
| Profile | `skills/soundlikeme/profiles/paul-graham.md` |
| Source | https://www.paulgraham.com/noob.html |
| Published | January 2020 |
| Original length | ~390 words |
| Degraded length | ~470 words |
| In profile samples? | No — recorded in the profile's `held_out` |
| Degradation frozen | 2026-08-30 |

## Why this piece

Short, one clean argument, and it turns on exactly the habits the Paul Graham profile claims to capture: a flat first sentence, a question posed and answered in the next breath, no section headers at all, and a closing that lands on the claim rather than summarising it. A degradation has somewhere obvious to go — add the headers, smooth the questions into statements, append a conclusion — which makes it visible whether `polish` puts any of it back.

## What the degradation did

Written by a separate agent that had only the original, with instructions to keep every claim, example and specific and to lose only the voice. What came back:

- Three section headers the original does not have, including one called `Conclusion`
- Contractions expanded throughout ("I am" for "I'm", "do not" for "don't")
- Sentence lengths pushed toward the middle, and the short abrupt ones padded ("Now that I'm old, I know this isn't true" became "Now that I am older myself, I can report that this assumption was mistaken")
- Signposted transitions added: "In fact", "Consider a straightforward example", "This raises an important question", "The lesson, then, is worth stating plainly"
- Hedges stacked onto flat claims: "arguably the correct thing", "appears to be inversely correlated", "I would suggest that"
- Diction raised: "relocate" for "move", "traveling in a new country" for "visiting some new country", "determine" for "figure out"
- A tricolon and a summarising final paragraph added

Content survived intact — Farawavia, hunter-gatherers, cryptocurrency, the hunger analogy, the local/global inversion are all still there. That is what makes the comparison about voice.

## Known weaknesses

- 390 words is short. There is less room for length-variance and paragraph-shape signals than in a full essay.
- The degradation and the polish were produced by the same model family, so `polish` may be unusually good at reversing this particular degradation — it is undoing its own defaults. A degradation written by a different model, or found in the wild, would be a harder test.
- The original's structure survives inside the degraded text, so this measures surface restoration and not much else. See the note at the end of [../README.md](../README.md).
