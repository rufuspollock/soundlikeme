# Protections

The hard part of this skill is not finding tells. It is not firing on the ones that are fine.

A naive banlist produces prose that is clipped, evenly short, stripped of opinion and digression, and instantly recognizable as machine-edited. That is a worse outcome than the slop it replaced, because the slop at least left the author's structure intact.

## The decision rule

> Return a finding only when the contextual defect is clearer than the value of leaving it
> alone.

A scanner match alone never authorizes an edit. Neither does a rhythm score, a cadence judgment, or a general sense that a passage is "AI-ish". Name the defect or leave it.

## Never touch

Edits to these are failures regardless of what else the pass achieved.

- **Quoted material.** Anything inside quotation marks, blockquotes, or attributed to another person. Including their slop.
- **Code, config, and data.** Code blocks, inline code, YAML, JSON, tables, URLs, file paths, API and product names, regulatory terms, exact UI labels, command syntax.
- **Facts and quantities.** Numbers, dates, names, units, citations, measurements. Never smooth a specific figure into a general claim.
- **Scope and force in rules.** "never", "must", "all", "only" in safety, security, legal, medical, or technical instructions. These are load-bearing. Preserve them exactly.
- **Attribution and uncertainty.** If the author attributed a claim or hedged it, keep the attribution and keep the hedge. Confidence you added is a fabrication.
- **Headings the user set**, unless the task is to fix headings.

## Never add

No claims, statistics, examples, sources, anecdotes, opinions, conclusions, or personality that were not in the input. If concreteness is missing, say so — do not invent a number to satisfy the concreteness rule. This failure mode is common and severe.

## Genre exemptions

Rules that are correct in a blog post are wrong elsewhere. Establish the genre before editing.

**Academic and scientific.** Passive voice is correct in a methods section. Hedging is epistemic honesty, not weakness. Domain terminology is precision, not jargon — "weighted interval score" is the right phrase. Formal register is required. What still applies: business buzzwords and AI vocabulary tells leaking in.

**Legal, medical, financial, regulatory.** Do not make it more opinionated, more direct, or shorter than the genre allows. Nuance here protects accuracy. Prefer a no-op.

**Reference documentation and specs.** Neutrality is the job. Repetition is a feature — the reader arrives mid-document. Parallel structure across sections is correct, not robotic.

**Email and messages.** Warmth, greetings, and sign-offs are not filler. "Hope you're well" in a note to a colleague is a social move, not slop.

**Marketing and landing pages.** Some inflation is the genre. The line is claims that could be about any product. Judge by the portability test below, not by adjective count.

**Talks and spoken text.** Repetition, signposting, and direct address are how listeners follow an argument without a scrollbar. Do not edit a transcript to read like an essay.

**Social posts.** Short, punchy, and fragmented is the medium. Do not impose paragraph prose.

## When a tell is not a tell

Protect a match when it is:

- **Literal.** "delve" in a piece about mining. "tapestry" about a tapestry.
- **Quoted or attributed.** Someone else's words.
- **Domain-valid.** "leverage" in finance. "robust" in statistics. "vital" in medicine.
- **Accurately caveated.** The surrounding text already supplies the mechanism, definition, measure, or action the phrase would otherwise hand-wave. "Game-changer" is vague; "game-changer: it cut review time from thirty minutes to eight" is not.
- **Genre-natural.** See above.
- **The author's actual habit.** If the voice profile names it, the profile wins. Some writers really do use em dashes constantly, or open with a rhetorical question, or end on a call to reflection. Stripping those is the failure this whole skill exists to prevent.

A noun referent is not enough on its own. "A game-changer for the product" is still vague unless the text says what changes.

## Micro-conventions are voice

The smallest, most mechanical habits carry more identity than the big rhetorical moves, and they are exactly what a cleanup pass normalizes without noticing. Read them off the profile and preserve them exactly:

- **Dash convention.** Spaced en dash, unspaced em dash, or neither. Converting one to the other changes the typographic personality of every paragraph. In testing, judges named this as the clearest single signal separating two otherwise similar drafts.
- **Connectives.** "Furthermore", "Nevertheless", "Moreover", "i.e.", "etc". The concision rules call these filler. For a writer who uses them as structural joints they are load-bearing, and the profile wins. Never strip a connective the profile names.
- **Quote marks on contested terms.** Single versus double for scare quotes.
- **Numerals versus words** for small numbers. "over 3 years ago" is not the same voice as "over three years ago".
- **The occasional exclamation mark or unguarded flourish.** The one burst of enthusiasm in a piece is often the most personal thing in it, and it is the first casualty of polishing. In testing, a draft that kept the author's exclamation mark beat one that flattened it to a period — both orders, high confidence.
- **Self-referential parentheticals.** "(see next sentence)", "(at least in our simple case)". They read as untidy because they are habit.

None of these survive a pass aimed at making prose cleaner. That is the point. The author was not aiming for clean.

## The portability test

If a sentence could move unchanged into a piece about a different person, company, country, or product, it is probably filler. Cut it, or replace it with a fact, mechanism, consequence, or judgment specific to this subject.

Use this instead of adjective-counting. It catches real emptiness and leaves strong writing alone.

## Flat is a tell too

After the cuts, check the piece still has:

- A position it commits to
- Varied sentence length, including at least one long one
- At least one detail only this writer would have known or chosen
- A digression, aside, admission, or joke, if the author's profile has them

If a paragraph is now tidier and duller, revert it. Preserve blunt language, profanity, humour, self-interruption, mixed feelings, and honest admissions. These are the voice.

## Preserve structure

Keep the author's progression, including detours. Reorganize only when the current order genuinely obscures the argument — and when you do, say why in the change note.

Do not force every section into the same point-then-support shape. Do not make every paragraph the same length. Do not make every paragraph equally tidy. Uniformity is the tell.
