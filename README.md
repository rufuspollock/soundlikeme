# Sound Like Me

Make AI-generated writing sound like *you* -- or your team, or your organisation.

## The Problem

AI writing sounds like AI. When you dictate ideas, brainstorm, or draft with an AI assistant, the output loses your voice. It comes out generic and flat.

Two separate problems hide inside that complaint:

1. **Removing AI tells.** A floor. The patterns are public, well catalogued, and [several good skills already do this](docs/prior-art.md).
2. **Sounding like a specific person.** The actual work, and much less solved.

Most tools do the first and claim the second. A draft with every tell removed can still sound like nobody at all -- and over-editing produces its own detectable style: clipped, tidy, opinion-flattened.

## What's In This Repo

### `skills/soundlikeme`

One skill, seven commands, built to the [agentskills.io](https://agentskills.io) specification.

| Command | What it does | Cost |
|---|---|---|
| `audit` | Report AI tells with quotes and fixes. Changes nothing. | ~4.5k tokens |
| `deslop` | Cheap pass. Tells and structures only. No profile needed. | ~4.5k tokens |
| `polish` | Full pass: tells, concision, your voice profile. | ~8.5k tokens |
| `polish --deep` | The above plus the full 1918 *Elements of Style*. | ~26k tokens |
| `draft` | Write new text in your voice from notes or a brief. | ~8.5k tokens |
| `extract` | Build a voice profile from your writing samples. | -- |
| `calibrate` | Sharpen a profile with an A/B quiz against your real writing. | -- |
| `eval` | Score the skill against held-out writing. | -- |

The skill is a thin router. Each command loads exactly one reference file, so a quick pass on a Slack message doesn't drag in eighteen thousand tokens of Strunk.

Two rules do most of the work:

- **Minimum effective edit.** Change only spans with a named defect. Copy everything else byte-for-byte. No findings, no changes.
- **A match is not a licence.** Finding a banned word doesn't authorise an edit. Check the protections first, and prefer a no-op to an uncertain edit.

### `evals/`

The part that makes this more than assertion.

Take a piece you published that your profile was *not* built from. Strip it to a de-voiced brief. Have the skill draft from the brief. Compare the draft to what you actually wrote.

The score alone means nothing, so two runs bracket it: a **ceiling** (a different real piece of yours, scored the same way) and a **floor** (the same brief drafted with no profile). The number that matters is the fraction of that headroom closed, per dimension.

Plus the control group nobody runs: same brief, profile on versus profile off. If the profile doesn't win, it's decoration.

See [evals/README.md](evals/README.md) and the [design rationale](docs/plans/2026-08-29-objective-function-design.md).

### `docs/`

- [`prior-art.md`](docs/prior-art.md) -- survey of the eight serious projects in this space, what each does well, and what's worth stealing
- [`research.md`](docs/research.md) -- techniques for voice-matching: few-shot samples, extracted style descriptions, anti-patterns
- [`plans/`](docs/plans/) -- design documents

## Install

```bash
npx skills add rufuspollock/soundlikeme/skills/soundlikeme
```

Works with Claude Code, Cursor, GitHub Copilot, and [other agents that support the agentskills.io spec](https://skills.sh/).

## Usage

1. **Build a profile:** `/soundlikeme extract` with 3-5 samples of your writing, varied by register. Hold at least one piece back -- it becomes an eval case.
2. **Sharpen it:** `/soundlikeme calibrate`. You're bad at describing your own writing and excellent at recognising it, so the A/B quiz gets further than a review.
3. **Write or dictate freely.**
4. **Polish:** `/soundlikeme polish`. Or `audit` first if you just want to see what's there.

## Credits

- *The Elements of Style* (1918) is public domain. Packaging it as an agent reference is [obra's idea](https://github.com/obra/the-elements-of-style), and the vendored text is his.
- The tell catalogue draws on [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) and the projects surveyed in [`docs/prior-art.md`](docs/prior-art.md) -- particularly [no-ai-slop](https://github.com/petergyang/no-ai-slop) for the self-check idea, [unslop](https://github.com/theclaymethod/unslop) for the minimum-effective-edit contract, and [better-writing](https://github.com/forjd/better-writing) for genre exemptions.

## Status

Early. The skills work; the eval harness has one seeded case and has not been run yet. Next up: run it, add practice authors so the technique isn't overfitted to one voice, and build fixtures for `polish` (the eval currently only measures `draft`).
