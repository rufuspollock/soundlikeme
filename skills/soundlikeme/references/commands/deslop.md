# deslop

The cheap pass. Remove AI tells and nothing else. No profile, no concision rewrite, no restructuring.

Load: [tells.md](../tells.md), [protections.md](../protections.md). Roughly 4.5k tokens including the router.

Use this when there is no voice profile, when the text is not the user's own writing, or when they want the floor rather than the full treatment.

## What this pass does

Hard tells only, plus cluster tells that actually clustered. Fix them in place with the smallest repair.

## What this pass does not do

- Restructure, reorder, or reparagraph
- Apply concision rules beyond what removing a tell requires
- Change the register, the level of formality, or the opinion strength
- Improve anything. A sentence that is merely mediocre stays mediocre.

If the text needs more than tell-removal, say so and offer `polish`. Do not quietly upgrade the job.

## Procedure

1. Read the whole thing before editing. Establish genre and audience.
2. List confirmed findings. For each, check [protections.md](../protections.md) and mark it confirmed or protected, with a reason.
3. Repair every confirmed span with the smallest edit. Copy everything else byte-for-byte, in its original order and paragraphs.
4. Second pass over your own output for tells the first pass introduced: recycled transitions, new staccato rhythm, a paragraph that got flatter.
5. Run [self-check.md](../self-check.md) items 1-17 and 23-27. Fix failures.

With no confirmed findings, return the input exactly and say so. A no-op is a valid and common result.

## Output

The cleaned text, then:

```markdown
## What changed

- [pattern] → [what you did], N instances
- Left alone: [anything notable you protected, and why]
```

Keep the note to a few lines. If the user asked for the text only, give the text only.
