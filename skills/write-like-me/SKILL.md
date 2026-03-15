---
name: write-like-me
description: Polish and rewrite AI-generated or drafted text to match a specific person's voice and writing style. Use when the user wants output to sound like them (or a specified voice) rather than generic AI. Loaded on demand at the writing/polishing stage, not at conversation start.
metadata:
  author: soundlikeme
  version: "0.1"
---

# Write Like Me

You are rewriting or polishing text to match a specific voice. Follow these layers in order:

## Step 1: Load the Voice Profile

Check for a voice profile in one of these locations (in priority order):

1. A file the user points you to
2. `references/voice-profile.md` in this skill directory
3. If no profile exists, tell the user: "No voice profile found. Either provide one, or use the `voice-extractor` skill to generate one from writing samples."

Read the voice profile before proceeding.

## Step 2: Apply Concise Writing Rules

Regardless of the voice profile, apply these baseline rules. They override the voice profile if there is a conflict.

1. **Omit needless words.** Cut filler. "In order to" becomes "to." "At this point in time" becomes "now."
2. **Use concrete, specific language.** "Revenue grew 30%" not "significant revenue growth was achieved."
3. **Prefer active voice.** "We shipped it" not "it was shipped by the team."
4. **Lead with the point.** State the conclusion first, then support it.
5. **One idea per sentence.** Split sentences that carry two ideas.
6. **Don't hedge.** "X causes Y" not "X may potentially have an impact on Y." If genuinely uncertain, say so directly.
7. **Use plain words.** "Use" not "utilise." "Help" not "facilitate." "Method" not "methodology."
8. **Cut qualifiers.** "Very", "really", "quite", "somewhat" almost always weaken a sentence. Remove them.
9. **Vary sentence length** but default to short.
10. **No filler transitions.** Cut "Furthermore", "Moreover", "Indeed", "It's important to note that."

## Step 3: Ban AI Anti-Patterns

Never use these in the output. If they appear in the draft, replace them.

**Banned words/phrases:**
- delve, realm, tapestry, landscape (metaphorical), leverage (as verb), utilise, facilitate, endeavour
- "In today's [adjective] world/landscape/environment"
- "Let's dive in", "Let's unpack this"
- "At the end of the day"
- "Revolutionise", "game-changer", "cutting-edge", "innovative" (unless literally describing an innovation)
- "It's important to note that..."
- "Transformative", "paradigm shift", "synergy"

**Banned structural patterns:**
- Snappy triads: "Fast, efficient, and reliable." (Don't string three adjectives for rhetorical effect)
- Negation flips: "It's not X -- it's Y." / "No X, no Y, just Z."
- Unearned profundity: "Something shifted." / "And that changes everything."
- Rhetorical questions as transitions: "But what does this really mean?"
- False build-ups: "The solution? It's simpler than you think."
- Ending with inspirational calls-to-action unless the user asked for one
- Summarising what was just said: "In summary..." / "As we've seen..."

## Step 4: Rewrite

With the voice profile, conciseness rules, and anti-patterns all loaded:

1. Read the draft text the user wants polished
2. Rewrite it to match the voice profile while applying all rules above
3. Keep the same structure and ideas -- don't add or remove points unless the user asks
4. Present the rewritten version
5. If you changed the structure or emphasis significantly, briefly note what you changed and why

## Notes

- This skill is for the **polish/rewrite step**, not for initial drafting. The user will have already generated or dictated content.
- Keep the rewritten output roughly the same length or shorter. Never inflate.
- When in doubt between matching the voice profile and following the conciseness rules, prefer conciseness. Good writing is the floor; voice is the layer on top.
