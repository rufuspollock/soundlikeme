---
date: 2026-08-29
title: One skill, seven commands, and a way to tell if it works
promote: true
---

The two skills are now one. `soundlikeme` is a thin router over per-command reference files, so asking it to tidy a Slack message no longer drags in eighteen thousand tokens of Strunk. Seven commands: `audit` reports AI tells without touching anything, `deslop` is the cheap pass, `polish` is the full one, `draft` writes from notes in your voice, `extract` builds a voice profile from your samples, `calibrate` sharpens it with an A/B quiz, and `eval` scores the whole thing.

Behind that sit two rules doing most of the work, both taken from a survey of the eight serious projects already in this space. Minimum effective edit: change only spans with a named defect, copy the rest byte-for-byte, return the input unchanged when there is nothing wrong with it. And a match is not a license: finding a banned word doesn't authorize an edit, because a naive banlist produces clipped, opinion-flattened prose that is its own detectable style and a worse outcome than the slop it replaced.

The more interesting half is `evals/`. Everything else in a skill like this is assertion — you cannot tell whether the banlist growing made the writing better or just flatter. So: take a piece you published that your profile was not built from, strip it to a bulleted brief, have the skill draft from the brief, and compare the draft to what you actually wrote. Content is held constant, so the only variable is voice. A bare score means nothing, so two runs bracket it — a ceiling (a different real piece of yours, scored the same way) and a floor (the same brief with no profile). What gets reported is the fraction of that headroom closed, per dimension.

Plus the control group nobody runs: same brief, profile on versus profile off. If the profile doesn't beat the floor, it is decoration, and that is worth knowing.

One case is seeded, from a 2011 post outside the profile's samples. The harness has not been run yet.

See: [the skill](https://github.com/rufuspollock/soundlikeme/tree/main/skills/soundlikeme), [the eval protocol](https://github.com/rufuspollock/soundlikeme/blob/main/evals/README.md), and [the survey of prior art](https://github.com/rufuspollock/soundlikeme/blob/main/docs/prior-art.md).
