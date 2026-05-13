# Atlas · UX Spec

> Generated: 2026-04-28 · Source language: en
> Archetype: landing

## 1. Product Framing

- **Type:** Landing / marketing site
- **Audience:** Independent developers and small dev teams (2-10 people) evaluating new dev tools. They reach the page from Twitter / Hacker News / dev newsletter referrals. Tech-literate, allergic to marketing fluff.
- **Core JTBD:** "When I see a new dev tool mentioned on HN, I want to grok in 30 seconds whether it solves my problem and how it differs from alternatives, so I can decide to bookmark, sign up, or move on."
- **Success metric:** Sign-up conversion (CTA click → email captured), 8% target. Secondary: time on page > 45s.
- **Out of scope:** Blog, changelog, docs, customer dashboards, billing — all live on subdomains.

## 2. Functional Scope

### Must-have features (v1)

- Hero section with headline, sub-headline, primary CTA above fold
- Feature highlights (4 features, with icons + 2-line descriptions)
- "How it works" 3-step explainer
- Social proof: customer logos + 2 testimonials
- Pricing table (3 tiers, monthly/yearly toggle)
- FAQ (8 questions, accordion)
- Footer with legal, contact, secondary nav
- Email capture (sticky on scroll past 50%)
- Cookie banner (GDPR-compliant)

### Nice-to-have (v1.5+)

- Embedded product demo video (auto-play muted)
- Live metrics dashboard widget (real customer activity)
- Interactive code playground in features section

### Explicitly out of scope

- Live chat widget (premature for v1, support via email)
- Personalized hero based on referrer (over-engineering for launch)
- A/B testing framework (set up post-launch)

## 3. User Flows

### Flow 1: Cold visitor → email capture [primary]

1. **Entry:** External link (HN, Twitter, newsletter) → land on page top
2. Read hero in 5-10 seconds → assess fit
3. Scroll to features → confirm relevance
4. Scroll to pricing → check viability
5. Scroll-triggered email capture appears at 60% page depth
6. **Outcome:** Email captured, "thanks, we'll send your trial link" inline confirmation

**Failure paths:**
- User scrolls past email capture without clicking → no capture, but we re-show on FAQ section
- User clicks CTA but bounces from form → form is single-field (email only) to minimize abandonment

### Flow 2: Pricing-first visitor

1. **Entry:** Direct link to `/pricing` anchor or pricing nav click
2. Compare 3 tiers
3. Toggle monthly/yearly
4. Click "Start free trial" on chosen tier
5. **Outcome:** Routed to external signup page with tier preselected

**Failure paths:**
- Confused by tiers → FAQ link inline, contact-sales button on enterprise tier

### Flow 3: FAQ-driven visitor

1. **Entry:** Search engine landing on FAQ schema results
2. Read specific FAQ
3. Browse related FAQs (accordion)
4. CTA at end of FAQ
5. **Outcome:** Either email capture or external signup

## 4. Screen Inventory

| ID | Screen | Purpose | Entry points | Key actions |
|----|--------|---------|--------------|-------------|
| S1 | Landing (single page) | Convert visitors to email captures or sign-ups | External links, search, direct | Read, click CTA, capture email |

(Single-page site — sections, not screens.)

## 5. Per-Screen Briefs

### S1 · Landing

- **Information hierarchy:**
  - H1: Hero headline (2 lines max, ~10 words)
  - H2: Sub-headline (1 line, ~15 words)
  - H3: Primary CTA button
  - H4: Secondary content (features, pricing, etc.) below fold

