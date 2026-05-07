# Product Archetypes

Seven archetypes covering the product/UI scope this skill handles. For each: defaults for screens, flows, edge states, density type, and variation axes worth exploring.

These defaults feed Phase 2 (inferred draft). When the user says "habit tracker", the skill can infer 5 must-have features and 5 screens without asking.

## Redesigning an existing product

If the project root contains any of `context-map.md`, `CLAUDE.md`, `AGENTS.md`, or a populated project memory (`MEMORY.md` with linked product files), **read those files before proposing an archetype**. Existing screens — Jinja templates, React pages, route handlers — are the source of truth. Phase 1 framing questions (audience, JTBD, platform, design context) are usually already answered there; skip Phase 1 in that case.

Still pin down the archetype explicitly in §1 of the spec, even when redesigning — huashu uses the archetype label to choose density and patterns. Note the source: `Inferred from context-map.md` or similar.

When redesigning, the screen inventory is partly a re-listing of what exists plus what's new. Mark new screens explicitly so the user can spot scope creep.

---

## 1. Mobile app — utility

**Examples:** habit trackers, note apps, todo lists, journals, pomodoro timers, expense trackers, water reminders, period trackers.

**Default screens (5-7):**
- S1 Home — primary list / today view (the hero screen)
- S2 Detail — single entity view (e.g., one habit, one note)
- S3 Add / create — form for new entry (often modal sheet)
- S4 Stats / history — trends, calendars, streaks
- S5 Settings — account, notifications, appearance
- S6 (optional) Onboarding — 2-3 step intro
- S7 (optional) Paywall — if monetized

**Default flows:**
1. **First launch onboarding** → home (with sample data or empty state)
2. **Add new entry** → home updated
3. **Open detail** → edit / delete → back to list
4. **View stats** → drill into specific metric

**Default density type:** **High-density** (utility apps live or die on showing the smart, contextual data — streaks, predictions, trends. Plain "list of items" feels generic).

**Common edge states:**
- Empty habit list on first launch — needs strong CTA
- Notification permission denial — degraded state
- Long history (1+ year of data) — performance / scroll
- Streak break — emotional design moment

**Variation axes worth exploring:**
- Layout: list vs. grid vs. calendar-first
- Density: minimalist (Done-style) vs. data-rich (Streaks-style)
- Hero metric: streak vs. completion % vs. today's tasks

**Position-4 default per screen:**
| Screen | Role | Distance | Temperature | Capacity |
| Home | hero | 10cm phone | warm-focused | high-density risk |
| Detail | data | 10cm phone | calm | OK |
| Add | transition | 10cm phone | neutral | OK |
| Stats | data | 10cm phone | analytical | OK |
| Settings | end | 10cm phone | calm | OK |

---

## 2. Mobile app — content

**Examples:** RSS readers, podcast apps, ebook readers, meditation apps, video players, news aggregators, museum guides.

**Default screens (5-8):**
- S1 Library / feed — list of available content
- S2 Reader / player — the content consumption screen
- S3 Search / discover — find new content
- S4 Saved / favorites
- S5 Settings — playback / reading prefs, sync, account
- S6 (optional) Onboarding with content recommendations

**Default flows:**
1. **Discover content** → preview → consume
2. **Save for later** → access from saved
3. **Continue where you left off** → reader resumes
4. **Adjust playback / reading settings** in-context

**Default density type:** **Restrained** (content apps need the content to breathe — chrome should disappear when reading/listening).

**Common edge states:**
- Empty library on first launch — needs onboarding suggestions
- Offline content access — visual indicator
- Long-form content rendering — typography, line length
- Audio interruption (calls, headphones) — playback behavior

**Variation axes:**
- Reader layout: classic / paged / scroll
- Discovery: editorial / algorithmic / search-first
- Theme: light / dark / sepia / system

**Position-4 default:**
| Screen | Role | Distance | Temperature | Capacity |
| Library | hero | 10cm phone | inviting | OK |
| Reader | hero | 10cm phone | calm-immersive | OK (max breathing) |
| Search | transition | 10cm phone | neutral | OK |
| Settings | end | 10cm phone | calm | OK |

---

## 3. Web app — productivity

