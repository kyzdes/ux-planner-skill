# UX Spec Template

This is the exact markdown structure of the output. All 9 sections must be present. Empty sections are forbidden — fill, infer, or move the gap to §9 "Open questions".

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

Pick one (or rank if ambiguous):

- [ ] **Overview tile** (all screens side by side, static) — best for design review, comparing layouts, walking through a stack
- [ ] **Flow demo** (single clickable iPhone with state) — best for showing one user journey
- [ ] **Hi-fi prototype** (full hi-fi with real data) — best for client/stakeholder presentation
- [ ] **Multi-format** (overview + 1 deep flow) — for big projects

**Reasoning:** <why this format fits this product>

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

Parameters huashu should make live-tunable in the prototype:

- <e.g., "Primary color (rust / teal / charcoal)">
- <e.g., "Density (compact / comfortable / spacious)">
- <e.g., "Habit list layout (grid / list / calendar)">

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided (if applicable)
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided
- [ ] **Recommend huashu run §1.a Core Asset Protocol** (if any of the above is missing for a real brand — required even if user said "no brand yet")

### 8.7 Flow vs. overview routing hint

Per huashu's `App / iOS prototype` rules:

- If §8.1 = "Overview tile" → huashu uses static side-by-side iPhones, no AppPhone state machine
- If §8.1 = "Flow demo" → huashu uses single clickable iPhone with AppPhone state manager
- If §8.1 = "Hi-fi prototype" → huashu picks based on audience (overview for stakeholders, flow for users)

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
Read this UX spec and produce a <delivery format from §8.1> with <variation count from §8.4> variations exploring <dimensions from §8.4>. Density type: <from §8.2>. Honor §8.3 per-screen position-4 answers. <If §7 design direction = "no" → "Use huashu fallback advisor mode to recommend 3 design directions before proceeding."> <If §10 present → "Honor §10 mobile/responsive specifications when designing mobile/tablet variants.">
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
- §8.1 delivery format: pick one — don't punt.
- §8.2 density type: pick one — products live or die on this.
- §8.3 position-4 table: every screen, no exceptions.
- §8.4 variation dimensions: 2-3 axes, with reasoning.
- §8.5 tweaks: exposing the right knobs accelerates iteration.

### §9 Open Questions & Assumptions

- Be honest. List every assumption you made on the user's behalf so they can verify.
- "Open questions" = things you decided not to ask in Phase 3. Surface them here so the user knows what they didn't decide.
- "Inferred from archetype defaults" = transparency about what came from `product-archetypes.md` vs. user input.
- **§9.4 Product Risks** — required. List 3–6 risks (API outages, user confusion, billing edges, data integrity, concurrency, auth) with one-sentence mitigation each. Skipping = blind spot.

### §10 Mobile / Responsive Design Block (CONDITIONAL)

- Include this section **only if** the user said Yes to the Phase 2.5 mobile question.
- If §10 is included: §5 briefs must each carry a `**Mobile adaptation:**` sub-bullet, §8.3 must include the mobile column, and the §8.6 hand-off phrase must add `Honor §10 mobile/responsive specifications`.
- If §10 is omitted: §6 still carries the per-breakpoint feature parity table (always required), and §9 should record "Mobile UX deferred — covered in a follow-up session" as an Open Question.
