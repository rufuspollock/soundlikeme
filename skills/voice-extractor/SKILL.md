---
name: voice-extractor
description: Analyse writing samples to generate a compact voice profile. Use when the user wants to create a tone-of-voice guide from existing writing -- their own, a team's, or an organisation's. The output is a voice profile file usable by the write-like-me skill.
metadata:
  author: soundlikeme
  version: "0.1"
---

# Voice Extractor

Generate a compact voice profile from writing samples. The profile should be usable by the `write-like-me` skill to make AI output match the analysed voice.

## Step 1: Collect Samples

Ask the user for writing samples. Guide them with:

- "Provide 2-5 pieces of writing that represent how you want to sound."
- "Variety helps -- e.g. a blog post, an email, a document. This lets me distinguish your consistent patterns from context-dependent ones."
- "Each sample should be at least a few paragraphs. Longer is better."

Accept samples as:
- Pasted text
- File paths to read
- URLs to fetch

## Step 2: Analyse Each Sample

For each sample, identify:

1. **Sentence length** -- short/medium/long, and how they're mixed
2. **Paragraph length** -- dense or broken up
3. **Vocabulary level** -- plain, technical, academic, colloquial
4. **Use of contractions** -- frequent, occasional, never
5. **Person** -- first person, second person, third person, mixed
6. **Tone** -- formal/informal, warm/cool, confident/tentative
7. **Rhetorical devices** -- questions, repetition, analogies, examples, lists
8. **Structure** -- how ideas are organised (conclusion-first, narrative build-up, problem-solution, etc.)
9. **Signature moves** -- anything distinctive or recurring

## Step 3: Find Consistent Patterns

Compare across samples. Separate:
- **Consistent patterns** (appear in most/all samples) -- these define the voice
- **Context-dependent patterns** (vary by sample) -- note these but don't include in the core profile

## Step 4: Generate the Voice Profile

Write a voice profile in this structure:

```markdown
# Voice Profile: [Name or Label]

## Voice and Tone
[2-3 sentences describing the overall voice]

## Sentence Structure
[How sentences are constructed, length patterns, rhythm]

## Vocabulary
[Word choice tendencies, formality level, jargon usage]

## Signature Moves
[Distinctive recurring techniques -- e.g. "uses concrete examples",
"frames as question then answers", "references personal experience"]

## What This Voice Does NOT Do
[Anti-patterns specific to this voice -- things that would sound wrong]
```

**Constraints:**
- Keep the total profile under 500 words. It needs to be token-efficient.
- Be specific and concrete. "Uses short sentences" is better than "has a direct style."
- Include actual phrases or constructions the writer uses, not just descriptions.
- Don't invent patterns that aren't clearly present in the samples.

## Step 5: Validate

Present the profile to the user and ask:

1. "Does this capture how you sound?"
2. "Anything missing or wrong?"
3. "Any phrases or habits I missed that feel very 'you'?"

Revise based on feedback.

## Step 6: Save

Save the final profile to the location the user specifies. Default: `references/voice-profile.md` in the `write-like-me` skill directory, so it's automatically picked up.

If the user provides a specific path, save there instead and tell them how to point `write-like-me` at it.
