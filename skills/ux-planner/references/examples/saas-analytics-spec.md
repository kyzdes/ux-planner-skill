# Pulse · UX Spec

> Generated: 2026-04-28 · Source language: en
> Archetype: dashboard

## 1. Product Framing

- **Type:** Web app (dashboard / analytics)
- **Audience:** SRE / DevOps engineers at mid-sized companies (50-500 engineers) responsible for keeping production services up. They check dashboards multiple times daily, especially during incidents. Tech-literate, expect Grafana-level density and shortcuts.
- **Core JTBD:** "When I'm responding to a production incident or a routine on-call shift, I want to triage what's wrong and what's recovering across all our services in 30 seconds, so I can focus my attention on the right service and avoid alert fatigue."
- **Success metric:** Time-to-triage (median seconds from dashboard open to clicking into the right service drill-down). Target: < 25 seconds.
- **Out of scope:** Log search (separate product), full-trace exploration, customer-facing status page, billing / org admin (handled in separate "Admin" web app).

## 2. Functional Scope

### Must-have features (v1)

- Service overview grid: all services with current health status, primary metrics (latency p99, error rate, throughput), trend sparklines
- Drill-down per service: full metric panels (latency p50/p95/p99, error rate, throughput, custom metrics)
- Time range picker: presets (15m, 1h, 6h, 24h, 7d) + custom range + relative-to-now
- Filtering: by environment (prod/staging/dev), region, team, custom tags
- Alerts panel: active alerts with severity, time fired, click-to-drill
- Comparison view: 2 services or 2 time periods side by side
- Saved views (bookmarkable URLs preserving filters)
- Keyboard shortcuts: g+s (services), g+a (alerts), / (search), 1-9 (recent views)
- Search (cmd+k): jump to any service, alert, saved view

### Nice-to-have (v1.5+)

