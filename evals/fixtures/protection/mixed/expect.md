# Expectations: mixed

The hard one. Real slop and protected spans sit in the same document, so the pass has to edit and restrain at once rather than choosing a disposition for the whole piece. Every other protection fixture can be passed by doing nothing; this one cannot.

The protected spans are of five kinds: the author's own micro-conventions (spaced dashes, "Furthermore" and "Nevertheless" as joints, a numeral for a small number, one exclamation mark), a passage of scientific writing where the passive is correct, a quoted vendor blockquote that is pure slop and is therefore evidence, contract wording where the scope words are load-bearing, and a code identifier.

## Must fix - a survivor is a false negative

1. `In today's fast-paced digital landscape (hard phrase, opening line)`
2. `It's important to note that (hard phrase)`
3. `Let's unpack this (hard phrase)`
4. `The reality is that (hard phrase)`
5. `Studies show that most analytics projects fail (weasel attribution)`
6. `At the end of the day (hard phrase)`
7. `you can't fix what you can't see (fake-profound kicker)`
8. `The implications are significant (vague declarative)`

## Must not touch - an edit here is a false positive

1. `for over 3 years`
2. `Samples were collected at 14-day intervals`
3. `The weighted interval score was used`
4. `Coverage was assessed at the 50% and 90% nominal levels`
5. `Furthermore, the sites that moved most`
6. `Nevertheless, something real is in there`
7. `This transformative deployment stands as a testament`
8. `delve into the intricate tapestry of operational excellence`
9. `unlocking the full potential of the modern data estate`
10. `retain all licence identifiers in every copy of the Licensed Materials without modification`
11. `The one thing that surprised me - and this is the part I keep coming back to - is`
12. ``ingest --validate-schema``
13. `five weeks`
14. `So what now? I don't have a clean answer`
15. `explain this to a procurement officer!`

Correct behavior: eight repairs, nothing else moved. Scoring this fixture reports both rates, and the interesting number is whether a pass that fires correctly eight times keeps its hands off the fifteen spans next to them.
