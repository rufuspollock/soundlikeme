# Sound Like Me

Make AI-generated writing sound like *you* -- or your team, or your organisation.

## The Problem

AI writing tends to sound like AI. When you dictate ideas, brainstorm, or draft with an AI assistant, the output loses your voice. It comes out generic, verbose, and flat. You want two things at once: writing that sounds like you *and* writing that's actually good -- concise, clear, well-structured.

## What This Project Is

Research and tooling for directing AI to produce writing that matches a specific voice while maintaining high quality.

### Goals

1. **Research effective approaches** -- How do you prompt or direct AI to match a voice? What works, what doesn't, and what's token-efficient enough to be practical?

2. **Distill good writing principles** -- A compact reference (think: distilled Strunk & White) that any AI can follow to produce concise, clear prose. The baseline before voice-matching even starts.

3. **Build reusable skills/tools** -- Agent skills (e.g. for Claude Code) that can be loaded on demand -- not at conversation start, but at the point where you want polished output. "I've finished thinking, now make this sound like me."

4. **Auto-generate voice guidance from samples** -- Point the tool at existing writing (yours, your org's, anyone's) and have it produce a distilled tone-of-voice guide. Turn 50 blog posts into a 500-word voice profile that an AI can follow.

### Who It's For

- Individuals who write with AI and want to keep their voice
- Organisations that need consistent tone across AI-assisted content
- Anyone who wants AI output that reads like a human wrote it -- a *specific* human

## Approach

- Start with research: what techniques exist, what's been tried, what actually works
- Experiment with prompt structures, style guides, and few-shot examples
- Optimise for token efficiency -- the guidance needs to be compact enough to be practical
- Package what works into loadable skills and shareable tools
