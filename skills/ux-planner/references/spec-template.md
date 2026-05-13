# UX Spec Template

## Contents

- [Filename](#filename) — naming convention for saved specs
- [Template](#template-copy-this-structure-verbatim) — verbatim spec structure
  - §1 Product Framing · §2 Functional Scope · §3 User Flows
  - §4 Screen Inventory · §5 Per-Screen Briefs · §6 Constraints & Context
  - §7 Design Context · §8 Hand-off to huashu-design (§8.1–§8.8)
  - §9 Open Questions & Assumptions (§9.4 Risks, §9.5 Alternatives)
  - §10 Mobile / Responsive Design Block (conditional)
- [Field-by-field guidance](#field-by-field-guidance) — per-section rationale and gotchas

---

This is the exact markdown structure of the output. All 9 sections must be present (10 if mobile track active). Empty sections are forbidden — fill, infer, or move the gap to §9 "Open questions". §9.5 Considered Alternatives is the only deliberately-empty subsection at first generation (it's the round-trip target for the cjm-canvas Copy button).

The spec is **always English**, regardless of input language.

---

## Filename

`ux-spec-<YYYY-MM-DD>-<short-slug>.md`

Examples:
- `ux-spec-2026-04-28-habit-tracker.md`
- `ux-spec-2026-04-28-doctor-booking.md`
- `ux-spec-2026-04-28-server-metrics-dashboard.md`

Slug = 2-4 hyphenated lowercase words from the product name.

---

## Template (copy this structure verbatim)

```markdown
# <Product Name> · UX Spec

> Generated: YYYY-MM-DD · Source language: <ru | en | mixed>
> Archetype: <mobile-utility | mobile-content | web-productivity | dashboard | landing | internal-tool | extension>

## 1. Product Framing

- **Type:** <Mobile app | Web app | Dashboard | Landing | Internal tool | Browser extension>
- **Audience:** <primary persona — role, demographics, context of use>
- **Core JTBD:** "When <situation>, I want to <motivation>, so I can <outcome>."
- **Success metric:** <retention | conversion | task completion | time-to-value | etc.>
- **Out of scope:** <explicit non-goals — what we're deliberately not building>

## 2. Functional Scope

### Must-have features (v1)

- <feature 1 — one line, action-oriented>
- <feature 2>
- ...

### Nice-to-have (v1.5+)

- <feature>
- ...

### Explicitly out of scope

- <thing we will not build, with one-line reasoning>
- ...

## 3. User Flows

### Flow 1: <Flow name> [primary]

1. **Entry:** <how the user arrives — push notification / home tap / external link / etc.>
2. <Step — user action + system response>
3. <Step>
4. ...
N. **Outcome:** <success state — what the user sees / has accomplished>

**Failure paths:**
- <what can go wrong> → <how the user recovers>

### Flow 2: <Flow name>

(same structure)

### Flow 3: <Flow name>

(same structure)

## 4. Screen Inventory

> IDs MUST be contiguous (S1..SN). If a screen is removed during iteration, renumber rather than leaving gaps. Gaps are forbidden unless explicitly justified in §9 Assumptions.

| ID | Screen | Purpose | Entry points | Key actions |
|----|--------|---------|--------------|-------------|
| S1 | <Home> | <primary purpose in 1 line> | <tab / deeplink / first-launch> | <main 2-3 actions> |
| S2 | ... | ... | ... | ... |

## 5. Per-Screen Briefs

### S1 · <Home>

- **Information hierarchy:** <H1: most prominent → H2: secondary → H3: tertiary>
- **Key elements:** <bulleted list of must-show components, 3-7 items>
- **States:**
  - **Empty:** <what to show when no data>
  - **Loading:** <skeleton | inline spinner | no loading needed>
  - **Error:** <copy + recovery action>
  - **Success:** <happy path layout description>
- **Interactions:** <primary gestures / clicks / keyboard shortcuts>

### S2 · <Screen name>

(same structure)

(repeat for every screen in S1..SN)

## 6. Constraints & Context

- **Platform & breakpoints:** <iOS | Android | both | web responsive | desktop>. For web/responsive, list pixel boundaries:
  - Desktop ≥1280px
  - Tablet 768–1279px
  - Mobile 375–767px
- **Per-breakpoint feature parity** (always required, even if §10 mobile track = No):

| Screen | Mobile (375–767) | Tablet (768–1279) | Desktop (≥1280) |
|--------|------------------|--------------------|------------------|
| S1 | <full / read-only / hidden / desktop-only> | ... | ... |
| S2 | ... | ... | ... |

- **Accessibility:** <WCAG level, contrast, screen reader support, voice nav>
- **Localization:** <ru | en | both | future-ready (i18n hooks needed)>
- **Performance budget:** <e.g., LCP < 2s, list scroll 60fps, no jank on cold start>
- **Auth model:** <none | email | social | passkey | magic link | guest mode>
- **Data sources:** <user input | sync | external API | manual import>
- **Offline behavior:** <full offline | read-only offline | online-required>

## 7. Design Context (for huashu)

- **Existing design system:** <yes — link/path | no>
- **Brand assets available:**
  - Logo: <SVG path | needs collection>
  - Colors: <hex list | "use existing" | needs collection>
  - Fonts: <names | "use existing" | needs collection>
  - Product images / UI screenshots: <paths | needs collection | N/A>
- **References / inspiration:** <competitor URLs | screenshots | "user wants advisor mode">
- **Design direction known:** <yes — describe in 1-2 sentences | no — recommend huashu fallback advisor mode>
- **Brand voice / tone:** <calm / playful / authoritative / humble / etc.>

## 8. Hand-off to huashu-design

This section pre-answers huashu's standard interview. Fill all sub-sections.

### 8.1 Recommended delivery format

Pick exactly one. Default = `cjm-canvas`. Pick `hi-fi-static` only when project meets ALL three skip-conditions: ≤2 screens AND ≤1 flow AND no anon↔authed / multi-state transitions worth exploring.

- [ ] **`cjm-canvas`** — interactive HTML canvas: iframe-wrapped screen preview (with fake browser chrome) + right sidebar with (a) §8.4 tweak toggles filtered to the active screen, (b) numbered CJM flow steps (clickable to switch active screen), (c) alternate states / variant swaps reachable from any step, (d) meta footer (source spec / design system / density), (e) "Copy lock-in prompt" button at the bottom of the sidebar. Default for everything ≥3 screens or ≥2 flows or with anon↔authed transitions.
- [ ] **`hi-fi-static`** — single full-fidelity HTML screen (or single landing page) with no canvas chrome and no flow sidebar. Use only for tiny projects: 1–2 screens, 1 flow, no state branches.

**Reasoning:** <why this format fits this product — reference screen count, flow count, state-branching complexity>

### 8.2 Information density type

Pick one:

- [ ] **Restrained** — give content room to breathe, fewer containers, deliberate whitespace. Default for content apps, marketing landings, simple utilities.
- [ ] **High-density** — at least 3 product-differentiating data points per screen. Required for AI tools, dashboards, trackers, analytics, copilots, health/finance monitoring.

**Reasoning:** <why this density fits this product's value prop>

### 8.3 Per-screen position-4 answers

| Screen | Narrative role | Audience distance | Visual temperature | Capacity check |
|--------|---------------|-------------------|---------------------|----------------|
| S1 Home | hero \| transition \| data \| quote \| end | 10cm phone \| 1m laptop \| 10m projector | calm \| excited \| cold \| authoritative \| warm \| solemn | OK \| risk-tight \| risk-empty |
| S2 ... | | | | |

(repeat for every screen)

### 8.4 Variation dimensions to explore

- **Dimension 1:** <e.g., "Layout: tab-first vs. card-stack" — what to vary>
- **Dimension 2:** <e.g., "Density: restrained vs. data-rich">
- **Dimension 3:** <optional>

**Variation count recommendation:** 3 (default) | 2 | 4

**Reasoning:** <why these axes matter for this product>

### 8.5 Tweaks worth exposing

Parameters huashu should make live-tunable in the prototype. Each tweak MUST be tagged with `[scope: global]` (applies to whole canvas) or `[scope: S<id>, S<id>]` (applies only when those screens are active in the canvas). The cjm-canvas right sidebar filters tweaks by current screen using these tags.

- <e.g., "Primary color (rust / teal / charcoal) [scope: global]">
- <e.g., "Density (compact / comfortable / spacious) [scope: global]">
- <e.g., "Habit list layout (grid / list / calendar) [scope: S1]">
- <e.g., "Save-bar position (top-right / bottom-right / inline) [scope: S2, S4]">

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided (if applicable)
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided
- [ ] **Recommend huashu run §1.a Core Asset Protocol** (if any of the above is missing for a real brand — required even if user said "no brand yet")

### 8.7 Canvas construction hint (for huashu)

> **Reference scaffold:** `skills/ux-planner/assets/canvas-scaffold.html` ships a working v1.3-correct canvas with cross-screen `tweakState`, `touchedKeys` tracking, multi-block `buildLockInPrompt`, live counter badge, empty-state toast, and a `file://` clipboard fallback. Huashu SHOULD start from this scaffold and replace the `// HUASHU: REPLACE` blocks with product-specific JSX from §1–§9. Reinventing the canvas shell from scratch loses subtle v1.3 behaviors (e.g., touched-only emission, conflict-safe Copy).

If §8.1 = `cjm-canvas`, huashu MUST produce a single HTML file (React + Babel via CDN, no build step) with this structure:

**Layout:**
- Left/center stage: iframe (or contentWindow-equivalent component) wrapping the active screen render. Wrap in fake browser chrome (3 traffic-light dots, URL bar showing the product domain from §1, optional secondary bookmarks bar). Above the chrome, a small pill `S<id> · <SCREEN-NAME>` showing the active screen.
- Right sidebar (~360–400px, sticky): contains four blocks in this order:
  1. **Tweaks** — render only §8.5 entries whose `[scope]` tag matches the active screen (or `global`). Each tweak is a labeled toggle group (3 buttons typical). Each toggle group MUST be cross-referenced to its §8.4 dimension when applicable, with the heading format `§8.4 DIM <n> <NAME>` so users see which spec axis they are touching.
  2. **Flow steps** — numbered list of the active CJM flow from §3. Active step is highlighted (filled dot + accent text); inactive steps muted. Clicking a step swaps the active screen in the canvas (and the sidebar re-filters tweaks accordingly).
  3. **Alternate states** — corner-case states / variant swaps reachable from any step. Source: §5 states (non-success branches) + §9 risks. Each row labeled `<screen> · <state>` with a `VARIANT N` or `SWAP` tag.
  4. **Meta footer** — three lines: `SOURCE · ux-spec-<date>-<slug>.md`, `SYSTEM · <design-system from §7 or "—">`, `DENSITY · <RESTRAINED | HIGH-DENSITY>`.
- **"Copy lock-in prompt" button** — sticky at the bottom of the sidebar. Accumulates the user's picks **across every screen they touched in this session** and writes a single merged prompt to clipboard via `navigator.clipboard.writeText`. Show a 2s "Copied · N picks across M screens" toast on success.

**Cross-screen state management (mandatory):**

- All tweak picks are held in a single React state object keyed by `<scope>|<tweak-label>` (e.g., `global|Primary color`, `S2|Save-bar position`). State persists when the user navigates between screens via flow-step clicks — switching screens MUST NOT clear picks made on other screens.
- Maintain a `touchedKeys` Set tracking which `<scope>|<tweak-label>` entries the user has explicitly clicked at least once. Tweaks the user never touched (still at their default) MUST NOT appear in the lock-in prompt — only explicit picks are accumulated.
- Sidebar tweak block continues to render only the active screen's scoped tweaks (filter behavior unchanged). The accumulator runs only at Copy time.

**Copy button UX:**

- Button label includes a live counter: `Copy lock-in prompt · <N> picks across <M> screens` (where N = `touchedKeys.size`, M = number of distinct screen scopes among touched keys, with `global` counted as its own "scope" but not a screen for the M count).
- If `touchedKeys.size === 0`: button is visually muted and on click shows a toast `Pick at least one tweak before copying` instead of writing to clipboard.
- After successful copy: 2s toast `Copied · N picks across M screens`.
- Optional secondary affordance: a tiny `Reset` text-link below the button that clears `touchedKeys` + resets state to defaults (no confirm dialog — fast iteration is the point). Not required, include only if it fits the sidebar visually.

**No page reload, no router.** Switching screens via flow-step click only updates `activeScreenId` + re-filters the sidebar tweak list.

If §8.1 = `hi-fi-static`, huashu produces a single full-fidelity screen with no canvas chrome, no sidebar, no flow nav. Tweaks (if any) sit in a small floating panel; no clipboard prompt button.

### 8.8 Lock-in prompt template (for the cjm-canvas Copy button)

The Copy button accumulates **every tweak the user explicitly touched across every screen they visited** and assembles a single merged prompt. One Copy click = one paste = the whole iteration. The template:

```
Lock these design choices into the UX spec at <ABS-PATH-FROM-§8.1-METADATA>:

Global:
- §8.4 DIM <n> <NAME>: <SELECTED-VARIANT>
- <Tweak label>: <SELECTED-VARIANT>
(emit one line per touched key whose [scope] is `global`)

Screen <S-id-A> · <SCREEN-NAME-A>:
- §8.4 DIM <n> <NAME>: <SELECTED-VARIANT>
- <Tweak label>: <SELECTED-VARIANT>
(emit one line per touched key whose [scope] includes S-id-A)

Screen <S-id-B> · <SCREEN-NAME-B>:
- <Tweak label>: <SELECTED-VARIANT>
(repeat one block per screen the user touched, in S-id order)

Action: update §8.4 — mark these variants as "locked" (globally for Global block, per-screen for Screen blocks) and move non-chosen variants to §9.5 Considered Alternatives. If the same DIM is locked to different variants across blocks, do NOT lock — record the conflict in §9.5 and surface it back to the user. Re-run §6 self-review and regenerate the §8 hand-off phrase.
```

**Emission rules:**

- Omit the `Global:` block entirely if no global tweaks were touched. Same for any Screen block.
- If `touchedKeys.size === 0`, the Copy button does NOT emit a prompt (see §8.7 empty-state toast).
- Screen blocks are ordered by §4 S-id (ascending), not by visit order.
- Within a block, lines preserve the §8.5 declaration order.
- The absolute path of the spec must be embedded into the canvas as a constant (huashu reads it from the user's hand-off message). If the user moves the spec, they re-run the hand-off phrase to regenerate the canvas with the new path.

**Backwards compatibility:** an older single-screen prompt (no `Global:` block, only one `Screen Sx · Name:` block) remains a valid input to Phase 7.5 of this skill. Canvases generated before v1.3 still emit the legacy single-screen format and continue to work. New canvases (v1.3+) MUST emit the multi-block format described above.

## 9. Open Questions & Assumptions

### Assumptions made (verify these)

- **Assumption:** <thing inferred without explicit user confirmation>
- **Assumption:** ...

### Open questions (need user input later)

- **Q:** <unresolved decision> — **why it matters:** <what depends on this>
- **Q:** ...

### Inferred from archetype defaults

- <thing taken from product-archetypes.md without user confirmation, listed for transparency>

### Product Risks

> List 3–6 risks. Categories to consider: API/integration outages, user confusion zones, billing/quota edge cases, data integrity (soft delete, versioning), concurrency conflicts, auth/session edge cases. Each entry = 1 sentence describing the risk + 1 sentence mitigation hint.

- **<Risk name>:** <one-sentence description>. Mitigation — <one-sentence approach>.
- **<Risk name>:** ...

### Considered Alternatives (§9.5)

> Initially empty. Populated automatically when the user pastes a "lock-in prompt" from the cjm-canvas Copy button — non-chosen §8.4 variants are archived here per screen so future iterations remember what was tried and rejected. Format:
>
> - **S<id> · §8.4 DIM <n> <NAME>:** considered `<variant-A>`, `<variant-B>`; locked `<variant-C>` on YYYY-MM-DD.

## 10. Mobile / Responsive Design Block

> **Include this section ONLY if mobile track was confirmed in Phase 2.5. Otherwise omit entirely.**

### 10.1 Mobile-first principles for this product

- **Navigation pattern:** <bottom-tab | hamburger | contextual top nav | drawer>
- **Gesture model:** <swipe / long-press / pull-to-refresh — what gestures are core>
- **Performance budget:** <LCP target on 3G/4G, JS bundle cap, image strategy>

### 10.2 Per-screen mobile adaptation

| ID | Desktop layout | Mobile adaptation | Hidden / collapsed | Mobile-specific gestures |
|----|----------------|-------------------|--------------------|---------------------------|
| S1 | <e.g., sidebar + canvas> | <e.g., bottom-nav + single column> | <e.g., right panel collapses to icon> | <e.g., swipe row to action> |
| S2 | ... | ... | ... | ... |

### 10.3 Touch interactions vs pointer

- Hover-only patterns are replaced by: <long-press / explicit menu button / always-visible action>
- Tap targets ≥44×44pt
- <Swipe directions, pull-to-refresh placement, drag-and-drop notes>

### 10.4 Mobile-only screens or modes (if any)

- <e.g., simplified Sx with "Switch to desktop" prompt for power features>
- <e.g., bottom-sheet variant of a modal that lives as a side-panel on desktop>

### 10.5 Mobile column for §8.3 position-4

| Screen | Mobile audience distance | Mobile capacity check |
|--------|---------------------------|------------------------|
| S1 | 10cm phone | OK / risk-tight / risk-empty |
| S2 | ... | ... |

---

**Hand-off phrase suggestion** (paste into huashu chat):

```
Read this UX spec at <ABS-PATH>. Produce a <cjm-canvas | hi-fi-static from §8.1> exploring <dimensions from §8.4> as variant toggles. Density type: <from §8.2>. Honor §8.3 per-screen position-4 answers and §8.7 canvas construction rules (filtered tweaks per active screen, flow-step nav, alternate-states block, meta footer, "Copy lock-in prompt" button generating §8.8 prompt). <If §7 design direction = "no" → "Use huashu fallback advisor mode to recommend 3 design directions before proceeding."> <If §10 present → "Honor §10 mobile/responsive specifications when designing mobile/tablet variants.">
```
```

---

## Field-by-field guidance

### §1 Product Framing

- **Type** must match one of the 7 archetypes from `product-archetypes.md`.
- **JTBD** uses the standard "When ___, I want to ___, so I can ___" form. Don't skip it — even if vague, it forces clarity.
- **Out of scope** is critical — protects huashu from designing things that aren't real requirements.

### §2 Functional Scope

- Each feature is **one line, action-oriented** (verb + noun + qualifier). Bad: "Notifications". Good: "Send daily 8am reminder to log habit, snoozable to 8pm".
- Cap "must-have" at ~8 features. More than that → split into v1 / v1.1.
- "Out of scope" prevents huashu from designing extra screens you don't need.

### §3 User Flows

- 3-4 flows max for a typical app. More than that → consider splitting product into modules.
- Each flow is **numbered steps**, not prose. Step format: `<user action> + <system response>`.
- "Failure paths" are not optional — every flow has at least one. Empty failure path = blind spot.

### §4 Screen Inventory

- One row per screen. ID format: `S1`, `S2`, etc. (sequential, not by importance).
- **IDs MUST be contiguous** — no gaps. If a screen is removed during iteration, renumber the rest. Self-review in Phase 6 enforces this.
- "Purpose" is one line — what makes this screen exist.
- "Entry points" forces you to think about navigation — if multiple screens share entry points, IA is unclear.

### §5 Per-Screen Briefs

- One section per screen. Skip a screen here = force huashu to invent.
- "Information hierarchy" is the **most important field** — it tells huashu what's hero vs. supporting.
- States: empty/loading/error/success. Don't say "standard" — be specific. "Empty habit list shows 'Tap + to add your first habit' with illustration of a calendar" beats "empty state".

### §6 Constraints

- Be specific about platform — "responsive" means nothing without breakpoints.
- Auth model is critical — onboarding design depends on it.
- Offline behavior changes the entire data layer perception in UI (loading states differ).

### §7 Design Context

- This is huashu's most-asked input. Get it right.
- If user has a design system → write the path to it. Don't paraphrase what's in it.
- If brand exists but assets are missing → say so explicitly: "Brand: Acme Co (logo not provided, recommend huashu run Core Asset Protocol)".
- Direction known/not known toggle determines whether huashu enters its own advisor mode.

### §8 Hand-off to huashu

- The whole point of this skill is this section being complete.
- §8.1 delivery format: pick exactly one of `cjm-canvas` (default) / `hi-fi-static` (only when ≤2 screens AND ≤1 flow AND no anon↔authed / multi-state branching). Don't punt.
- §8.2 density type: pick one — products live or die on this.
- §8.3 position-4 table: every screen, no exceptions.
- §8.4 variation dimensions: 2-3 axes, with reasoning.
- §8.5 tweaks: exposing the right knobs accelerates iteration. **Each tweak MUST carry a `[scope]` tag** (`global` or `S<id>[, S<id>...]`) so the cjm-canvas right sidebar filters them per active screen.
- §8.7 canvas construction hint: tells huashu the exact HTML/React structure of the canvas. Don't simplify it away — that's what makes the deliverable interactive.
- §8.8 lock-in prompt template: defines the clipboard payload of the Copy button. Embed the spec's absolute path so the round-trip back to a Claude session is one paste.

### §9 Open Questions & Assumptions

- Be honest. List every assumption you made on the user's behalf so they can verify.
- "Open questions" = things you decided not to ask in Phase 3. Surface them here so the user knows what they didn't decide.
- "Inferred from archetype defaults" = transparency about what came from `product-archetypes.md` vs. user input.
- **§9.4 Product Risks** — required. List 3–6 risks (API outages, user confusion, billing edges, data integrity, concurrency, auth) with one-sentence mitigation each. Skipping = blind spot.
- **§9.5 Considered Alternatives** — empty at first generation; this is the canonical landing zone for non-chosen §8.4 variants when the user runs the cjm-canvas Copy button and pastes the lock-in prompt back. Always include the empty subsection so the round-trip target exists.

### §10 Mobile / Responsive Design Block (CONDITIONAL)

- Include this section **only if** the user said Yes to the Phase 2.5 mobile question.
- If §10 is included: §5 briefs must each carry a `**Mobile adaptation:**` sub-bullet, §8.3 must include the mobile column, and the §8.6 hand-off phrase must add `Honor §10 mobile/responsive specifications`.
- If §10 is omitted: §6 still carries the per-breakpoint feature parity table (always required), and §9 should record "Mobile UX deferred — covered in a follow-up session" as an Open Question.