**Examples:** Notion-style note apps, project mgmt tools, kanban boards, document editors, CRMs, calendar apps, design tools.

**Default screens (6-10):**
- S1 Workspace / home — sidebar nav + main content
- S2 Document / item view — primary editing surface
- S3 List / board view — overview of all items
- S4 Search / command palette — global navigation
- S5 Member / collaboration view — who's doing what
- S6 Settings — account, workspace, integrations
- S7 (often) Onboarding / template gallery
- S8 (optional) Empty workspace state

**Default flows:**
1. **Create / open document** → edit → autosave → close
2. **Find item** via search / cmd-k → jump
3. **Invite collaborator** → role assignment → notification
4. **Switch workspace / context** via sidebar

**Default density type:** **High-density** (productivity tools earn their keep by surfacing contextual data — recently edited, mentions, activity).

**Common edge states:**
- Empty workspace (just signed up)
- Conflicting edits in real-time collaboration
- Slow / large documents
- Permission errors (can view, can't edit)

**Variation axes:**
- Sidebar: persistent / collapsible / hidden
- Density: compact (Linear) / comfortable (Notion) / spacious
- Command palette: cmd-k everywhere / dedicated search bar

**Position-4 default:**
| Screen | Role | Distance | Temperature | Capacity |
| Workspace | hero | 1m laptop | calm-focused | OK |
| Document | hero | 1m laptop | calm | depends on doc length |
| Board | data | 1m laptop | analytical | high-density risk |
| Settings | end | 1m laptop | neutral | OK |

---

## 4. Dashboard / analytics

**Examples:** server monitoring, sales analytics, web traffic, financial dashboards, IoT control, fleet management.

**Default screens (6-10):**
- S1 Overview / KPIs — top-level numbers + trend charts (the hero)
- S2 Drill-down / detail — single metric expanded
- S3 Comparison — side-by-side periods, segments, cohorts
- S4 Reports / exports
- S5 Alerts / anomalies
- S6 Settings — data sources, integrations, alert config
- S7 (optional) Empty state — connect first data source

**Default flows:**
1. **Open dashboard** → scan KPIs → drill into anomaly
2. **Filter time range / segment** → KPIs update → share view
3. **Configure alert** → trigger condition → notification channel
4. **Export report** → format selection → download

**Default density type:** **High-density** (this is the canonical high-density use case — dashboards exist to compress data into glanceable form).

**Common edge states:**
- No data connected yet (empty state)
- Stale data / connection lost
- Anomaly detected (alert state)
- Comparison with no overlap period
- Permission-restricted views

**Variation axes:**
- Layout: grid / freeform / single-column
- Chart density: full / sparkline / mixed
- Time range UX: presets / picker / scrubber

**Position-4 default:**
| Screen | Role | Distance | Temperature | Capacity |
| Overview | hero | 1m laptop | analytical-cold | high-density risk |
| Drill-down | data | 1m laptop | analytical | OK |
| Alerts | data | 1m laptop | urgent | OK |
| Settings | end | 1m laptop | neutral | OK |

---

## 5. Landing / marketing site

**Examples:** product launches, SaaS marketing, app pre-launch pages, agency portfolios, conference sites.

**Default screens (1, scrolling):**
- Single long-scroll page with sections:
  - Hero (above fold) — primary headline + CTA
  - Social proof (logos, testimonials)
  - Features (3-6 highlighted)
  - How it works (3-step)
  - Pricing (if applicable)
  - FAQ
  - Footer (about, contact, legal)

**Default flows:**
1. **Land → read hero → scroll** → CTA click → signup / external
2. **Direct to pricing** → compare plans → sign up
3. **Scroll-driven engagement** → keep reading → CTA at bottom

**Default density type:** **Restrained** (landings convert by being scannable — density kills conversion).

**Common edge states:**
- Mobile vs desktop scroll behavior
- Slow connection (image weight)
- A/B variant routing
- Cookie / GDPR banner overlay

**Variation axes:**
- Hero layout: text-left + image-right / centered / fullscreen image / video
- Pricing: cards / table / toggle (monthly/yearly)
- CTA placement: hero only / sticky / repeated per section

**Position-4 default:**
| Section | Role | Distance | Temperature | Capacity |
| Hero | hero | 1m laptop | inviting-warm | OK (max breathing) |
| Features | data | 1m laptop | calm | OK |
| Pricing | data | 1m laptop | trustworthy | OK |
| FAQ | end | 1m laptop | calm | OK |

---

## 6. Internal tool / admin panel

**Examples:** customer support tools, ops dashboards, CMS admin, refund tools, bulk action interfaces, role management.

**Default screens (6-10):**
- S1 Main table — searchable / filterable list of records (the hero)
- S2 Record detail — single record with actions
- S3 Bulk action confirmation
- S4 Search results
- S5 User / role management
- S6 Audit log
- S7 Settings — integrations, permissions

**Default flows:**
1. **Find record** via search → open detail → take action
2. **Bulk action** → select rows → confirm → execute → audit
3. **Investigate issue** → record → linked records → fix

**Default density type:** **High-density** (internal tools reward dense info — power users learn the layout).

**Common edge states:**
- No matching records (empty search)
- Permission-restricted action (button disabled)
- Long audit log
- Bulk action partial failure

**Variation axes:**
- Table: dense rows / comfortable / cards
- Action placement: inline / per-row menu / bulk bar
- Search: global vs scoped per page

**Position-4 default:**
| Screen | Role | Distance | Temperature | Capacity |
| Main table | hero | 1m laptop | analytical | high-density risk |
| Detail | data | 1m laptop | neutral | OK |
| Bulk confirm | transition | 1m laptop | cautious | OK |
| Audit | end | 1m laptop | calm | OK |

---

## 7. Browser extension

**Examples:** ad blockers, password managers, productivity overlays, scraper tools, page action helpers.

**Default screens (3-5):**
- S1 Popup — main interaction surface (clicked from toolbar)
- S2 Settings page — full options (chrome://extensions style)
- S3 In-page injection — overlay / sidebar / inline button
- S4 (optional) Onboarding tab on install
- S5 (optional) Account / sync screen

**Default flows:**
1. **Install → onboarding tab opens** → permission grants → first action
2. **Click toolbar icon → popup opens** → take action → popup closes
3. **In-page interaction** → injected UI → action

**Default density type:** **Restrained for popup** (small surface, can't cram), **moderate for settings page**.

**Common edge states:**
- Permission denied (e.g., page access)
- Sync conflicts (across devices)
- Page-specific behavior (does extension work here?)
- First-launch popup vs. logged-in popup

**Variation axes:**
- Popup size: 320px / 380px / 480px wide
- Action style: button / form / dashboard-mini
- Settings: tabbed / single scroll / search

**Position-4 default:**
| Screen | Role | Distance | Temperature | Capacity |
| Popup | hero | 10cm screen-corner | quick-focused | tight (small surface) |
| Settings | data | 1m laptop | calm | OK |
| In-page UI | transition | 1m laptop (in-context) | unobtrusive | tight |

---

## How to use this file

When in **Phase 0** (archetype detection): read this file end-to-end, find best match, lock it in.

When in **Phase 2** (inferred draft): pull defaults verbatim — features, screens, flows. The user will edit; you don't have to ask "what screens do you need?" — propose the archetype defaults and let them subtract / add.

When in **Phase 4** (per-screen briefs): use position-4 defaults as starting point. Override for specific user input.

When in **Phase 5** (design context advisor): if user has no design direction, the archetype + audience implies a default tone (productivity = neutral / utility = warm / dashboard = analytical / landing = inviting). Mention this in §7 of the spec.

---

## Borderline / hybrid cases

Some products straddle archetypes:

- **Doctor booking app** — mobile-utility (schedule + record) + content (find doctor info). Treat as mobile-utility primarily.
- **Notion** — web-productivity + content (rich docs). Treat as web-productivity.
- **Calendar app** — mobile-utility on mobile, web-productivity on desktop. Pick by primary platform.
- **AI chatbot** — mobile-utility OR web-productivity depending on whether it has multiple conversations / threads. Default to mobile-utility for simple chat, web-productivity for thread mgmt.
- **Investment app** — could be mobile-utility (Robinhood) or dashboard (broker desktop). Pick by audience: retail = mobile-utility, pro = dashboard.

When borderline, mention both options in Phase 1 question 1 instead of picking blind.
