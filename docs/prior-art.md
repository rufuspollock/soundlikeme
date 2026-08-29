# Prior Art: AI-slop removal and voice-matching skills

Survey of what already exists (Aug 2026), what each does well, and what we should steal.

Companion to `research.md`, which covers *techniques*. This covers *shipped implementations*.

## 1. The field

Eight projects matter. They sit on a spectrum from "one markdown file" to "skill + deterministic scanners + eval harness + CI".

| Project | Size | What it is | Standout idea |
|---|---|---|---|
| [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) | 2.6KB SKILL + 3 refs | The minimal ancestor. 8 rules, 12 quick checks, 5-dimension score out of 50 | Scoring rubric as a stopping condition |
| [stephenturner/skill-deslop](https://github.com/stephenturner/skill-deslop) | 8KB SKILL + 44KB refs | stop-slop expanded, aimed at scientific writing | Domain awareness — passive voice is *correct* in a methods section |
| [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop) | 1 SKILL + `eval.md` | Best-written single-file editor skill | `eval.md`: a self-check the model runs against its own output before returning |
| [forjd/better-writing](https://github.com/forjd/better-writing) | 6KB SKILL + 6 refs + eval fixtures | Genre-aware editor | Dials, not blanket rules; confidence tiers; "flat is a tell too" |
| [conorbronsdon/avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing) | 100KB SKILL + 120KB JS detector | The maximalist. 112-word table, 69 pattern categories, 51 engines | Tiered vocabulary + preservation validator + MCP server |
| [theclaymethod/unslop](https://github.com/theclaymethod/unslop) | SKILL + 20 refs + 20 Python scripts + eval suite | The most ambitious. Audit / rewrite / **teach** / **mimic** | Voice building from a harvested corpus, stylometric scoring, single behavior contract |
| [realrossmanngroup/no_ai_slop_writing_rules](https://github.com/realrossmanngroup/no_ai_slop_writing_rules) | CLAUDE.md + skills | One person's voice, derived from 513k words of their own corpus | Quantitative voice profile: number density, sentence-length variance, claim-then-proof paragraph shape |
| [obra/the-elements-of-style](https://github.com/obra/the-elements-of-style) | thin SKILL + 71KB reference | Strunk, packaged as a skill | Explicit token warning + "dispatch a subagent to copyedit when context is tight" |

Also relevant but not a skill: [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) — the community-maintained canonical list. unslop has a `wiki_sync.py` that pulls from it.

## 2. What everyone agrees on

The banlists have converged. Across all eight, the same items recur, and our current `write-like-me` Step 3 already has most of them:

- **Words:** delve, tapestry, realm, leverage, utilize, facilitate, foster, robust, streamline, empower, harness, elevate, embark, paradigm shift, game-changer, transformative, meticulous, intricate, multifaceted, ever-evolving
- **Phrases:** "it's worth noting", "in today's X landscape", "at the end of the day", "let's dive in", "the reality is"
- **Structures:** binary contrast ("not X, it's Y"), negative listing, dramatic fragmentation, self-answered rhetorical question, colon reveal, tricolon/snappy triad, fake-profound kicker, summary-recap ending, superficial `-ing` analysis ("highlighting its commitment to..."), weasel attribution ("experts agree"), importance puffery, em-dash abuse, bold-first bullets

**Conclusion: the banlist is a commodity.** Building a bigger one is not a differentiator. avoid-ai-writing has 100KB of it and open-sourced it.

## 3. What separates the mature ones

This is where the real learning is. Every project's v1 is a banlist. The mature projects spend most of their complexity on *not firing*.

### 3.1 False-positive protection is the actual hard problem

A naive banlist produces a new, equally detectable style: clipped, staccato, opinion-flattened, all character sanded off. unslop names this explicitly and bans it ("Do not create staccato anti-slop prose"). Three different mechanisms:

- **avoid-ai-writing — tiered vocabulary.** Tier 1A fires on sight (`delve`). Tier 1B is a clarity swap (`utilize`→`use`). Tier 2 fires only *in a cluster*. Tier 3 fires only at high density or when 3+ boilerplate phrases stack. A single hit is not evidence.
- **unslop — the decision rule.** "A scanner match alone never authorizes an edit." Protect literal, domain-valid, quoted, attributed, or genre-natural uses. "Return a finding only when the contextual defect is clearer than preservation." Prefer a no-op to an uncertain edit.
- **better-writing / deslop — genre exemptions.** Passive voice is right in a methods section. Warmth is right in a support email. "Never edit on a single feature." "The most durable tell is uniform tone that never adapts to audience or genre."

### 3.2 Minimum effective edit + byte-for-byte preservation

unslop: *"Edit only sentences with confirmed findings, using the smallest repair. Copy every other sentence byte-for-byte in its original order and paragraph. With no findings, return the source exactly."*

petergyang: *"A rough draft with a real voice should still sound like the same person after editing."*

Cheap to adopt, big effect. Our current skill says "rewrite it to match the voice profile" — that licenses total reconstruction, which is the thing that destroys voice.

### 3.3 Self-evaluation before returning

petergyang's `eval.md` is the single cheapest high-value idea in the field. ~30 pass/fail questions the model checks its own output against, then fixes and re-checks. No second agent, no scripts. It converts "follow these rules" into "verify you followed these rules".

stop-slop and deslop do the lighter version: score 1–10 on Directness / Rhythm / Trust / Authenticity / Density; below 35/50, revise.

### 3.4 Deterministic scanners do detection; the LLM does judgement

unslop ships `banned_phrase_scan.py`, `structure_scan.py`, `silhouette_scan.py`, `readability_metrics.py`, `voice_score.py`, `validate_preservation.py`, `diff_check.py`. avoid-ai-writing ships the same in JS plus an MCP server exposing `score_text` / `audit_text`.

Why this matters: regex and statistics are free, exact, and testable. The model then only adjudicates the hits. It also makes the skill *evaluable* — you can regression-test it.

`validate_preservation.py` / `validate.js` are the interesting ones: they prove the rewrite didn't touch code blocks, YAML, blockquotes, tables, URLs, file paths, headings, quantities, dates, names, or citations.

### 3.5 Voice as a measurement, not a vibe

The weakest area across the field, and the one closest to what this repo is for.

- **unslop `teach`:** harvest a corpus (from writing folders *and chat transcripts*, dropping assistant turns so AI text doesn't contaminate the profile) → user approves samples → emits a machine profile (`profile.json`) plus a layered human-readable voice card (`card.md`).
- **unslop `calibrate`:** an A/B game. Show the user pairs, ask which is theirs, use the answers to sharpen the profile. Turns voice extraction into an interactive loop instead of a one-shot guess.
- **unslop `silhouette_scan.py` / `voice_score.py`:** stylometric fingerprint — sentence-length variance, punctuation distribution, function-word entropy. Gives a number for "does this sound like the target".
- **Rossmann profile:** derived from 513,683 words. Concrete measurable markers — testable-number density, sentence-length variance, claim-then-proof paragraph structure, contraction rate.

Our `voice-profile.md` is qualitative prose. Good, but unfalsifiable. The field's direction is qualitative card + quantitative fingerprint.

### 3.6 Progressive disclosure and levels

Everyone converged on modes, and they map onto the levels you wanted:

| Our level | unslop | avoid-ai-writing | better-writing |
|---|---|---|---|
| Detect only, no edit | `cleanup --report` | Detect (P0/P1/P2 severity) | review request |
| Minimal in-place fix | `cleanup` | Edit mode | rewrite |
| Full rewrite | `rewrite` (two-pass) | Rewrite (two-pass) | stricter "de-AI" pass |
| Draft *in* the voice | `mimic` | voice profiles | voice calibration |
| Build the voice | `teach` | — | — |

Two-pass rewriting shows up twice independently: first pass removes the obvious patterns, second pass catches the tells the first pass *introduced*.

## 4. What `impeccable` contributes

Not the same problem, but the architecture transfers almost one-to-one:

| impeccable | soundlikeme equivalent |
|---|---|
| Thin SKILL.md that is a **router**: commands table → one reference file per command | Same. Never load the whole thing |
| `craft-floor.md` loaded *immediately before editing*, explicitly "do not load for planning-only work" | Load the banlist and Strunk reference only at the rewrite step |
| **Modes** (Persuade / Operate / Read / Experience) chosen per surface | Genre/register (essay, email, talk, README, social post). better-writing already does this |
| `PRODUCT.md` + `DESIGN.md` as durable per-project context, generated by `init` / `document` | `voice-profile.md`, generated by `voice-extractor` |
| "Verify in **bounded passes, not a loop**" — build, inspect once, fix in one batch, one confirm round, stop | Directly applicable. Anti-slop passes are exactly the kind of thing a model will grind on forever |
| Detector **hook** that runs after file edits and surfaces findings | A post-write hook running a slop scanner |
| `doctor` — reports drift between artifacts and current version, but never repairs as a side effect | Profile staleness checking |
| Sub-agents for the expensive finish passes | obra's advice too: dispatch a subagent to copyedit with the 12k-token style guide |

The single most transferable lesson: **the skill is a router, the content is in references, and the router names exactly one reference per job.**

## 5. Strunk & White: which version

`write-like-me` currently has 12 hand-distilled rules with examples, and points at obra's repo as optional.

obra's actual shape: a ~40-line SKILL.md that is just an index of the rule names, plus `elements-of-style.md` at 71KB (~12k tokens), with an explicit warning to read it only when actually editing prose, and a fallback of dispatching a subagent when context is tight. MIT-adjacent, has a LICENSE, worth attributing.

**Recommendation:** keep our 12 distilled rules as the always-loaded concision layer (they are good, and they carry examples that obra's index does not), and vendor obra's `elements-of-style.md` as an opt-in deep reference for the high-stakes path. Do not merge the two — the whole point is that one is 300 tokens and the other is 12,000.

## 6. Where soundlikeme is differentiated

Slop removal is solved to the point of commodity. Voice is not.

- Of the eight, only unslop and the Rossmann repo do serious voice work, and only unslop is generic.
- Nobody combines: corpus-derived voice profile + interactive calibration + quantitative voice scoring + tiered levels + a concision floor.
- Nobody handles the **organisational** case well (shared voice across multiple authors), which the README already claims as a use case.

So: don't try to out-banlist `avoid-ai-writing`. Compete on voice fidelity, on honest levels (cheap pass vs. full pass), and on the extraction/calibration loop.

## 7. Concrete steals, ranked by value/effort

1. **`eval.md` self-check.** One file. Biggest single quality jump. (petergyang)
2. **Minimum-effective-edit + byte-for-byte preservation rule.** Two sentences in the SKILL. Fixes the "rewrite destroys voice" failure. (unslop)
3. **Levels / router architecture.** Restructure SKILL.md as a thin router over references. (impeccable, unslop)
4. **False-positive protections: tiers, genre exemptions, "a match alone never authorizes an edit".** (avoid-ai-writing, unslop, better-writing)
5. **Genre/register dials** instead of blanket rules. (better-writing)
6. **Two-pass rewrite**, second pass catching tells the first introduced. (unslop, avoid-ai-writing)
7. **Vendored `elements-of-style.md`** as opt-in deep reference. (obra)
8. **Quantitative voice fingerprint** alongside the prose profile. (unslop silhouette, Rossmann)
9. **Interactive calibration (A/B quiz)** to sharpen an extracted profile. (unslop)
10. **Deterministic scanner scripts + eval fixtures with false-positive protections.** Highest effort, but it is what makes the skill testable rather than vibes. (unslop, avoid-ai-writing)

## 8. Open questions this survey did not settle

- Does a quantitative voice score actually correlate with "sounds like me" as judged by the author? unslop has an eval for it (`check_mimic.py`, `LIVE-MIMIC-PROTOCOL.md`) — worth reading before building our own.
- How much of the banlist should be vendored vs. linked? avoid-ai-writing is MIT and its patterns file is 120KB.
- Is the deterministic-scanner path worth it for a repo this size, or does it become the maintenance burden that kills the project?

## Sources

- [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)
- [stephenturner/skill-deslop](https://github.com/stephenturner/skill-deslop)
- [petergyang/no-ai-slop](https://github.com/petergyang/no-ai-slop)
- [forjd/better-writing](https://github.com/forjd/better-writing)
- [conorbronsdon/avoid-ai-writing](https://github.com/conorbronsdon/avoid-ai-writing)
- [theclaymethod/unslop](https://github.com/theclaymethod/unslop)
- [realrossmanngroup/no_ai_slop_writing_rules](https://github.com/realrossmanngroup/no_ai_slop_writing_rules)
- [obra/the-elements-of-style](https://github.com/obra/the-elements-of-style)
- [labarba/sciwrite](https://github.com/labarba/sciwrite)
- [yzhao062/agent-style](https://github.com/yzhao062/agent-style)
- [danielrosehill/My-Tone-Of-Voice](https://github.com/danielrosehill/My-Tone-Of-Voice)
- [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- [Peter Yang: Use My /No-AI-Slop Skill](https://creatoreconomy.so/p/use-my-no-ai-slop-skill-to-remove-20-ai-slop-patterns)
- [Gabriel Cassady: Stop Slop](https://gabrielcassady.com/tools/stop-slop-claude-skill-to-remove-ai-writing-tells/)
