# Research: Making AI Write in Your Voice

A survey of techniques, tools, and findings for directing AI to produce writing that matches a specific person's (or organisation's) voice while maintaining quality.

## 1. The Core Problem

AI writing defaults to a generic, over-polished style. It hedges, uses filler, reaches for impressive-sounding words ("leverage", "delve", "transformative"), and produces structurally formulaic output. The challenge is twofold:

1. **Voice matching** -- making output sound like a specific person
2. **Quality baseline** -- ensuring the writing is actually good (concise, clear, well-structured) regardless of whose voice it's in

These are related but distinct problems. You can match someone's voice perfectly but still produce bloated prose, or write concisely but sound nothing like the target.

## 2. Approaches to Voice Matching

### 2.1 Few-Shot Examples (Provide Writing Samples)

The most consistently effective technique across all sources. Give the AI 1-3 samples of the target writing and ask it to match the style.

**Key findings:**
- A single well-chosen sample outperformed more complex approaches by ~20% in one controlled experiment (Taylor, Saxifrage Blog)
- 2-3 samples are better than 1, and *variety* matters -- e.g. a blog post, an email, a memo -- so the AI can distinguish consistent patterns from context-dependent ones
- Samples should be 2,000-4,000 words each for sufficient signal
- Label samples clearly with brackets or headers so the AI knows they're style references, not content to respond to

**Why it works:** LLMs learn patterns in-context. Providing examples is effectively "many-shot" in-context learning -- the model internalises the writing distribution from the examples.

**Token cost:** High. 3 samples at 3,000 words each = ~12,000 tokens of context. This is the main drawback -- it's effective but expensive per-call.

### 2.2 Extracted Style Descriptions (Voice Profiles)

Use AI to *analyse* writing samples and produce a compact style description, then use that description (not the samples) in future prompts.

**The Forte Labs method (Tiago Forte):**
1. Select 2-3 representative pieces (use your best/most characteristic writing)
2. Prompt: *"You have expertise in linguistics, NLP, and prompt engineering. Convert the provided text into an elaborate style guide serving as a blueprint for fresh content while maintaining original style. Pay special attention to: voice and tone, mood, sentence structure, transition style, rhythm and pacing, and signature styles."*
3. Feed multiple samples to refine the description
4. Result: a reusable style guide covering voice, tone, mood, sentence structure, transitions, rhythm, and signature techniques

**The Panickssery pipeline (Nina Panickssery):**
1. Collect 5+ writing samples, convert to markdown
2. Ask AI to describe the style for use in system prompts
3. Generate synthetic prompts that could have produced each sample
4. Build fake conversation history (user prompt -> sample as response)
5. Use conversation history + style description as the prompt

The Panickssery approach is more sophisticated -- it creates synthetic training data from samples. A Python script automates the pipeline.

**Token cost:** Much lower than raw samples. A 300-500 word style description replaces 12,000+ tokens of samples.

**Trade-off:** Some fidelity is lost in compression. The description captures what AI *notices* about the style, which may miss subtleties.

### 2.3 Adjective/Attribute-Based Descriptions

Describe the desired voice using specific adjectives and style attributes rather than samples.

**Common frameworks:**
- **3-5 adjectives** with explanations: e.g. "Direct -- state conclusions first, qualify later"
- **Voice dimensions:** formality, complexity, warmth, authority, humour
- **Anti-patterns:** explicitly list what to avoid (see Section 4)
- **Tone registers:** Word.Studio catalogues 29 distinct registers (playful, minimalist, academic, edgy, etc.) with descriptions

**When to use:** When you don't have writing samples, or for an organisation defining a new voice. Less effective than samples for matching a *specific* person, but useful for setting a general direction.

**Token cost:** Very low. 50-200 words.

### 2.4 Hybrid: Description + Examples

The most practical approach for ongoing use:

1. **Once:** Use AI to analyse your samples and generate a style description
2. **Per-session:** Load the compact description into the system prompt or as a skill
3. **Optionally:** Include 1 short sample as a concrete reference point

This balances fidelity (the sample grounds the description) with token efficiency (the description does most of the work).

## 3. Concise Writing Principles (The Quality Baseline)

Regardless of voice, AI output benefits from explicit conciseness constraints. These are the "Strunk & White layer" -- universal principles of good writing that prevent AI bloat.

**Core rules (distilled from Strunk & White, Gwern, and others):**

1. **Omit needless words.** Every word should serve a purpose. Cut filler ("in order to" -> "to", "at this point in time" -> "now").
2. **Use concrete, specific language.** "Revenue grew 30%" not "significant revenue growth was achieved."
3. **Prefer active voice.** "We shipped the feature" not "the feature was shipped by the team."
4. **Lead with the point.** State the conclusion, then support it. Don't build up to it.
5. **One idea per sentence.** If a sentence has two ideas, split it.
6. **Avoid hedging.** Say "X causes Y" not "X may potentially have an impact on Y." If uncertain, say so directly: "I'm not sure whether X causes Y."
7. **Use plain words.** "Use" not "utilise." "Help" not "facilitate." "Method" not "methodology."
8. **Cut qualifiers.** "Very", "really", "quite", "somewhat" almost always weaken a sentence.
9. **Vary sentence length for rhythm** but default to short.
10. **Limit section headers to six words.**

**Token cost for including these in a prompt:** ~200-300 words. Worth it.

## 4. Anti-Patterns: What to Explicitly Ban

AI has characteristic tics. Explicitly banning them is surprisingly effective.

