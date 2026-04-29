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

- [ ] Overview tile
- [ ] Flow demo
- [x] **Hi-fi prototype** (full hi-fi with real data)
- [ ] Multi-format

**Reasoning:** Landing pages live or die on first impression. Need full visual fidelity to evaluate. Single page so no flow demo needed.

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

- Primary accent color (palette of 4 to swap)
- Hero layout switcher (the 3 options above)
- Pricing toggle default (monthly / yearly)
- Show/hide testimonials section

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided (Linear / Vercel / Plain)
- [x] **Recommend huashu run §1.a Core Asset Protocol** to collect logo + UI screenshots

### 8.7 Flow vs. overview routing hint

Hi-fi prototype: huashu generates a single full-fidelity HTML page with all sections, scroll behavior, and interactive states (FAQ expand, pricing toggle, email capture form).

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
