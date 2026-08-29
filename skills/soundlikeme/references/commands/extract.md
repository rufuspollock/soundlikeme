# extract

Build a voice profile from writing samples.

Load: [profile-spec.md](../profile-spec.md).

## Gather

Ask for 3-5 samples, varied by register — an essay, a talk or transcript, an email, a short post. Variety is what lets you separate consistent habits from context-dependent ones. Aim for 2,000+ words each where the author has them. URLs, files, or pasted text all work.

Two questions before you start, and they matter more than sample count:

1. **"Did you use AI to write or edit any of these?"** Contaminated samples poison the extraction — you capture the model's patterns reflected back and then flatter the user by telling them it is their voice. Drop anything AI-assisted.
2. **"Are these your best or most characteristic writing?"** Not their most recent, not their most professional. The pieces that sound most like them.

## Hold one out

Before extracting, set aside at least one piece. Record it in the profile's `held_out` frontmatter. It becomes an eval case — see `evals/README.md`. Without a held-out piece there is no way to check whether the profile does anything, and a profile nobody can check is decoration.

## Extract

Read every sample fully. Then work through the sections in [profile-spec.md](../profile-spec.md).

- **Describe how, never what.** The profile must carry no topic, opinion, or content from the samples.
- **Quote real examples** for every signature move. A move without an example is an assertion, and the ones you assert are usually the ones you invented.
- **Measure the markers.** Actually count. Take a 400-word stretch from two samples, count sentences and words, count contractions, count em dashes. Estimated markers are the part of the profile most likely to be wrong.
- **Prefer discriminating patterns.** Drop anything that would be true of most competent writers. The test: could someone tell this author from another using only this profile?
- **Note what the voice does not do.** Often the most useful section, because it blocks the model's defaults.

## Hand it back

Present the profile and say plainly: *some of this will be wrong, and you will spot it immediately.* Ask specifically about the signature moves and the "does not do" section, and about anything you were unsure of. Models reliably hallucinate one or two patterns and miss the most important one.

Then offer `calibrate` to sharpen it against real examples.

## Save

Write to `profiles/<name>.md` — kebab-case, e.g. `rufus-pollock.md`. Tell the user where it went and how to point commands at it.

## Organizations

Same process with samples from several authors. Extract only the patterns present across all of them. Expect a thinner profile, and expect the "does not do" section to do most of the work — shared voice is usually defined more by what everyone avoids than by what anyone does.
