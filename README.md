# ux-researcher-skill

A [Claude Code](https://claude.com/claude-code) skill that turns a loose product description (in any language) into a structured **English UX Spec** ready to hand off to a visual-design agent like [`huashu-design`](https://github.com/example/huashu-design).

> The technical skill identifier (in `SKILL.md` frontmatter) is `ux-planner`. The repo is named `ux-researcher-skill` because the skill's job is closer to UX research / product framing than visual planning.

---

## What it does

You describe what you want to build:

> "Хочу приложение для трекинга привычек"
>
> "I need a SaaS dashboard for screening resumes"

The skill runs an 8-phase workflow:

1. **Phase 0** — detects product archetype (1 of 7), creates a TaskCreate plan to track progress
2. **Phase 1** — asks a single batch of 5 framing questions (audience, JTBD, platform, design context). Skipped automatically if the project already has `context-map.md` / `CLAUDE.md` / project memory describing the product
3. **Phase 2** — drafts features, flows, and screens from archetype defaults so you edit instead of answering 30 atomic questions
4. **Phase 2.5** — single yes/no question: *do you want a mobile/responsive UX track?* If yes, a dedicated §10 Mobile Block is added to the spec with per-screen mobile adaptation, touch interactions, and mobile-only states
5. **Phase 3** — adaptive follow-ups about gaps (auth, monetization, empty states, notifications, etc.). Hard-capped at 5 questions per round, max 2 rounds, ≤8 total
6. **Phase 4–5** — synthesizes per-screen briefs (information hierarchy, key elements, states, position-4 answers for huashu)
7. **Phase 6** — generates the spec, runs a mandatory self-review checklist (sequential IDs, brief-count match, density type set, etc.)
8. **Phase 7** — outputs a hand-off code block ready to paste into huashu, and saves an auto-memory pointer so the next session keeps context

When the description is too vague, the skill switches into **advisor mode** and proposes 2–3 differentiated archetypes with tradeoffs.

---

## Why it exists

Without this skill, every time you go to a visual-design agent like huashu you re-answer its 10 standard questions (design context, variations, fidelity, scope, tweaks, position-4 per screen, density type, brand assets). The spec produced by `ux-planner` pre-answers all of them in §8 "Hand-off to huashu-design" — so huashu skips its interview and goes straight to design.

It also forces you to think about edge states (empty / loading / error / success) per screen — the most common gap in AI-assisted product design.

---

## Output

A markdown file at `<project>/ux-spec-<YYYY-MM-DD>-<slug>.md` with 9–10 sections:

1. Product Framing
2. Functional Scope
3. User Flows
4. Screen Inventory
5. Per-Screen Briefs
6. Constraints & Context (with per-breakpoint feature parity table)
7. Design Context (for huashu)
8. **Hand-off to huashu-design** (the critical section)
9. Open Questions & Assumptions (with §9.4 Product Risks)
10. **Mobile / Responsive Design Block** (conditional, only if confirmed in Phase 2.5)

Plus a hand-off message — a fenced code block — you paste into a fresh huashu-design session.

---

## Scope

✅ Mobile apps (utility, content)
✅ Web apps (productivity, dashboards)
✅ Internal tools / admin panels
✅ Landing / marketing sites
✅ Browser extensions
✅ Redesigning existing products (reads `context-map.md` / `CLAUDE.md` / memory and skips Phase 1)

❌ Slide decks, animations, infographics — those go directly to huashu-design
❌ PRDs, business cases, market sizing — out of scope
❌ Visual design execution — that's huashu's job

---

## Installation

### Global (recommended)

Clone into `~/.claude/skills/`:

```bash
git clone https://github.com/kyzdes/ux-researcher-skill.git ~/.claude/skills/ux-planner
```

The folder name on disk should be `ux-planner` (matching the skill identifier in `SKILL.md` frontmatter), even though the repo is named `ux-researcher-skill`.

### Per-project

Drop the folder into a project's local skills directory if your harness supports it.

---

## Trigger phrases

The skill triggers on (Russian): *хочу приложение*, *нужен dashboard*, *сделай UX*, *помоги придумать*, *продумай интерфейс*, *сделай app*, *разработать продукт*.

The skill triggers on (English): *design a*, *build me*, *I want an app for*, *need a dashboard*, *make a SaaS*, *feature spec for*, *UX spec*, *user flows for*.

---

## Test it

In a fresh Claude Code session (with the skill installed):

```
Хочу сделать сервис записи к врачу онлайн
```

Expected behavior: detect mobile/web borderline archetype → issue 5-question batch → draft features/flows/screens → ask the mobile-track question → adaptive follow-ups → save English spec → output hand-off code block.

Five canonical test prompts (with verifiable `expectations[]`) live in `skills/ux-planner/evals/evals.json`, following the [skill-creator](https://github.com/anthropics/claude-plugins-official) schema so they can be replayed via `run_eval.py` / `aggregate_benchmark.py`.

---

## How it differs from huashu-design

| Aspect | ux-planner (this skill) | huashu-design |
|---|---|---|
| Layer | Product / UX research | Visual design / dev |
| Output | Markdown spec | HTML / interactive prototypes |
| Asks about | Audience, features, flows, states | Colors, fonts, layouts, components |
| Trigger | "хочу сделать X" | "сделай красивый UI / make a hi-fi prototype" |
| Time | 5–10 min interview | 30–60 min design |

Use them in sequence: `ux-planner` → spec → `huashu-design`.

---

## Repo layout

```
ux-researcher-skill/                            # repo
├── README.md                                   # this file
├── LICENSE
├── .claude-plugin/plugin.json                  # plugin manifest
├── hooks/hooks.json                            # SessionStart auto-update hook
├── scripts/auto-update.sh                      # marketplace refresh script
└── skills/
    └── ux-planner/                             # the skill itself (folder name = SKILL.md `name`)
        ├── SKILL.md                            # main workflow + interview logic (8 phases + 7.5 re-entry)
        ├── evals/
        │   └── evals.json                      # 5 canonical test cases with expectations[] (skill-creator schema)
        ├── assets/
        │   └── canvas-scaffold.html            # reference cjm-canvas HTML (React+Babel CDN, v1.3-correct)
        └── references/
            ├── interview-flow.md               # batch templates + Phase 2.5 mobile question + adaptive triggers + advisor mode
            ├── product-archetypes.md           # 7 product archetypes + redesign-existing branch
            ├── ux-patterns.md                  # common UX pattern library
            ├── spec-template.md                # output format + §10 Mobile Block (conditional)
            ├── handoff-to-huashu.md            # hand-off phrasing + pointer to SKILL.md SSOT self-review
            └── examples/
                ├── habit-tracker-spec.md
                ├── saas-analytics-spec.md
                └── landing-product-spec.md
```

---

## Status

v1.3 — current release. Additions over v1.2:

- **Multi-screen Copy button accumulator.** The cjm-canvas "Copy lock-in prompt" button now collects every tweak the user explicitly touched **across all screens they visited**, not just the active one. One Copy click = one paste = the entire iteration locked in.
- **§8.8 multi-block prompt format.** Optional `Global:` block (for `[scope: global]` picks) + one `Screen Sx · Name:` block per screen the user touched. Legacy single-screen prompts from pre-v1.3 canvases remain valid input — Phase 7.5 accepts both formats.
- **Live counter on the Copy button.** Label reads `Copy lock-in prompt · N picks across M screens` so the user sees exactly how much is queued. Empty-state click shows a toast `Pick at least one tweak before copying` and skips the clipboard write.
- **Conflict detection in Phase 7.5.** If the user picked different variants for the same DIM on different screens, that DIM is NOT locked — the conflict is recorded in §9.5 with both variants kept open, and surfaced in the user-facing summary so they can resolve manually.

Carried over from v1.2:

- **§8.1 delivery format = `cjm-canvas` by default** (interactive HTML canvas with iframe-wrapped screen + right sidebar with §8.4 tweaks filtered per active screen, clickable CJM flow nav, alternate-states block, meta footer). Falls back to `hi-fi-static` only when ≤2 screens AND ≤1 flow AND no auth/state branching.
- **§8.5 tweaks require `[scope: ...]` tags** so the canvas sidebar filters them by active screen.
- **§8.7 canvas construction hint** — explicit four-block sidebar spec for huashu (Tweaks / Flow steps / Alternate states / Meta footer).
- **§8.8 lock-in prompt template + Copy button** at the canvas sidebar bottom — round-trip back to a Claude session via clipboard. Pasting the prompt re-enters the skill at Phase 7.5 and updates §8.4 (locked variants) + §9.5 (archived alternatives).
- **§9.5 Considered Alternatives** — the canonical landing zone for non-chosen variants when the user iterates.
- **Phase 7.5** — re-entry path for lock-in prompts (skip Phases 0–5, targeted spec edit only).

Carried over from v1.1:

- Phase 2.5 Mobile / Responsive scope confirmation
- §10 Mobile Design Block (conditional)
- §9.4 Product Risks (mandatory)
- Per-breakpoint feature parity table (always required)
- Existing-project branch (skip Phase 1 when `context-map.md` / memory exists)
- Self-review hard gate before hand-off
- Memory pointer step in Phase 7
- Hand-off phrase as fenced code block (was blockquote)

Future iterations:

- Description optimization via `skill-creator`'s `run_loop.py`
- More archetype examples (B2B SaaS, e-commerce, edtech)
- Optional bilingual spec output (currently English-only)

---

## License

MIT.
