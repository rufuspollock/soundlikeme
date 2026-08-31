---
title: "humanizer vs soundlikeme: a worked comparison"
date: 2026-08-31
---

# humanizer vs soundlikeme: a worked comparison

Prompted by the question "is it worth keeping soundlikeme, or just use `blader/humanizer`?" (~39k stars, the category default — see `prior-art.md`). This records the test and the conclusion so a future session does not have to redo it.

## Method

Not an automated run. humanizer was not installed; its published `SKILL.md` rules were applied by hand to one document, and soundlikeme's `polish` command was applied to the same document following its reference files (`tells.md`, `protections.md`, `concision.md`, the `rufus-pollock.md` profile, `self-check.md`).

Test document: `~/src/flowershow/wayofmarkdown/why.md` — an argument essay under Rufus's byline (updated July 2026), chosen because it is prose-heavy and visibly AI-polished: bold-label bullets, aphoristic kickers, metadiscourse.

## What both catch identically

Every hard tell in the piece, removed in nearly the same words by both approaches:

- "Here's a secret:" — faux-reveal setup
- "Platforms die, formats rot — plain text doesn't" — binary-contrast kicker
- "That ubiquity is itself the argument: … an investment the whole ecosystem keeps paying interest on" — interpretive metadiscourse plus a strained metaphor
- "In other words:" — meta-joiner that then restates
- "markdown is just the substrate; the value is in what's built on it" — restatement of the section
- "Simplicity, portability, ownership — demonstrated, not proclaimed" — tricolon plus contrast kicker
- "your notes become programmable" — em-dash-then-three-word payoff

On slop removal alone the two outputs are interchangeable.

## Where they diverge

Every difference traces to the stored profile. Without `rufus-pollock.md`, soundlikeme's `deslop` and humanizer produce the same text.

| Passage | humanizer | soundlikeme + Rufus profile |
|---|---|---|
| "files from twenty years ago … perfectly in fifty" | left as words | numerals: "20 years ago … perfectly in 50" (profile marker) |
| em dashes throughout (`—`) | "reduce unnecessary dashes", no direction on which mark | flips to spaced en dash `– `; the profile names this the clearest single voice signal |
| bold-label bullets in "Why this wins" | flags "lists with bold labels on every item", likely strips or converts to prose | minimum-effective-edit: no named defect, and the structure mirrors `manifesto.md`, so left alone |
| "Want the declaration rather than the argument?" / "Ready?" | "announcing the next point", likely flattened to statements | kept: direct address to the reader is a listed Rufus signature move, and both lines deliberately mirror the manifesto's closers |

The real delta on this document: two micro-conventions (numerals, dash style) plus a more conservative posture on structure and reader-facing questions. Small, and contingent on a measured profile already existing.

## Conclusion

Keep soundlikeme, but narrow its scope.

- **humanizer is the better banlist.** Maintained, large, tracks [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). `prior-art.md` §2 already concludes the banlist is a commodity. Maintaining `tells.md` in parallel is wasted effort — `deslop` could vendor or defer to humanizer's pattern list and lose nothing on this evidence.
- **soundlikeme's defensible core is what humanizer structurally cannot do:** a stored, corpus-derived, measured voice profile with held-out pieces and markers, and the eval harness that showed the profile beats no-profile 4-0 pairwise. humanizer's voice matching is "paste a sample, match its sentence length and punctuation" — no persistence, no measurement, no proof.
- For "not obviously AI", humanizer alone is enough and lower-maintenance. For "sounds like this specific person, and I can measure whether it does", that is the part that is soundlikeme's.

## Follow-on work this implies

- Consider replacing `references/tells.md` with a vendored copy of humanizer's pattern list (MIT), keeping `protections.md` and the profile machinery as the differentiator. This would cut maintenance and stop the two banlists drifting.
- If that vendoring happens, re-run the pairwise eval to confirm the swap does not regress voice fidelity.
