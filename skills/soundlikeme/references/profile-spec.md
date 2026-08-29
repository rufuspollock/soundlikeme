# Voice profile spec

A profile has two halves. The prose half tells a model how to write. The markers half is
what makes the profile testable — numbers you can check an output against, and the thing
that stops a profile being an unfalsifiable vibe.

Target 400-600 words total. A profile that grows past a page stops being loaded properly.

## Frontmatter

```yaml
---
name: <person or organisation>
generated: <date>
samples: <count>
registers: <which kinds of writing the samples cover>
held_out: <pieces deliberately excluded, for eval>
---
```

`held_out` matters. A profile built on everything the author wrote cannot be evaluated —
see `evals/README.md`. Always leave at least one piece out, and record which.

## Sections

### Voice and tone

Two or three sentences. What the reader feels. Register, warmth, distance, confidence.
Say what the writing is *like*, with a comparison if it helps.

### Sentence structure

How sentences are built and how they vary. Clause density. Fragment use. Where the emphasis
lands. Paragraph length and whether it varies.

### Vocabulary

Register tier. Spelling convention. Contraction habit. How technical terms are handled.
Recurring words and phrases the author actually uses — quote them.

### Signature moves

The specific devices, five to ten, each with a real example from the samples. This is the
highest-value section. Be concrete: not "uses analogies" but "reaches for historical
parallels — Gutenberg, tulip mania, LambdaMOO — to deflate present-day hype".

### What this voice does not do

Explicit negative space. Often more useful than the positive description, because it stops
the model reaching for its defaults.

### Markers

Quantitative, checkable, measured from the samples. Approximate is fine; the point is that
an output can be compared against it.

```markdown
## Markers

- Mean sentence length: ~24 words
- Sentence length range: 4-60 words; high variance is characteristic
- Paragraph length: 3-6 sentences, medium-to-long
- Contractions: frequent (isn't, don't, we're)
- First person: "I" throughout, "we" for collective claims
- Spelling: British (sceptical, organise, defence)
- Em dashes: frequent, mid-sentence, for asides
- Italics for emphasis: several per piece, on single words
- Footnotes: 1-3 per piece
- Numbered or lettered sub-lists inside an argument: common
- Numbers and named specifics: moderate density
- Opens with: position stated, then the case
- Closes with: the argument's last concrete point, sometimes a call to reflection
```

Pick markers that discriminate. If a marker would be true of most competent writers, it
does not belong. The test: could you tell this author from another author using only the
markers?

## Building one

1. **Gather 3-5 samples**, varied by register — an essay, a talk, an email, a post. Variety
   lets you separate consistent habits from context-dependent ones. Two thousand words plus
   per sample where possible.
2. **Reject contaminated samples.** Anything the author wrote with AI assistance poisons the
   extraction — you will capture the model's patterns reflected back. Ask.
3. **Hold at least one piece out.** Record it in `held_out`. It becomes an eval case.
4. **Extract patterns, not content.** The profile describes how, never what.
5. **Measure the markers** rather than guessing them. Count sentences in a sample.
6. **Hand it back for editing.** The author will spot a wrong pattern instantly, and the
   model will always assert one or two. This step is not optional.

## Organisational profiles

Same shape, built from multiple authors. Extract only the patterns that appear across all of
them — the shared floor, not any one person's habits. Expect a thinner profile, and expect
the "does not do" section to carry more weight than the signature moves.
