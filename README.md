# Sound Like Me

An agent skill that makes AI-assisted writing sound like *you* — plus the eval harness that proves whether it actually does.

Works in Claude Code, Cursor, GitHub Copilot, and [other agents supporting the agentskills.io spec](https://skills.sh/).

```bash
npx skills add rufuspollock/soundlikeme/skills/soundlikeme
```

---

## The problem

AI writing sounds like AI. You dictate ideas, brainstorm, or draft with an assistant, and the output comes back generic and flat — your thinking, somebody else's voice.

Two separate problems hide inside that complaint, and most tools conflate them:

1. **Removing AI tells.** A floor. The patterns are public, well catalogued, and [several good skills already do this](docs/prior-art.md).
2. **Sounding like a specific person.** The actual work, and much less solved.

A draft with every tell removed can still sound like nobody at all. Worse, over-editing produces its own detectable style — clipped, tidy, opinion-flattened — which is a downgrade from the slop it replaced, because the slop at least left your structure intact.

This project does the second thing, and treats the first as table stakes.

## What it looks like

Both passages below carry identical content. The first is a real essay deliberately de-voiced into the prose an assistant produces from the same ideas. The second is what `polish` returned with the author's voice profile loaded.

**Before**

> When I was young, I assumed that older people had essentially everything figured out. Now that I am older myself, I can report that this assumption was mistaken.
>
> In fact, I find that I feel like a noob almost constantly. I am generally either talking with a startup operating in a field I know nothing about, or working through a book on a subject I do not understand adequately, or traveling in a new country where I have little idea how anything actually works.
>
> ## The Paradox of Feeling Like a Noob

**After**

> When I was young, I thought old people had everything figured out. Now that I'm old myself, I can report that they don't.
>
> In fact, I feel like a noob almost constantly. I'm usually either talking to a startup in a field I know nothing about, or working through a book on a subject I don't understand well enough, or traveling in a country where I have no idea how anything works.
>
> It's not a pleasant feeling, and "noob" is not meant as a compliment. But I recently realized there's something encouraging hidden in it: the more of a noob you are locally, the less of a noob you are globally.

Contractions back, invented section headers gone, hedges dropped, "figure out" restored over "determine". This is a real fixture, not a mockup — it lives in [`evals/fixtures/polish/being-a-noob/`](evals/fixtures/polish/being-a-noob/), and blind judges picked the second version as closer to the author 2-0 in both orders.

## Quick start

You need an agent that supports skills, and three to five samples of your own writing.

**1. Install**

```bash
npx skills add rufuspollock/soundlikeme/skills/soundlikeme
```

**2. Build your voice profile**

```
/soundlikeme extract
```

Point it at 3-5 samples varied by register — an essay, a talk, an email, a post. Two things matter more than sample count: the samples must be writing *you* wrote without AI help, and they should be your most characteristic work rather than your most recent or most professional. It saves the profile to `profiles/<your-name>.md`.

Hold at least one piece back. The skill will ask you to. That held-out piece is the only way to check later whether the profile does anything.

**3. Sharpen it**

```
/soundlikeme calibrate
```

Ten rounds of a blind A/B quiz: your real paragraph against one written from the profile, and you say which is yours. People are bad at describing their own writing and excellent at recognizing it — `extract` asks you to do the first thing, `calibrate` asks you to do the second. The valuable answer is not which one you picked, it's *what gave it away*.

**4. Write freely, then polish**

```
/soundlikeme polish <your draft>
```

Draft or dictate without worrying about how it reads. Run `polish` at the end. Use `audit` first if you only want to see what's there without anything changing.

## Commands

| Command | What it does | Context cost |
|---|---|---|
| `audit` | Report AI tells with quotes and fixes. Changes nothing. | ~5.3k tokens |
| `deslop` | Cheap pass. Tells and structures only. No profile needed. | ~5.3k tokens |
| `polish` | Full pass: tells, concision, your voice profile. The main event. | ~9.5k tokens |
| `polish --deep` | The above plus the full 1918 *Elements of Style*. | ~27k tokens |
| `draft` | Write new text in your voice from notes or a brief. | ~9.5k tokens |
| `extract` | Build a voice profile from your writing samples. | — |
| `calibrate` | Sharpen a profile with a blind A/B quiz against your real writing. | — |
| `eval` | Score the skill against your held-out writing. | — |

You can also just say what you want — "make this sound like me", "does this read as AI?", "de-slop this" — and the skill routes to the right command.

`--deep` is for writing published under your name to readers who know your work. Not for a Slack message.

## How it works

**The skill is a thin router.** [`SKILL.md`](skills/soundlikeme/SKILL.md) names exactly one reference file per command, so a quick pass on a Slack message doesn't drag in eighteen thousand tokens of Strunk. The costs in the table above are measured against real file sizes, not estimated.

**Two rules do most of the work:**

- **Minimum effective edit.** Change only spans with a named defect, using the smallest repair that fixes it. Copy every other sentence byte-for-byte. No findings, no changes — a no-op is a valid and common result.
- **A match is not a license.** Finding a banned word doesn't authorize an edit. Check [`protections.md`](skills/soundlikeme/references/protections.md) first, and prefer a no-op to an uncertain edit.

**The protections are the actual product.** The banlist has converged across every project in this space and is a commodity; what's hard is not firing on the cases that are fine — quoted material, scientific writing where the passive is correct, code, legal wording, banned words used literally, and above all *your own habits*. If your profile says you open with a rhetorical question or use "Furthermore" as a structural joint, the profile wins over the general rule. Both of those are real bugs the eval caught and fixed.

**Two worked profiles ship with the repo** as examples of the format: [`rufus-pollock.md`](skills/soundlikeme/profiles/rufus-pollock.md) and [`paul-graham.md`](skills/soundlikeme/profiles/paul-graham.md). Read one before writing your own — the [spec](skills/soundlikeme/references/profile-spec.md) explains the shape, but the examples show what a *discriminating* marker looks like.

## Does it actually work?

This is the part that makes the project more than assertion, and it's why the repo is bigger than the skill.

**The method.** Take a piece you published that your profile was *not* built from. Strip it to a de-voiced brief that leaks none of your phrasing. Have the skill draft from the brief. Then have blind judges compare that draft to what you actually wrote — forced choice, run twice with the order swapped and a different judge each way. A result that flips with order is not a result.

**The control group nobody runs:** same brief, profile on versus profile off. If the profile doesn't win, it's decoration.

**Where it stands** after [four runs](evals/results/):

- **Profile-on beats no-profile 6-0**, across two authors, every comparison in both orders, every judge high confidence.
- **Judges pick winners on micro-conventions, not rhetoric** — dash style, connectives, numerals, scare-quote marks, footnote form, spelling convention. Six of six judgments, both authors. This is the most robust finding here, and it's the opposite of what you'd expect.
- **False positives: 0 on 76 protected spans. Detection: 63 of 66 planted tells.** The rate `protections.md` exists to control, measured for the first time on 2026-08-30.

**Two real bugs the eval caught**, neither findable by reading the skill:

- The concision rules were banning "Furthermore" and "Moreover" as filler while the author being imitated uses them as structural joints. The cleanup layer was deleting a voice signal, in a project whose entire premise is that voice survives the cleanup.
- `deslop` deletes where the rules ask it to repair — it removed a post's payoff along with the binary contrast inside it ([#7](https://github.com/rufuspollock/soundlikeme/issues/7), open).

**What it does not yet show.** Two authors is better than one and is not many; both are anglophone essayists writing argumentative nonfiction. The drafters, judges and profile-builder are all the same model family, which nothing here rules out as a confound. And no author has yet confirmed that a profile of them is actually right — that's [#1](https://github.com/rufuspollock/soundlikeme/issues/1).

Full detail in [`evals/README.md`](evals/README.md), the [design rationale](docs/plans/2026-08-29-objective-function-design.md), and the [run write-ups](evals/results/).

## Repo layout

```
skills/soundlikeme/     the skill: router, seven commands, references, profiles
evals/
  cases/                reconstruction cases: brief, reference, metadata
  fixtures/             protection, detection and polish fixtures
  results/              dated run write-ups, with the scored texts kept alongside
docs/                   prior art, research, design plans
changelog/              what has shipped, one file per entry
```

## Contributing

Read [`AGENTS.md`](AGENTS.md) first — it's the guide for both humans and AI agents working in here, and `CLAUDE.md` is a symlink to it. [`NEXT.md`](NEXT.md) is what to do next, written so a fresh session can pick it up cold.

Two conventions worth knowing before you open a PR:

- **Every new tell ships with a protection.** Adding to `tells.md` without adding a false-positive case to `protections.md` in the same edit is how a banlist becomes a blunt instrument. Ideally add a fixture too.
- **Don't grow the banlist.** It has converged across all eight surveyed projects, and [avoid-ai-writing](docs/prior-art.md) has 100KB of it under MIT. Adding entries is not progress here; adding a measurement is.

The most useful contribution right now is a **third author profile** with held-out pieces — ideally one that breaks the current pattern, such as a different register, a non-native English speaker, a technical writer, or a fiction writer. Where the profile mechanism *fails* is more informative than another win.

## Credits

- *The Elements of Style* (1918) is public domain. Packaging it as an agent reference is [obra's idea](https://github.com/obra/the-elements-of-style), and the vendored text is his.
- The tell catalogue draws on [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) and the projects surveyed in [`docs/prior-art.md`](docs/prior-art.md) — particularly [no-ai-slop](https://github.com/petergyang/no-ai-slop) for the self-check idea, [unslop](https://github.com/theclaymethod/unslop) for the minimum-effective-edit contract, and [better-writing](https://github.com/forjd/better-writing) for genre exemptions.
- Eval corpora are published writing used verbatim under fair use for evaluation: [rufuspollock.com](https://rufuspollock.com) and [paulgraham.com](https://www.paulgraham.com).

## License

MIT, as declared in the skill manifest. The vendored *Elements of Style* is public domain.

## Status

Early, and measured rather than asserted. The skill works and the eval has run four times; the honest summary is that the mechanism demonstrably does something on two authors, and how far that generalizes is exactly the open question. See [`NEXT.md`](NEXT.md) and the [open issues](https://github.com/rufuspollock/soundlikeme/issues).
