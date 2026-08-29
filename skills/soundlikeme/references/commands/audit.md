# audit

Report what reads as AI. Change nothing.

Load: [tells.md](../tells.md), [protections.md](../protections.md). A profile if one exists — it changes which patterns are the author's habits rather than defects.

## Rules

- **Do not rewrite.** Not even one sentence, not even to demonstrate. Offer at the end.
- **Do not score the author** or estimate how much of the piece was AI-written. Detectors guess. Named patterns are evidence the user can check for themselves.
- **Do not claim to detect AI authorship.** You are naming patterns, not attributing.
- Quote the smallest defective span, not the paragraph around it.
- Say which findings are judgment calls. A user who cannot tell your confident findings from your speculative ones will ignore all of them.

## Establish first

Genre, audience, and medium — they decide which patterns are exempt. Ask once if unclear.

## Output

```markdown
## Findings

**Hard** — these read as AI in any context

- "[quoted span]" — [pattern name]. [Fix in a few words.]

**Cluster** — individually fine, collectively a tell

- [pattern name], N instances: "[span]", "[span]" — [what to do]

**Judgement calls** — may be deliberate

- "[quoted span]" — [pattern name]. [Why it might be intentional.]

## Protected

- [Anything that matched a rule but should stay, and why.]

## Assessment

[Two or three sentences: where the real problem is concentrated, and what fixing it costs.]
```

The **Protected** section is not optional. It shows the user you looked at context rather than running a regex, and it is the part that builds trust in the findings above it.

Close by offering `deslop` or `polish`.
