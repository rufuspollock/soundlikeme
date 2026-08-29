# calibrate

Sharpen an existing profile by testing it against the author, rather than asking them to
review a description of themselves.

Load: the profile, [profile-spec.md](../profile-spec.md).

People are bad at describing their own writing and excellent at recognising it. `extract`
asks them to do the first thing. This command asks them to do the second.

## The A/B quiz

Ten rounds. Each round:

1. Take a paragraph of the author's real writing — from a held-out piece, never from the
   samples the profile was built on.
2. Write a rival paragraph on the same content, following the current profile.
3. Show both, unlabelled, in random order.
4. Ask which is theirs.
5. Ask the more valuable question: **what gave it away?**

Their answer to (5) is the actual output of this command. "Mine has the aside in brackets"
or "I'd never open with the definition" is a profile edit you could not have derived any
other way.

## Reading the results

- **They pick correctly every time and can say why.** The profile is missing something
  concrete. Their explanations are the fix. This is the normal and most useful outcome.
- **They pick correctly but cannot say why.** Push: point at two specific spans and ask which
  feels wrong. Recognition without articulation still localises the gap.
- **They cannot reliably tell.** The profile is working for this register. Say so plainly
  rather than manufacturing more findings, and move to a different register — the profile
  may hold for essays and fail entirely for email.
- **They pick the generated one as theirs.** Note it. Either the profile is genuinely good,
  or the real paragraph was unrepresentative. Two of these in a row means the former.

## After the quiz

Update the profile with what you learned. Be specific — the value is in the detail:

- Add signature moves they named, with the example from the round that surfaced it
- Correct markers the rounds contradicted
- Add to "does not do" every time they said "I'd never write that"
- Delete any pattern they did not recognise as theirs

Show the diff, not the whole rewritten profile. Say which round produced each change.

## Voice check

A single-round variant. The user asks "does this sound like me?" about one piece.

Score it against the profile's markers and signature moves, quoting evidence. Say what is
off and by how much. Do not rewrite unless asked — this is a diagnostic, and the user is
usually deciding whether to publish, not asking for an edit.