- **Key elements (top to bottom):**
  - Sticky top nav (logo, features, pricing, docs link, sign in)
  - Hero: headline + sub-headline + primary CTA + product visual (right side)
  - Feature grid: 4 cards (icon + 1-line title + 2-line desc)
  - "How it works" 3-step (numbered, illustrated)
  - Social proof: 6-8 customer logos in monochrome row
  - Testimonial cards (2 quotes, attribution + role)
  - Pricing table (3 tiers, monthly/yearly toggle, feature comparison)
  - FAQ accordion (8 Q's, expandable)
  - Footer (4 columns: product, company, legal, social)
  - Sticky email capture bar (appears at 60% scroll)

- **States:**
  - **Empty:** N/A (static content always present)
  - **Loading:** Above-fold content critical-CSS inlined; below-fold lazy-loaded with subtle fade-in
  - **Error:** Email capture failure → inline red text "Something went wrong, retry" with retry button
  - **Success:** Email submission → button morphs to "✓ check inbox" for 3s, then resets

- **Interactions:**
  - Smooth scroll on nav clicks
  - Pricing toggle (monthly ⇄ yearly) updates table inline
  - FAQ accordion expand/collapse with keyboard arrow nav
  - Email capture form submit (single field + button)
  - Hover states on CTAs (subtle scale + shadow lift)

## 6. Constraints & Context

- **Platform:** Web responsive
- **Devices:** Mobile (320px+) → desktop (1440px+). Tablet treated as small desktop.
- **Accessibility:** WCAG 2.1 AA. All interactions keyboard-accessible. CTAs have aria-labels. Color contrast 4.5:1 minimum.
- **Localization:** English v1. i18n hooks ready (translatable strings, but no second language shipped).
- **Performance budget:** LCP < 1.5s on 3G, total page weight < 800kb, no jank on scroll.
- **Auth model:** None (this is marketing). Sign-up CTA routes to external auth flow.
- **Data sources:** Static content + email capture POST to backend
- **Offline behavior:** N/A (online-required marketing site)

## 7. Design Context (for huashu)

- **Existing design system:** No
- **Brand assets available:**
  - Logo: needs collection
  - Colors: needs collection (recommend tech-developer palette: charcoal + single accent)
  - Fonts: needs collection (recommend mono + sans pairing for dev audience)
  - Product images / UI screenshots: needs collection from product team
- **References / inspiration:** Linear.app (clean dev marketing), Vercel.com (minimal hero), Plain.com (typography-first)
- **Design direction known:** No — recommend huashu fallback advisor mode (suggest 3 directions: minimalist editorial / brutalist developer / warm humanist)
- **Brand voice / tone:** Direct, confident, no marketing fluff. Talks to devs as peers, not prospects.

## 8. Hand-off to huashu-design

### 8.1 Recommended delivery format

- [ ] `cjm-canvas`
- [x] **`hi-fi-static`** — single-page landing, 1 screen in §4, 1 primary flow (cold visitor → email capture). All 3 skip-conditions met: ≤2 screens, ≤1 flow, no anon↔authed transitions / multi-state branching.

**Reasoning:** Single-page landing, no inter-screen navigation, no auth flow. CJM canvas would be overhead — there's no flow nav to populate, and the only "tweaks" worth exploring (hero layout, pricing layout, typography) work better as a static a/b/c grid than as a sidebar toggle on a single page. Use `cjm-canvas` only if the site grows to ≥3 screens or adds a multi-step signup flow.

### 8.2 Information density type

- [x] **Restrained** — landings convert by being scannable, density kills conversion
- [ ] High-density

**Reasoning:** Dev audience is allergic to dense marketing. Lots of breathing room signals confidence.

### 8.3 Per-screen position-4 answers

| Screen | Narrative role | Audience distance | Visual temperature | Capacity check |
|--------|---------------|-------------------|---------------------|----------------|
| Hero | hero | 1m laptop | confident-warm | OK (max breathing) |
| Features | data | 1m laptop | calm | OK |
| How it works | data | 1m laptop | calm | OK |
| Social proof | quote | 1m laptop | trustworthy | OK |
| Pricing | data | 1m laptop | trustworthy | OK |
| FAQ | data | 1m laptop | calm-helpful | OK |
| Footer | end | 1m laptop | calm | OK |

### 8.4 Variation dimensions to explore

- **Dimension 1: Hero layout** — text-left + product visual on right / centered single column / fullscreen background video
- **Dimension 2: Pricing presentation** — comparison table / 3-card / single-tier-emphasized
- **Dimension 3: Typography pairing** — mono + sans (dev-coded) / serif + sans (editorial) / pure sans (modern)

**Variation count recommendation:** 3

**Reasoning:** Hero layout is the highest-impact decision for conversion. Pricing layout determines trust. Typography defines tone.

### 8.5 Tweaks worth exposing

- Primary accent color (palette of 4 to swap) [scope: global]
- Hero layout (text-left + visual / centered single-col / fullscreen video) [scope: S1] — §8.4 DIM 1
- Pricing presentation (comparison table / 3-card / single-tier-emphasized) [scope: S1] — §8.4 DIM 2
- Typography pairing (mono+sans / serif+sans / pure-sans) [scope: global] — §8.4 DIM 3
- Pricing toggle default (monthly / yearly) [scope: S1]
- Show / hide testimonials section [scope: S1]

(Even though §8.1 = `hi-fi-static`, tweaks still carry `[scope]` tags for forward-compatibility — if the site grows into a multi-screen product later, the tweak metadata is already structured for cjm-canvas conversion.)

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided (Linear / Vercel / Plain)
- [x] **Recommend huashu run §1.a Core Asset Protocol** to collect logo + UI screenshots

### 8.7 Canvas construction hint (for huashu)

`hi-fi-static`: single full-fidelity HTML page (React + Babel via CDN, or plain HTML + CSS — huashu's choice based on tweak count). All sections in one scroll: hero → features → how-it-works → social proof → pricing → FAQ → footer. Interactive states: FAQ accordion expand, pricing monthly/yearly toggle, sticky email capture appearing at 60% scroll, email submit success/error.

No CJM sidebar, no flow nav, no alternate-states block — this is a single screen. If huashu wants to expose tweaks (hero / pricing / typography), use a small floating panel (top-right or bottom-right corner), not a full sidebar. No "Copy lock-in prompt" button (round-trip not needed for a single screen — user just edits §8.4 directly).

**Scaffold note:** `skills/ux-planner/assets/canvas-scaffold.html` is for `cjm-canvas` only — do NOT start from it for `hi-fi-static`. The scaffold's sidebar / flow nav / state model is overhead for a single screen.

### 8.8 Lock-in prompt template (for the Copy button)

Not applicable — `hi-fi-static` doesn't include a Copy button. To lock §8.4 picks, the user edits the spec directly or starts a new ux-planner session with feedback. (If the site ever grows past the skip-conditions, switch §8.1 to `cjm-canvas`, regenerate the canvas from `skills/ux-planner/assets/canvas-scaffold.html`, and this section gets populated with the v1.3 multi-block template — see `references/spec-template.md` §8.8 for the canonical shape.)

## 9. Open Questions & Assumptions

### Assumptions made (verify these)

- **Assumption:** Audience is dev/technical (inferred from "AI tool for developers"). If actually broader, hero copy and tone change.
- **Assumption:** Free trial is the primary conversion path. If it's "request demo", CTA copy and email capture flow change.
- **Assumption:** Pricing has 3 tiers. If actually freemium-only or contact-sales-only, pricing section shape changes.

### Open questions (need user input later)

- **Q:** Specific product features — needs concrete list of 4 features for the feature grid. — **why it matters:** Without this, huashu fills with generic "fast / secure / open source" placeholders.
- **Q:** Customer logos — at least 6 needed. — **why it matters:** Social proof is empty without them.
- **Q:** Two testimonial quotes — full text + attribution. — **why it matters:** Same.

### Inferred from archetype defaults

- Section structure (hero → features → how it works → social proof → pricing → FAQ → footer)
- Sticky nav at top
- Mobile-first responsive breakpoints
- WCAG 2.1 AA accessibility target
- Standard cookie banner

### Product Risks

- **Email capture spam / disposable addresses:** lifts vanity numbers but kills downstream activation rate. Mitigation — server-side disposable-domain blocklist + post-submit email verification before granting trial.
- **Pricing tier confusion:** dev audience reads pricing first; ambiguous tier diff = bounce. Mitigation — feature-comparison table option (§8.4 DIM 2) tested first; FAQ link inline next to tier cards.
- **Cookie banner annoyance for dev audience:** intrusive banner contradicts "no marketing fluff" voice. Mitigation — minimal one-line bottom banner with single accept + single "essential only" toggle, no nag.
- **HN/Twitter spike LCP miss:** 3G LCP > 1.5s on launch day = bad first impression. Mitigation — critical CSS inlined, hero image as compressed AVIF + LCP-friendly format fallback, defer below-fold JS.
- **Pricing page indexability:** SEO needs `/pricing` as a separate URL, but spec says single-page. Mitigation — anchor `#pricing` + history API push when scrolled into view, plus duplicate static `/pricing.html` for crawlers.

### Considered Alternatives (§9.5)

> Empty at first generation. (For `hi-fi-static`, this section is informational only — no Copy-button round-trip exists. If §8.1 is later switched to `cjm-canvas`, this becomes the active landing zone for non-chosen variants.)
