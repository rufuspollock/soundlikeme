# Fixtures

The other half of the objective function. The reconstruction eval in [../README.md](../README.md) measures whether `draft` sounds like the author. These measure whether the editing commands break things they should have left alone, and catch things they should have caught.

Two families, and they fail in opposite directions.

- **Protection.** Text where the right answer is a no-op, or something very close to one. Already-human prose, scientific writing where the passive is correct, quoted material including quoted slop, code and config, precise legal wording, banned words used literally, and one author's genuine structural habit that the tell list marks as a hard tell. **Any edit to a listed protected span is a false positive**, regardless of how much else the pass got right.
- **Detection.** Text that is genuinely slopped. **Any listed tell that survives is a false negative.**

These are the rates `protections.md` exists to control, and they were unmeasured until 2026-08-30.

## Layout

```
evals/fixtures/
  protection/<id>/input.md     the text to run the command against
  protection/<id>/expect.md     why it is protected, and the exact spans that must survive
  detection/<id>/input.md
  detection/<id>/expect.md      the exact tells planted, and what correct repair looks like
```

`expect.md` is scoring key, not input. Never show it to the agent under test.

## Running

Same shape as the reconstruction eval: markdown, no scripts, subagents do the work.

1. For each fixture, dispatch a fresh subagent with `references/commands/deslop.md`, `references/tells.md`, `references/protections.md`, `references/self-check.md`, and `input.md`. Nothing else — not `expect.md`, not the rest of the repo, not the other fixtures. One agent per fixture: an agent that has just cleaned a slopped blog post arrives at a protection fixture primed to edit, and that priming is itself a measurable effect you do not want inside the measurement.
2. Have it write the full output text and its change note.
3. Diff output against input.
4. Score:

   ```
   false positive rate = protected spans altered / protected spans listed
   detection rate      = planted tells repaired / planted tells listed
   ```

Report both. A pass that scores well on one and badly on the other has not been tuned, it has been tilted.

## What counts as an edit

For protection fixtures, byte-identical output is the clean pass. Judge anything else against the listed spans: a changed span is a false positive, and a reflowed blank line is not. Where the command is `polish` rather than `deslop`, small edits outside the listed spans are permitted by the genre — the listed spans still are not.

## Adding a fixture

`tells.md` says every new tell ships with a protection case in `protections.md`. The stronger version of that rule: every new tell also ships with a fixture here, on whichever side it belongs. A tell nobody has measured a false-positive rate for is a guess.

Protection fixtures are worth more than detection fixtures. Detection is the commodity half of this repo — the banlist has converged across every project in [../../docs/prior-art.md](../../docs/prior-art.md) — and false positives are what actually ruin someone's writing.

## Note on the protection fixtures that mirror real findings

Two of them are regression tests for things the eval has already caught:

- `protection/human-prose` carries the Rufus Pollock micro-conventions — "Furthermore" and "Nevertheless" as structural joints, spaced dashes, single scare quotes, numerals for small numbers, one exclamation mark. Run 3 found `concision.md` rule 12 stripping exactly these.
- `protection/self-answered-questions` is built around the self-answered rhetorical question, which `tells.md` lists as a **hard** tell — one instance fires. It is the principal structural device of the second author in the repo, at roughly one question every two hundred words. If a pass fires on it, the skill has the same bug it had in August, on a different author.