- Anomaly detection highlights (auto-flag deviations)
- On-call schedule integration (show who's on for each service)
- Notification config inline (subscribe to alert from any panel)
- Export panel as image / PDF for incident postmortems

### Explicitly out of scope

- Log search and exploration — separate product
- Full distributed tracing UI — separate product
- Mobile responsive (v1 is desktop-only — engineers triage from laptops)
- Edit / configure metric collection — that's an infrastructure concern, separate admin
- White-label / theming — single product brand only

## 3. User Flows

### Flow 1: Routine check-in [primary]

1. **Entry:** Bookmark / browser tab, or notification jump
2. Land on Service Overview (default view)
3. Scan grid: all services green = move on; any red/yellow → investigate
4. Click service tile → drill-down screen
5. Review panels, scrub time range
6. **Outcome:** Confirm normal or spot anomaly to investigate

**Failure paths:**
- Stale data (last update > 1 min) → red staleness indicator banner top of grid
- Service tile data unavailable → "metric collection paused" inline state

### Flow 2: Incident response

1. **Entry:** PagerDuty alert → click link → Pulse drill-down for affected service
2. Auto-zoom to time range covering alert
3. Review which metric is anomalous
4. Click "Compare" → compare to previous week same time
5. (Optional) Add to incident timeline (export panel as image)
6. **Outcome:** Identify cause or hand off to deeper investigation

**Failure paths:**
- Service has been deprecated → "service no longer monitored" + "see related: <recent services>"
- Alert is stale → grayed out with "alert fired and recovered N min ago"

### Flow 3: Saved view / bookmark workflow

1. **Entry:** Bookmark URL with pre-saved filters (e.g., "prod-only / payments-team")
2. Land on filtered overview
3. Adjust time range or filters
4. Click "Save view" → name it → save
5. **Outcome:** Reusable URL, shareable with team

### Flow 4: Comparison investigation

1. **Entry:** Drill-down screen → click "Compare" button
2. Pick second service or second time window
3. Side-by-side panels appear
4. Scrub or filter affects both panels in sync
5. **Outcome:** Visual diff identified

## 4. Screen Inventory

| ID | Screen | Purpose | Entry points | Key actions |
|----|--------|---------|--------------|-------------|
| S1 | Service Overview (Grid) | Main triage view of all services | Default route, bookmark | Scan, filter, click tile, search |
| S2 | Service Drill-down | Single service deep-dive | Tile click, alert link, search | Review panels, scrub time, compare |
| S3 | Comparison view | Side-by-side panels | "Compare" button | Pick comparator, scrub |
| S4 | Alerts panel | All active alerts list | Sidebar nav, g+a shortcut | Filter, click to drill |
| S5 | Saved views | Manage bookmarks / shared views | Sidebar nav | Create, edit, share, delete |
| S6 | Search (cmd+k) | Global jump to anything | / or cmd+k | Type query, arrow keys, enter |
| S7 | Settings | Personal preferences (theme, density, default range) | User menu | Toggle prefs |
| S8 | Empty / first-launch | When workspace has no data sources connected | First login of new account | "Connect first data source" CTA |

## 5. Per-Screen Briefs

### S1 · Service Overview (Grid)

- **Information hierarchy:**
  - H1: Filter bar (environment, team, tags) + time range picker (top sticky)
  - H2: Aggregate health summary (X services healthy, Y degraded, Z critical)
  - H3: Service tile grid (the hero)
  - H4: Sidebar with saved views, alerts shortcut
- **Key elements:**
  - Top bar: filter pills, time range picker, search button, user menu
  - Aggregate strip: 3 numbers + sparklines (overall latency, error rate, throughput)
  - Service tiles: one per service, showing name, status indicator (color), p99 latency, error rate, throughput sparkline (last 1h)
  - Sidebar: saved views (collapsible), alerts shortcut, settings
- **States:**
  - **Empty (new workspace):** S8 layout — "Connect your first data source" with integration logos
  - **Loading:** Skeleton tiles while initial query runs (~2s)
  - **Error:** Per-tile: "Data unavailable" gray state. Aggregate level: banner.
  - **Success:** Full grid with live-updating data (every 30s)
- **Interactions:**
  - Click tile → S2 drill-down
  - Hover tile → tooltip with last update time
  - Filter pill click → opens filter selector
  - Time range click → presets + custom range picker
  - Cmd+K → S6 search overlay
  - Keyboard: 1-9 jumps to saved view 1-9

### S2 · Service Drill-down

- **Information hierarchy:**
  - H1: Service name + status pill + time range (top)
  - H2: Key metric panels (4-6 panels, customizable)
  - H3: Recent alerts for this service
  - H4: Comparison toggle, export, share buttons
- **Key elements:**
  - Header: breadcrumb (Overview > Service), service name, status pill
  - Metric panels grid: latency (p50, p95, p99), error rate, throughput, custom metrics. Each panel: title, current value, trend chart, range thresholds.
  - Alert panel: list of recent alerts (last 24h) with severity, time, link
  - Action buttons: Compare, Export, Share, Save view
- **States:**
  - **Empty (no data for this service):** "No metrics in selected range" + "expand range" CTA
  - **Loading:** Skeleton panels (~1.5s for queries)
  - **Error:** Per-panel error state with retry. Don't fail the whole screen.
  - **Success:** All panels filled, real-time updates
- **Interactions:**
  - Scrub time on any panel → all panels sync
  - Click panel → expand to fullscreen
  - Drag panel border → resize
  - Right-click panel → context menu (export, copy URL, hide)
  - Compare button → S3

### S3 · Comparison view

- **Information hierarchy:**
  - H1: Two columns side by side (Service A | Service B, or Time A | Time B)
  - H2: Synced controls at top
- **Key elements:**
  - Column headers: name + selector (change comparator)
  - Synced time range and filter controls (top center)
  - Mirrored metric panels in each column
  - Diff overlay (optional): "Δ" badges showing percentage difference
- **States:**
  - **Empty:** "Pick something to compare" with selector
  - **Loading:** Skeleton both columns
  - **Error:** Per-column error
  - **Success:** Both columns rendered
- **Interactions:**
  - Time scrub affects both
  - Filter affects both
  - Swap columns button

### S4 · Alerts panel

- **Information hierarchy:**
  - H1: Filter (severity, service, time)
  - H2: Alert list (sorted by recency)
- **Key elements:**
  - Filter bar
  - Alert rows: severity icon, alert name, service, fired time, status (active / recovered)
  - Click row → S2 with auto-zoom to alert window
- **States:**
  - **Empty:** "No active alerts — quiet so far" with positive emoji-free messaging
  - **Loading:** Skeleton rows
  - **Error:** Top banner
  - **Success:** Full list
- **Interactions:**
  - Click row → S2 drill-down
  - Filter pill click
  - Keyboard arrow nav

### S5 · Saved views

- **Information hierarchy:**
  - H1: List of saved views with metadata
- **Key elements:**
  - Row: name, owner (you / team), last updated, share status
  - Edit / Delete / Duplicate / Share inline buttons
- **States:**
  - **Empty:** "No saved views yet — bookmark a filtered overview to save"
  - **Loading:** Skeleton
  - **Error:** Inline retry
  - **Success:** Standard list
- **Interactions:**
  - Click row → opens that view (with filters applied)
  - Inline action buttons

### S6 · Search (cmd+k)

- **Information hierarchy:**
  - H1: Modal overlay, search input at top
  - H2: Results categorized (Services / Alerts / Saved views / Recent)
- **Key elements:**
  - Search input (focused on open)
  - Results list with category headers
  - Keyboard navigation (arrow keys + enter)
- **States:**
  - **Empty (no query):** Recent searches + popular services
  - **Loading:** Brief inline loader
  - **Error:** "Search unavailable, try again"
  - **Success:** Results shown
- **Interactions:**
  - Type → live results
  - Arrow keys to nav
  - Enter to select
  - Esc to close
  - Tab to switch category

### S7 · Settings

- **Information hierarchy:**
  - Grouped: Profile / Preferences / Notifications / API tokens
- **Key elements:**
  - Profile: name, email, avatar
  - Preferences: theme (light/dark/auto), density (compact/comfortable), default time range
  - Notifications: alert subscriptions
  - API tokens: list + create/revoke
- **States:**
  - **Empty:** N/A (always populated)
  - **Loading:** N/A
  - **Error:** Per-row inline
  - **Success:** Standard settings
- **Interactions:**
  - Inline toggles
  - Save buttons (auto-save where possible)

### S8 · Empty / first-launch

- **Information hierarchy:**
  - H1: Welcome + "Connect your first data source" big CTA
  - H2: Integration logos (Datadog, Prometheus, OpenTelemetry, custom)
  - H3: Docs link
- **Key elements:**
  - Welcome text
  - Integration cards (clickable, route to setup wizard)
  - Skip option (lands on empty grid)
- **States:**
  - **Empty:** This is the empty state itself
  - **Loading:** N/A (fast)
  - **Error:** N/A
  - **Success:** N/A (always shown to new accounts)
- **Interactions:**
  - Click integration → setup wizard (out of scope for this spec)
  - Skip → S1 with empty state

## 6. Constraints & Context

- **Platform:** Web (desktop only v1). Chromium / Firefox / Safari latest.
- **Devices:** Desktop minimum 1280x800. No mobile support v1 (engineers triage from laptops).
- **Accessibility:** WCAG 2.1 AA. Color blind safe (don't rely on color alone for status). All interactions keyboard-accessible.
- **Localization:** English v1 only. SRE / DevOps community is English-dominant.
- **Performance budget:** Initial overview render < 2s with 50 services. Time scrub feedback < 100ms. WebSocket updates every 30s without UI jank.
- **Auth model:** SSO (SAML, Google Workspace, Okta) for enterprise. Email + password fallback for trials.
- **Data sources:** Pull from Datadog API, Prometheus, OpenTelemetry, custom adapters.
- **Offline behavior:** Online-required (real-time data is the value prop). Show cached last-known state when offline with banner.

## 7. Design Context (for huashu)

- **Existing design system:** No (new product)
- **Brand assets available:**
  - Logo: needs collection (recommend wordmark with subtle pulse motif)
  - Colors: needs collection. Suggest dark theme primary (engineers prefer dark for monitoring).
  - Fonts: needs collection. Suggest mono + sans pair (mono for numbers, sans for text).
  - Product images / UI screenshots: N/A (designing from scratch)
- **References / inspiration:** Datadog (dense), Grafana (open source baseline), Honeycomb (modern observability), Linear (clean dev UX)
- **Design direction known:** No — recommend huashu fallback advisor mode (suggest 3 directions: dense Datadog-like / clean Honeycomb-modern / minimalist Linear-inspired)
- **Brand voice / tone:** Calm-authoritative. Numbers-first. No marketing emoji. Inspires confidence during incidents.

## 8. Hand-off to huashu-design

### 8.1 Recommended delivery format

- [x] **`cjm-canvas`** — 8 screens, 4 flows (routine check-in / incident response / saved-view / comparison), with empty-state branching (S8) and search-overlay state (S6). Fails all 3 skip-conditions decisively.
- [ ] `hi-fi-static`

**Reasoning:** 8 screens with shared visual language (filter bars, panels, sparklines, sidebar) and 4 distinct flows. CJM canvas lets the user click through Overview → Drill-down → Comparison in one HTML, toggle tile-density / sidebar / color-system variants per screen, and copy lock-in choices back to Claude.

### 8.2 Information density type

- [ ] Restrained
- [x] **High-density** — this is the canonical high-density use case. Engineers reward dense info; sparse dashboards waste their screen.

**Reasoning:** Triage in 30s requires everything visible at once. Restrained = wasted screen real estate.

### 8.3 Per-screen position-4 answers

| Screen | Narrative role | Audience distance | Visual temperature | Capacity check |
|--------|---------------|-------------------|---------------------|----------------|
| S1 Overview | hero | 1m laptop | analytical-cold | high-density risk (most data on one screen) |
| S2 Drill-down | data | 1m laptop | analytical | high-density risk |
| S3 Comparison | data | 1m laptop | analytical | risk-tight (two columns) |
| S4 Alerts | data | 1m laptop | urgent | OK |
| S5 Saved views | data | 1m laptop | calm | OK |
| S6 Search | transition | 1m laptop | quick-focused | OK |
| S7 Settings | end | 1m laptop | calm | OK |
| S8 Empty / first-launch | hero | 1m laptop | inviting | OK (max breathing) |

### 8.4 Variation dimensions to explore

- **Dimension 1: Tile density** — compact (16 services per row, sparkline-only) / comfortable (8 per row, sparkline + 1 number) / spacious (4 per row, full chart)
- **Dimension 2: Color system** — dark mode primary with status accent / light mode professional with restrained accent / mixed (dark for charts, light for chrome)
- **Dimension 3: Sidebar treatment** — persistent sidebar with full nav / collapsible to icon rail / hidden behind hamburger (for max screen real estate)

**Variation count recommendation:** 3

**Reasoning:** Tile density is the highest-impact decision (engineers' core metric: "how many services do I see at once?"). Color system signals brand. Sidebar treatment is the screen-real-estate tradeoff.

### 8.5 Tweaks worth exposing

- Theme (dark / light / auto) [scope: global]
- Tile density (compact / comfortable / spacious) [scope: S1] — §8.4 DIM 1
- Sidebar behavior (persistent / collapsible / hidden) [scope: global] — §8.4 DIM 3
- Color system (dark + status accent / light professional / mixed) [scope: global] — §8.4 DIM 2
- Default time range preset (15m / 1h / 6h / 24h) [scope: S1, S2]
- Show alerts in sidebar (yes / no) [scope: global]
- Drill-down panel grid (2-col / 3-col / freeform) [scope: S2]
- Comparison split direction (vertical / horizontal) [scope: S3]
- Cmd+K result categories layout (grouped / flat / tabs) [scope: S6]

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided (N/A — designing from scratch)
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided (Datadog, Grafana, Honeycomb, Linear)
- [x] **Recommend huashu run §1.a Core Asset Protocol** for logo + competitor UI reference screenshots

### 8.7 Canvas construction hint (for huashu)

cjm-canvas single HTML (React + Babel via CDN). Stage area: iframe wrapping desktop-sized viewport (≥1280px) with browser chrome (3 traffic dots, URL bar showing the product domain `pulse.example.com`, optional bookmarks bar) for the active screen. Above the chrome: pill `S<id> · <NAME>`.

Right sidebar (~360–400px sticky, 4 blocks):
1. **Tweaks** — filter §8.5 by active screen `[scope]`. Tweaks tagged with `§8.4 DIM <n>` headings render as primary toggle groups; non-DIM tweaks below in a "Polish" section.
2. **Flow steps** — 4 flows from §3 (Routine check-in / Incident response / Saved-view / Comparison). Default flow on canvas open = Routine check-in starting at S1.
3. **Alternate states** — sourced from §5 states + §9 risks. Examples:
   - `S1 Overview · Stale data banner` — VARIANT 1
   - `S1 Overview · Per-tile data unavailable` — VARIANT 2
   - `S2 Drill-down · No metrics in range` — VARIANT 3
   - `S2 Drill-down · Per-panel error` — VARIANT 4
   - `S4 Alerts · No active alerts` — VARIANT 5
   - `S6 Cmd+K · Search unavailable` — SWAP (overlays any screen)
   - `S8 First-launch · No data sources connected` — VARIANT 6
4. **Meta footer** — `SOURCE · ux-spec-2026-04-28-server-metrics-dashboard.md` / `SYSTEM · — (no design system)` / `DENSITY · HIGH-DENSITY`.

Sticky bottom button: "Copy lock-in prompt" — generates §8.8 prompt with current selections, writes to clipboard.

### 8.8 Lock-in prompt template (for the Copy button)

```
Lock these design choices into the UX spec at /Users/me/Desktop/projects/metrics-dashboard/ux-spec-2026-04-28-server-metrics-dashboard.md:

Screen <ACTIVE-S-id> · <ACTIVE-SCREEN-NAME>:
- §8.4 DIM <n> <NAME>: <SELECTED-VARIANT>
(repeat per active tweak that maps to a §8.4 dimension)

Action: update §8.4 — mark these variants as "locked" for this screen and move non-chosen variants to §9.5 Considered Alternatives. Re-run §6 self-review and regenerate the §8 hand-off phrase.
```

## 9. Open Questions & Assumptions

### Assumptions made (verify these)

- **Assumption:** SSO is the primary auth path for v1. If trials are common, email/password gets equal weight.
- **Assumption:** Real-time updates every 30 seconds. Could be 10s or 60s depending on backend cost.
- **Assumption:** Mobile is fully out of scope v1. If on-call requires phone access, this changes everything.
- **Assumption:** "Pulse" is the product name. If it's something else, replace throughout.

### Open questions (need user input later)

- **Q:** What's the maximum number of services in a typical customer workspace? — **why it matters:** Drives tile density choice and grid pagination.
- **Q:** Are there role-based views (e.g., team leads see all, engineers see only their team)? — **why it matters:** Adds permission-aware filtering, changes auth model.
- **Q:** Notification channels (Slack / PagerDuty / email)? — **why it matters:** Determines integration screens scope.
- **Q:** Custom metric definitions in v1 or v2? — **why it matters:** Big scope difference.

### Inferred from archetype defaults

- Sidebar nav with persistent saved views
- Top filter bar with time range picker
- Skeleton loading patterns
- Real-time WebSocket updates
- Cmd+K global search
- Standard alerts panel layout
- Settings split into Profile / Preferences / Notifications / API tokens

### Product Risks

- **Datadog/Prometheus API rate limits:** scaling beyond ~500 services hits upstream rate limits and partial-data UI states. Mitigation — batch + cache layer + per-tile staleness indicator instead of failing the whole grid.
- **Real-time update jank:** WebSocket pushes every 30s across 50+ panels can drop frames during scrub. Mitigation — debounce render to RAF + only update changed panels.
- **Alert fatigue mis-triage:** all alerts shown equally → noisy services drown out incidents. Mitigation — severity-weighted sort + collapsed-by-default for "info" severity.
- **Saved-view drift:** team renames a service → saved views break silently. Mitigation — soft-resolve by service ID + warn-on-load if any filter resolved 0 services.
- **Cmd+K result staleness:** search index updates lag behind service registry. Mitigation — display "Last indexed Xm ago" in cmd+k empty state + manual refresh.
- **Compare view perf:** 2× panels on S3 doubles query cost. Mitigation — share queries between columns when filters match + skeleton-stream individual panels.

### Considered Alternatives (§9.5)

> Empty at first generation. Will populate when the user runs the cjm-canvas Copy button and pastes the lock-in prompt back.
