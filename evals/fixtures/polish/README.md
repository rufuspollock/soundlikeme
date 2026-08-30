# Polish fixtures

Reconstruction measures `draft`. It cannot measure `polish`, whose input is the user's own rough text — there is no way to simulate that from a published piece, because the published piece is the output.

The shape that works instead is **degrade and restore**. Take a real piece by an author with a profile. Degrade it into the bland, evenly-paced, signposted prose a chatbot produces from the same ideas, keeping every claim and specific so the content is recoverable. Freeze that as the fixture input. Then run `polish` on it with the author's profile loaded, and ask how much of the original came back.

```
evals/fixtures/polish/<id>/
  original.md    the real published piece. Vendored, verbatim, never edited
  degraded.md    the fixture input. Frozen once written
  meta.md        provenance, and what the degradation pass did
```

## Writing the degradation

Do it in a separate pass, with a subagent that has only the original. Two rules:

- **Keep everything.** Same claims, same examples, same specifics, same order, nothing added, nothing dropped. If content is lost, the measurement stops being about voice.
- **Lose only the voice.** Even out the sentence lengths, raise the diction, signpost the transitions, make every paragraph equally tidy. Add the ordinary tells — a little puffery, a hedged weasel construction, a summarising final paragraph, a section heading the original did not have.

Aim for bland and plausible, not parody. A degradation carrying twelve hard tells measures `deslop`, which is already measured; the interesting fixture is the one where nothing is obviously wrong and the piece still sounds like nobody.

Then freeze it. A degradation edited between runs invalidates comparison with earlier ones, the same way a brief does.

## Scoring

Pairwise, for the same reason the reconstruction eval is pairwise: absolute scoring on this scale is noise.

Give a fresh judge `original.md` as the reference and the two texts — `degraded.md` and the polished output — as `text-1` and `text-2`, without saying which is which. Ask which reads more like the same person who wrote the reference. Both orders, a different judge each way.

**If the polished version does not beat the degraded input, `polish` is not doing its job.** That is the whole test, and it is a floor rather than a target: beating a deliberately de-voiced text is the least that should happen.

The stronger question, once the floor is cleared, is how the polished version does against the original itself — how much of the gap closed. That needs a third comparison and has not been run.

## Note on what this cannot measure

The real `polish` input is a person's rough draft: their structure, their thinking, their bad sentences. A degraded published piece has the author's structure already in it, because the degradation preserved it. So this measures whether `polish` restores surface voice to text that already has the right bones. It does not measure whether `polish` improves genuinely rough writing without flattening it, which is the thing the command is actually for, and which probably cannot be measured without the author in the loop.
