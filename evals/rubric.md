# Rubric

Seven dimensions, scored 1-5 against a reference text.

You are comparing two pieces of writing on **how they are written**, never on whether the
argument is correct or the content complete. The candidate and the reference cover the same
material by construction.

Every score needs an evidence quote from the candidate. A score with no quote is discarded.

Do not try to work out which text is machine-generated. That is not the question, and
guessing at it will distort every score below.

---

## 1. Stance

Does the candidate commit where the reference commits, and hedge where it hedges?

- **5** — Same confidence throughout. States positions as flatly as the reference states
  them; qualifies in the same places, for the same reasons.
- **4** — Mostly matched; one or two places softer or firmer than the reference.
- **3** — Recognizably an opinion, but noticeably more balanced or more strident.
- **2** — Hedged into neutrality, or asserting things the reference treats as open.
- **1** — Refuses to take a position the reference takes, or takes one it does not.

Balanced-on-every-side is the model's default. If the reference argues and the candidate
surveys, this is a 2 at best.

## 2. Argument shape

Does it move through the material the way the reference does?

- **5** — Same structural habit: same opening move, same ordering of claim and support, same
  use of digression, same closing move. Paragraph lengths in the same range.
- **4** — Same overall shape, one section ordered differently.
- **3** — Content in a sensible order, but the reference's characteristic moves are absent —
  its opening gambit, its detours, its way of ending.
- **2** — Default essay shape: introduction, body, conclusion, regardless of what the
  reference does.
- **1** — Structure fights the argument, or imposes a shape the author never uses.

## 3. Rhythm

Sentence-level cadence.

- **5** — Length variance matches. Long sentences where the reference goes long, short where
  it goes short. Same clause density, same fragment use, same punctuation habits.
- **4** — Close; slightly more uniform or slightly more varied.
- **3** — Readable, but flatter than the reference. Fewer long sentences, or fewer short ones.
- **2** — Metronomic: most sentences the same length. Either uniformly mid-length, or the
  clipped staccato of over-editing.
- **1** — Cadence actively wrong for this author.

Both failure directions score low. Uniformly short is not an improvement on uniformly medium.

## 4. Lexicon

Word choice and surface conventions.

- **5** — Same register tier, spelling convention, contraction rate, and handling of technical
  terms. Uses the reference author's recurring words where they fit.
- **4** — Close; a few words above or below the reference's register.
- **3** — Broadly right register, but generic. None of the author's characteristic vocabulary.
- **2** — Wrong register: too formal, too casual, or reaching for words the reference never
  uses.
- **1** — Reads as a different person's vocabulary entirely, or wrong spelling convention
  throughout.

## 5. Signature moves

The devices the reference author reaches for.

- **5** — Uses the reference's characteristic devices, at roughly its rate, in service of the
  argument.
- **4** — Uses most of them; one absent or one slightly overused.
- **3** — One or two present, mechanically. Deployed because they are on a list rather than
  because the sentence wanted them.
- **2** — Absent. Competent prose with no fingerprints.
- **1** — Parody: devices piled on far past the reference's rate.

Overuse scores worse than absence. A piece with three historical analogies where the author
uses one reads as impersonation, and impersonation is more obvious than blankness.

## 6. Concreteness

Density of specifics.

- **5** — Same density of names, numbers, dates, examples, and mechanisms as the reference.
  Claims are supported the way the reference supports them.
- **4** — Slightly thinner, but nothing important hand-waved.
- **3** — General where the reference is specific in one or two places.
- **2** — Mostly abstract. Claims stated without the supporting specifics the reference gives.
- **1** — Vague throughout, **or** contains specifics not present in the reference.

Invented specifics score 1. A fabricated number is a worse failure than a missing one, and it
does not matter how well it fits.

## 7. Tells

AI writing patterns.

- **5** — None.
- **4** — One cluster-tier pattern, arguably deliberate.
- **3** — A few cluster-tier patterns.
- **2** — One hard tell, or heavy clustering.
- **1** — Multiple hard tells.

**A score of 1 here fails the run regardless of the other six dimensions.**

Hard tells: "delve", "tapestry", "it's important to note", "in today's world", binary
contrast ("not X, it's Y"), self-answered rhetorical question, fake-profound closer,
summary-recap ending, weasel attribution, importance puffery, superficial `-ing` analysis,
chatbot residue.

Only count a pattern if it is not in the reference. If the author genuinely opens with a
rhetorical question, that is voice.

---

## Output format

```markdown
| Dimension | Score | Evidence |
|---|---|---|
| Stance | N | "quote" — one line of reasoning |
| Argument shape | N | "quote" — |
| Rhythm | N | "quote" — |
| Lexicon | N | "quote" — |
| Signature moves | N | "quote" — |
| Concreteness | N | "quote" — |
| Tells | N | "quote" or "none found" — |

Closest match: [where the candidate is nearest the reference]
Furthest: [where it is furthest, and what specifically is missing]
```

No overall score. The per-dimension breakdown is the product; averaging it away discards
the only actionable information in the run.
