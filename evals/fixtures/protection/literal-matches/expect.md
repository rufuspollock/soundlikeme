# Expectations: literal-matches

Banned words used literally, or in a domain where they are the precise term. A match is not a licence.

## Protected spans - an edit to any of these is a false positive

1. `began to delve along the lower bed`
2. `not the winding gear but a tapestry, nine feet by four`
3. `The owners had leverage over the leaseholder`
4. `that leverage disappeared`
5. `more robust than the national series`
6. `forty-one figures`
7. `1907 fall`
8. `two of its sixteen pages to the gift shop`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
