# Concision

Twelve rules distilled from Strunk's *Elements of Style*, with examples. This is the
always-on layer, about 700 tokens. The full 1918 text is in
[elements-of-style.md](elements-of-style.md) at roughly 18,000 tokens — load it only for
`--deep`, or hand a draft to a subagent with it when context is tight.

Good writing is the floor. Voice is the layer on top. Where the two genuinely conflict,
prefer the voice profile: a person's real habits beat a general rule, and Strunk was not
writing about your user. But most apparent conflicts are the model excusing bloat.

1. **Omit needless words.** Every word earns its place.
   - "in order to" → "to" · "the question as to whether" → "whether" · "he is a man who" →
     "he" · "the reason why is that" → "because" · "at this point in time" → "now"

2. **Be definite, specific, concrete.** Vague claims hide weak thinking.
   - Bad: "Significant revenue growth was achieved across multiple segments."
   - Good: "Revenue grew 30% in Q3, driven by enterprise sales."

3. **Use the active voice.** Shorter and clearer. Passive only when the actor is genuinely
   unknown or irrelevant — or when the genre calls for it.
   - Bad: "The feature was shipped by the team after testing was completed."
   - Good: "The team shipped the feature after testing."

4. **Lead with the point.** Conclusion first, support after. Do not make the reader wait.
   - Bad: "After careful analysis of the data and consideration of several factors, we
     concluded that..."
   - Good: "We should cancel the project. The data shows..."
   - Exception: a personal setup that creates context, tension, or character earns its place.
     Cut generic throat-clearing, not a real opening.

5. **Put statements in positive form.** Say what a thing is.
   - Bad: "He was not very often on time." Good: "He usually came late."

6. **One idea per sentence.** Two ideas, two sentences. A paragraph that drifts to a second
   topic wants a break. Do not split a long sentence that is clear.

7. **Do not hedge unless genuinely uncertain.** Hedging weakens everything near it.
   - Bad: "This may potentially have an impact on performance."
   - Good: "This slows performance." Or, if truly unsure: "I don't know whether this affects
     performance."

8. **Use plain words.** Fancy synonyms do not signal intelligence.
   - use / not utilise · help / not facilitate · method / not methodology · start / not
     commence · about / not with regard to · enough / not sufficient

9. **Cut qualifiers.** "very", "really", "quite", "somewhat", "fairly" almost always weaken.
   Remove and reread. Keep one when it carries real emphasis or the author's spoken rhythm.

10. **Vary sentence length.** Default short. Use a long sentence to carry a complex idea,
    then follow it with a short one. Three sentences of the same length in a row is a
    problem.

11. **Put emphatic words at the end.** The end of a sentence carries the most weight.
    - Weak: "Failure was, in the end, the result."
    - Strong: "In the end, the result was failure."

12. **No filler transitions.** "Furthermore", "Moreover", "Indeed", "Additionally". If the
    next sentence follows logically it needs no signpost. If it does not, a signpost will
    not save it.

## Make verbs do the work

"made a decision" → "decided" · "has the ability to" → "can" · "provides support for" →
"supports" · "is representative of" → "represents" · "take into consideration" → "consider"

## Do not over-apply

Concision is not compression. Do not:

- Strip nuance that protects accuracy
- Drop coverage the piece needs
- Reduce every sentence to its shortest form — that produces the staccato style described in
  [protections.md](protections.md)
- Cut an aside, joke, or digression that carries voice

The target is that every word earns its place, not that there are as few words as possible.
