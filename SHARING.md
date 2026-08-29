## Sound Like Me: Making AI Writing Sound Like You

I do a lot of writing with AI now. Dictating ideas, brainstorming, having it synthesise my thinking. The problem is the output sounds like AI, not like me. It comes out generic and flat -- all the rough edges sanded off, all the voice removed.

I want two things: writing that sounds like *me*, and writing that's actually *good*. Concise, clear, well-structured. These are related but distinct problems. You can match someone's voice perfectly and still produce bloated prose.

So I've been experimenting with how to solve this. The result is [Sound Like Me](https://github.com/rufuspollock/soundlikeme) -- a small open-source project: one agent skill built to the [agentskills.io](https://agentskills.io) specification, with a few separate jobs behind it.

1. **`extract`** -- you point it at a few samples of your writing and it generates a compact voice profile: your tone, sentence structure, vocabulary, signature moves, what you *don't* do. Plus measurable markers -- sentence length variance, contraction rate, how often you reach for a footnote -- so the profile is something you can actually check output against.

2. **`polish`** -- you load this on demand when you want polished output. It applies your voice profile, a set of distilled conciseness rules (think Strunk & White compressed to 12 rules with examples), and an explicit banlist of AI anti-patterns ("delve", "landscape", the dreaded snappy triad). The idea is you draft freely, then invoke this at the polish step.

3. **`eval`** -- the part I care most about, and the part nobody else seems to do. How do you know any of this is working? Take a piece you published that the profile wasn't built from, strip it to a bulleted brief, have the skill draft from that brief, and compare the draft to what you actually wrote. Then bracket the score: a ceiling (another real piece of yours) and a floor (the same brief with no profile). If the profile doesn't beat the floor, it's decoration.

The approach came out of a research survey I did on what actually works. The short answer: extracted voice profiles (compact descriptions generated from your writing samples) combined with conciseness rules and anti-pattern bans. Raw writing samples are more effective but burn too many tokens for practical use. A hybrid -- compact profile plus one short sample as an anchor -- is the sweet spot.

This isn't just for me. An organization could use the same approach to maintain consistent tone across AI-assisted content. And the cooler version -- which is where I'd like to take this -- is pointing the voice extractor at a corpus of someone's writing and having it auto-generate a voice guide. Turn 50 blog posts into a 500-word profile that any AI can follow.

It's early and experimental. The skills work but they need more testing across different types of writing. The repo is at [github.com/rufuspollock/soundlikeme](https://github.com/rufuspollock/soundlikeme) if you want to try it or contribute.

To install the skills:

```bash
npx skills add rufuspollock/soundlikeme/skills/soundlikeme
```

This works with Claude Code, Cursor, GitHub Copilot, and [other agents that support the agentskills.io spec](https://skills.sh/).

---

## Social Posts

### X/Twitter (variations)

**Version 1:**

I built two agent skills to make AI writing sound like me instead of like AI.

- Point it at your existing writing, it extracts a voice profile
- Distilled Strunk & White rules baked in
- Explicit banlist for AI slop ("delve", "landscape", snappy triads)
- Load on demand at the polish step, not at conversation start

Early prototype. Open source, agentskills.io spec.

🔗 [link]

**Version 2:**

AI writes like AI. I want it to write like me.

So I made "Sound Like Me" -- two agent skills that extract a voice profile from your writing samples, then apply it when you want polished output. Also bans the worst AI tics and enforces concise writing rules.

Works with Claude Code, Cursor, Copilot.

🔗 [link]

**Version 3:**

The hard part of writing with AI isn't getting it to write. It's getting it to sound like you.

Built a prototype: feed it a few blog posts, it extracts your voice into a 500-word profile. Then load it on demand to polish drafts. Plus distilled Strunk & White + an AI-slop banlist.

Open source: [link]
