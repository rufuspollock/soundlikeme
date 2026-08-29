---
name: soundlikeme
description: Make writing sound like a specific person rather than like AI. Audit prose for AI tells, strip slop, polish a draft into someone's voice, draft new text in that voice, build a voice profile from writing samples, or calibrate an existing profile. Use when the user wants their writing to sound like them, wants AI patterns removed, asks whether something reads as AI-written, or wants to capture a voice from samples.
license: MIT
user-invocable: true
argument-hint: "[audit · deslop · polish · draft · extract · calibrate · eval] [input]"
metadata:
  author: soundlikeme
  version: "0.2"
---

# Sound Like Me

Two separate jobs live here. Do not confuse them.

**Removing AI tells** is a floor. Anyone can do it, the patterns are public, and a draft with every tell removed can still sound like nobody at all.

**Sounding like a specific person** is the actual work. It needs a profile, and it needs restraint — most of what makes writing sound like someone is what you leave alone.

## The two rules that matter most

**1. Minimum effective edit.** Change only spans with a confirmed defect, using the smallest repair that fixes it. Copy every other sentence byte-for-byte, in its original order and paragraph. With no findings, return the input exactly. A rough draft with a real voice must still sound like the same person afterwards.

**2. A match is not a license.** Finding a banned word does not authorize an edit. Check `references/protections.md` first. Prefer a no-op to an uncertain edit. Over-editing produces clipped, opinion-flattened anti-slop prose, which is its own detectable style and a worse failure than the slop.

## Routing

Load exactly one command file before acting. Do not act from this page alone.

| Command | What it does | File |
|---|---|---|
| `audit` | Report tells with quotes and fixes. Changes nothing. | [commands/audit.md](references/commands/audit.md) |
| `deslop` | Cheap pass. Tells and structures only. No profile needed. | [commands/deslop.md](references/commands/deslop.md) |
| `polish` | Full pass: tells, concision, voice profile. The main event. | [commands/polish.md](references/commands/polish.md) |
| `draft` | Write new text in the voice, from notes or a brief. | [commands/draft.md](references/commands/draft.md) |
| `extract` | Build a voice profile from writing samples. | [commands/extract.md](references/commands/extract.md) |
| `calibrate` | Sharpen an existing profile with an A/B quiz. | [commands/calibrate.md](references/commands/calibrate.md) |
| `eval` | Score the skill against held-out writing. | [commands/eval.md](references/commands/eval.md) |

Routing by intent when no command word is given:

| The user says | Go to |
|---|---|
| "does this sound like AI?", "flag it, don't change it", "review before I publish" | `audit` |
| "de-AI this", "make it sound human", "remove the slop" | `deslop` |
| "make this sound like me", "polish this", "tidy this up" | `polish` |
| "write this as me", "draft a post about X" | `draft` |
| "learn my voice", "here are some samples" | `extract` |
| "does this sound like me?", "quiz me" | `calibrate` |

Ambiguous between `deslop` and `polish`? `polish` if a profile exists, `deslop` if not.

## Cost

These differ by an order of magnitude. Pick deliberately.

| Command | Loads | Rough cost |
|---|---|---|
| `audit` / `deslop` | router, tells, protections | ~4.5k tokens |
| `polish` / `draft` | the above plus concision, profile, self-check | ~8.5k tokens |
| `polish --deep` | the above plus the full 1918 Strunk text | ~26k tokens |

Use `--deep` for writing that will be published under the user's name and read by people who know their work. Not for a Slack message.

## Profiles

Profiles live in `profiles/<name>.md`. Resolution order:

1. A file the user points at
2. `profiles/` — if exactly one, use it; if several, ask which
3. No profile: say so and offer `extract`. `audit` and `deslop` still work without one.

## Bounded passes

Two passes, then stop. The first removes obvious patterns; the second catches tells the first pass introduced — recycled transitions, new staccato rhythm, flattened opinions. Then run `references/self-check.md` once and fix what it catches.

Do not loop. Grinding a third and fourth pass makes prose worse, not better, and burns the user's money doing it.

## References

| File | When |
|---|---|
| [tells.md](references/tells.md) | Every command that edits or audits |
| [protections.md](references/protections.md) | Same. Never load tells without it |
| [concision.md](references/concision.md) | `polish`, `draft` |
| [elements-of-style.md](references/elements-of-style.md) | `--deep` only. ~18k tokens |
| [profile-spec.md](references/profile-spec.md) | `extract`, `calibrate` |
| [self-check.md](references/self-check.md) | Before returning any edited or drafted text |