### Words and phrases to ban:
- "Delve", "realm", "tapestry", "landscape", "leverage", "utilise", "facilitate", "endeavour"
- "In today's [adjective] world/landscape/environment"
- "It's important to note that..."
- "Let's dive in", "Let's unpack this"
- "At the end of the day"
- "Revolutionise", "game-changer", "cutting-edge", "innovative" (unless literally true)
- "Furthermore", "Moreover", "Indeed" as sentence starters
- "Transformative", "paradigm shift", "synergy"

### Structural patterns to ban:
- **Snappy triads:** "Fast, efficient, and reliable." (AI overuses these)
- **The negation flip:** "It's not X -- it's Y." / "No X, no Y, just Z."
- **Unearned profundity:** "Something shifted." / "And that changes everything."
- **Rhetorical questions as transitions:** "But what does this really mean?"
- **The false build-up:** "The solution? It's simpler than you think."
- **Gratuitous em-dashes.** Use sparingly, not in every paragraph.

### Meta-patterns:
- Starting every section the same way
- Ending with an inspirational call-to-action
- Over-using bullet points when prose would be better
- Summarising what was just said ("In summary..." / "As we've seen...")

## 5. Tools and Implementations

| Tool/Method | What it does | Notes |
|---|---|---|
| **Panickssery pipeline** | Python script that automates sample -> style profile -> synthetic training data | [GitHub](https://github.com/ninapanickssery) -- most technically sophisticated approach |
| **Forte Labs method** | Manual 3-step process: select samples, analyse with AI, synthesise guide | Good for individuals, described in [blog post](https://fortelabs.com/blog/how-to-create-an-ai-style-guide-write-with-chatgpt-in-your-own-voice/) |
| **BulkForge Voice Analyzer** | Free web tool that analyses pasted text and generates a voice profile | Quick and easy, less customisable |
| **Custom GPTs** | OpenAI feature for persistent style configuration | Platform-locked, not portable |
| **LLM Writing Style Guide** | Open-source reference of 100+ style dimensions | [GitHub](https://github.com/viktorbezdek/definitive-llm-writing-style-guide) -- useful as a menu of attributes to pick from |
| **Word.Studio descriptors** | 29 tone/style categories with detailed descriptions | Good reference for choosing adjectives |

## 6. What Works Best (Synthesis)

Based on the research, the most effective and practical approach combines several layers:

### For initial setup (once):
1. Gather 3-5 writing samples that represent your best/most characteristic work
2. Use AI to extract a style profile (voice, tone, structure, signature moves)
3. Manually edit the profile -- remove what's wrong, sharpen what's right
4. Add a concise-writing baseline (Section 3 rules)
5. Add an anti-pattern banlist (Section 4)
6. Keep the total guidance under 800 words for token efficiency

### For ongoing use (per-session):
1. Load the style guide as a skill or system prompt (not at conversation start -- only when you want polished output)
2. Optionally include 1 short sample as a concrete anchor
3. Write/dictate freely, then invoke the style guide for the synthesis/polish step

### For organisations:
1. Same process but with multiple authors' samples to find the *shared* patterns
2. The style guide becomes a living document that evolves with editorial feedback
3. Anti-patterns can be customised per-org (e.g. "never say 'stakeholders'")

### For auto-generating voice profiles:
1. Point the tool at a corpus of writing
2. AI extracts patterns across: vocabulary, sentence length/structure, tone, rhetorical devices, paragraph structure, use of examples, level of formality
3. Output: a compact voice profile (300-500 words) usable in any AI system
4. Human review is essential -- AI will sometimes hallucinate patterns or miss the most important ones

## 7. Open Questions

- **How much does model matter?** Do these techniques transfer equally well across Claude, GPT-4, Gemini, etc.?
- **Decay over long outputs:** Style adherence may drift in longer pieces. Does periodic re-grounding help?
- **Evaluation:** How do you objectively measure "sounds like me"? Embedding distance (as Taylor used) is one approach, but subjective judgement may matter more.
- **Diminishing returns on samples:** At what point do more samples stop improving fidelity?
- **Interaction with other instructions:** Does a style guide conflict with other system prompt instructions?

## Sources

- [Forte Labs: How to Create an AI Style Guide](https://fortelabs.com/blog/how-to-create-an-ai-style-guide-write-with-chatgpt-in-your-own-voice/) -- Tiago Forte's 3-step method
- [Nina Panickssery: How to Make an LLM Write Like Someone Else](https://blog.ninapanickssery.com/p/how-to-make-an-llm-write-like-someone) -- automated pipeline approach
- [Saxifrage: AI Writing Style Prompt Experiment](https://www.saxifrage.xyz/post/ai-writing-style-prompt-experiment) -- controlled experiment showing 1-shot samples win
- [Definitive LLM Writing Style Guide (GitHub)](https://github.com/viktorbezdek/definitive-llm-writing-style-guide) -- comprehensive style dimension reference
- [Word.Studio: Tone of Voice & Style Prompt Descriptions](https://word.studio/tone-of-voice-style-prompts-descriptions/) -- 29 tone/style categories
- [Gwern.net: Manual of Style](https://gwern.net/style-guide) -- rigorous writing style principles
- [Blake Stockton: Don't Write Like AI](https://www.blakestockton.com/red-flag-words/) -- AI anti-patterns and red flag words
- [The Field Guide to AI Slop](https://www.ignorance.ai/p/the-field-guide-to-ai-slop) -- catalogue of AI writing cliches
- [Wikipedia: Signs of AI Writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) -- community-maintained list of AI tells
