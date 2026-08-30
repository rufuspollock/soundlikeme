# Expectations: human-prose

Genuinely human blog prose with a strong voice: digressions, an exclamation mark, spaced en dashes, single scare quotes, numerals for small numbers, and "Furthermore" and "Nevertheless" used as structural joints. The correct output is the input, unchanged.

## Protected spans - an edit to any of these is a false positive

1. `over 3 years`
2. `calls it 'open'`
3. `Furthermore, the people who use the word most confidently`
4. `Nevertheless, there is a real cost`
5. `Let me be clear:`
6. `explain it to a minister!`
7. `(Whether this is actually true of every portal is unclear to me`
8. `The hard part - and this is the part I keep getting wrong - is`
9. `So what do we do? I do not have a clean answer`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
