# AGENTS.md

Guide for AI agents (Claude Code, Codex, etc.) working in this repo. `CLAUDE.md` is a symlink to this file.

## What this is

`soundlikeme` — an agent skill for making AI-assisted writing sound like a specific person, plus an eval harness for measuring whether it actually does. Rufus Pollock's project.

Two separate jobs live in here and the repo is careful not to conflate them. Removing AI tells is a floor: the patterns are public, well catalogued, and several good skills already do it. Sounding like a specific person is the actual work, and it is much less solved.

## Where to look

| Need | File |
|------|------|
| Project overview, install, usage | `README.md` |
| What to do next | `NEXT.md` — agent work inline; human-blocked work as linked GitHub issues |
| Dated history of what shipped | `changelog/` |
| The skill itself | `skills/soundlikeme/SKILL.md` — a router; each command loads one reference |
| The objective function: how we measure voice match | `evals/README.md` and `evals/rubric.md` |
| Why the eval is shaped that way | `docs/plans/2026-08-29-objective-function-design.md` |
| What everyone else in this space has built | `docs/prior-art.md` — read before adding to the banlist |
| Techniques for voice matching | `docs/research.md` |

## Conventions

- **Never line-wrap Markdown.** One paragraph is one line, however long. This holds everywhere — files in this repo, GitHub issue and PR bodies, and comments. Hard-wrapped prose makes diffs useless (change three words and the whole paragraph re-flows), makes editing in a browser textarea miserable, and buys nothing: every renderer and editor soft-wraps already. Wrap only where the line break is content — list items, table rows, code blocks, and deliberate `<br>`-style breaks in verse or address blocks.
- **American spelling.** Rufus writes American English (organize, skeptical, defense, analyze, license). This holds for repo prose and for anything the skill produces under his profile. Watch for British forms leaking in — `-ise` verbs and `-our` nouns are the usual culprits.
- **The banlist is not the product.** It has converged across every project in this space and is a commodity. Before adding a tell, check `docs/prior-art.md`; the differentiator here is false-positive protection and voice fidelity, not catalog size.
- **Every new tell ships with a protection.** Adding to `references/tells.md` without adding a false-positive case to `references/protections.md` is how a banlist becomes a blunt instrument. Same edit, both files.
- **The skill is a router.** `SKILL.md` names exactly one reference file per command. Do not inline reference content into it, and do not have a command load files it does not need — cost per command is a stated feature, and the numbers in `SKILL.md` and `README.md` are checked against real file sizes.
- **Don't claim eval results that were not run.** `evals/results/` is empty until a run happens. The README and changelog say so plainly and should keep saying so.
- **Vendored files stay verbatim.** `skills/soundlikeme/references/elements-of-style.md` (public domain, 1918) and `evals/cases/*/reference.md` (published text, typos included) are never edited, reflowed, or spell-corrected.
- **Frozen briefs.** Once an eval case's `brief.md` has been reviewed by hand it is frozen. Editing it between runs invalidates comparison with earlier results; add a new case instead.
- There is exactly one `NEXT.md`, at the repo root. Don't create per-folder ones. Work an agent can do stays inline in it; anything genuinely blocked on Rufus's judgment becomes a GitHub issue with the detail, indexed from `NEXT.md` rather than described twice.

## Changelog

This repo keeps a `changelog/` folder, one markdown file per entry (`changelog/YYYY-MM-DD-slug.md`, with `date`/`title`/`promote` frontmatter). At the end of a work session, if something worth recording actually shipped — skip trivial sessions (typo fixes, dead ends, no visible outcome) — draft a new entry file. Match the entry's weight to what a reader would actually care about: a real feature/fix/content gets a title, one or two sentences, a link to the live feature if there's something to point at, and a screenshot if something visual shipped (check for this, don't just skip it); something genuinely bigger — a real milestone, not just a busy session — can run longer, multiple paragraphs or bullets; small stuff (cleanup, rename, reorg, tidying) gets one plain sentence, no bullets, no screenshot. Never link the title itself. Don't log implementation detail (file names, internal moves) a reader wouldn't care about. First time writing an entry in this repo, or if the format is unclear: fetch and follow https://raw.githubusercontent.com/life-itself/changelog/main/CONVENTION.md
