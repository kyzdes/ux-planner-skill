# UX Patterns Library

Compact reference of common UX patterns. Used in Phase 3 (targeted follow-ups) and advisor mode (when user is stuck on a specific decision). Not exhaustive — focuses on the 15-20 most common decisions every product faces.

For each pattern: what it is, when to pick which option, and tradeoffs.

---

## Navigation models

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Bottom tab bar** | Mobile apps with 3-5 main areas | Fixed, can't easily add 6th tab |
| **Hamburger menu** | Mobile apps with many features (avoid for primary nav) | Hides nav, lower discoverability |
| **Sidebar (persistent)** | Desktop / web productivity | Eats horizontal space |
| **Sidebar (collapsible)** | Desktop with content needing focus | Extra click for nav |
| **Top tabs** | Sub-navigation within a section | Limited width on mobile |
| **Contextual nav (no global)** | Single-task tools, focused flows | No persistent way "home" |
| **Command palette (cmd-k)** | Power users, productivity tools | Discoverability for new users |

**Default pick:** mobile utility → bottom tab; web productivity → sidebar; dashboard → sidebar + top filter; landing → top header.

---

## Onboarding strategies

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Skippable tour** | Apps with non-obvious value props | 30%+ skip, may miss key features |
| **Forced tour** | Apps requiring config (sync, permissions) | Users feel locked in |
| **Sample data** | Empty UI that intimidates | Risk: users mistake samples for real data |
| **Empty UI + tooltips on first action** | Simple apps with intuitive first action | Confusion if first action isn't obvious |
| **Progressive (just-in-time)** | Complex apps revealing depth gradually | Slower TTV for power users |
| **Mandatory signup first** | Apps with persistent state | Higher drop-off before TTV |
| **Guest mode → upgrade later** | Apps where value is immediate | Re-onboarding when they sign up |

**Default pick:** mobile utility → empty + tooltips OR sample data; landing → no onboarding, signup is the gate; productivity → progressive + template gallery.

---

## Auth strategies

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Email + password** | Default, expected | Password fatigue, leaks |
| **Magic link** | Low-friction, no passwords | Email delivery hiccups, slower login |
| **Social (Google / Apple)** | Mobile, consumer | Privacy concerns, lock-in to provider |
| **Passkey (WebAuthn)** | Modern, secure, no passwords | Browser/device support gaps still |
| **Guest mode (no auth v1)** | Single-device, local-only apps | No sync, lost on uninstall |
| **SSO (org tools)** | Enterprise, B2B | Complex setup, IT involvement |

**Default pick:** mobile utility → social + email; web productivity → magic link or social; B2B / internal → SSO; landing → no auth (CTA goes external); extension → optional sync auth.

---

## Empty states

| Pattern | Best for | Notes |
|---|---|---|
| **CTA + illustration** | First-time users with no data | Illustration ≠ AI slop — be specific |
| **Sample data (pre-populated)** | Apps where empty UI is intimidating | Mark clearly as samples |
| **Educational (this is what X is)** | Concept-heavy apps | Risks reading like marketing |
| **Single CTA, nothing else** | Highly opinionated UX | Feels minimal and confident |
| **Skeleton (faux data)** | Apps loading content | Don't fake permanent data |

**Default pick:** mobile utility → CTA + illustration; productivity → sample data + template gallery; dashboard → "connect a data source" CTA.

---

## Error states

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Inline (next to field)** | Form validation | Doesn't apply to global errors |
| **Toast (auto-dismiss)** | Non-critical confirmations / errors | Easy to miss |
| **Modal / dialog** | Errors requiring acknowledgment | Interrupts flow |
| **Page-level (full screen)** | Catastrophic errors (404, server down) | Hard to recover from |
| **Banner (top of screen)** | Persistent warnings | Eats space |
| **Empty-state-with-error** | When data fetch failed | Combines empty + error gracefully |

**Default pick:** form validation → inline; network errors → banner or inline-with-retry; permissions → page-level explanation.

---

## Search patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Instant (live as you type)** | Local / fast indexes | Backend cost if remote |
| **Submit-driven** | Slow / expensive search | Slower, "did you submit?" confusion |
| **Scoped (within current section)** | Productivity tools | Doesn't find global results |
| **Global (everywhere)** | Apps with cross-area content | Results need clear categorization |
| **Filters + search combined** | Lists / catalogs | More UI surface |
| **Command palette (cmd-k)** | Power users | Hidden until learned |

**Default pick:** mobile utility → simple instant within list; productivity → cmd-k global; dashboard → scoped search per panel.

---

## List / collection patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Infinite scroll** | Discovery / browsing feeds | Can't reach footer, performance with 1k+ |
| **Pagination (numbered)** | Reference / search results | Choice paralysis if many pages |
| **Load more (button)** | Curated lists | User must take action |
| **Virtualized list** | 100+ items, performance-critical | Engineering complexity |
| **Group by category** | Heterogeneous lists | Header noise if few items per group |
| **Sectioned (alphabetical / date)** | Phone book / chronological | Empty section headers |

**Default pick:** mobile utility → simple list (likely <100 items); content app → infinite or load-more; productivity → virtualized + filter.

---

## Form patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Single-screen form** | Short forms (3-7 fields) | Can feel busy if too many fields |
| **Wizard (multi-step)** | Long forms or branching logic | Can't see end state, more abandons |
| **Inline edit (click-to-edit)** | Productivity, settings | Discoverability — what's editable? |
| **Auto-save** | Long-form content (docs, drafts) | "Did it save?" anxiety without indicator |
| **Side-panel form** | Adding to an existing list | Doesn't block the list |
| **Modal / sheet form** | Quick inputs, mobile | Can't keep list visible |

**Default pick:** add habit / note → modal sheet on mobile; long onboarding → wizard; doc edit → inline + auto-save; settings → inline.

---

## Settings IA

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Flat list** | Few settings (5-10) | Doesn't scale |
| **Grouped by category** | Medium (10-30) | Discovery — which group? |
| **Nested (sub-pages)** | Many (30+) | Many taps to reach a setting |
| **Search-first** | Complex apps with many settings | Required if 50+ settings |
| **Tabbed (across top)** | Web settings, 3-5 categories | Doesn't fit on mobile |
| **Sidebar settings (web)** | Productivity tools | Eats horizontal space |

**Default pick:** mobile utility → grouped + flat with sections; web productivity → sidebar settings; extension → simple tabbed.

---

## Notification patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Push (system)** | Time-sensitive, re-engagement | Permission ask, unsubscribe risk |
| **Email** | Async, summary, transactional | Can land in spam |
| **In-app only** | Activity feeds, mentions | Only seen when app is open |
| **SMS** | Critical alerts (2FA, transactions) | Cost, intrusiveness |
| **Webhook (developer)** | Integration scenarios | Out of scope for end-user UX |
| **No notifications v1** | MVP, focus elsewhere | Loses re-engagement vector |

**Default pick:** mobile utility → push + in-app; productivity → in-app + email digest; dashboard → email alerts + in-app.

---

## Monetization patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Free** | Marketing tool, future monetization | No revenue |
| **One-time purchase** | Tools with defined value | Hard to grow ARPU |
| **Subscription (monthly/yearly)** | SaaS, ongoing value | Subscription fatigue |
| **Freemium with paywall** | Mass-market apps | Where to draw the line is hard |
| **Usage-based** | API, infra | Unpredictable cost for users |
| **Ad-supported** | Free consumer | Ad UI degrades quality perception |

**Default pick:** mobile utility consumer → freemium with paywall on stats / advanced features; productivity → subscription per-user; landing → no monetization (it's a marketing piece); B2B → contact sales / annual.

---

## Detail / drilldown patterns

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Push to new screen** | Mobile, deep info | Loses list context |
| **Modal / sheet** | Quick peek, return easily | Can't reach list while open |
| **Side panel** | Desktop / wide screens | Eats horizontal space |
| **Inline expand** | Lists with brief details | Layout shift, jarring |
| **Master-detail (split view)** | Tablet / desktop | Doesn't work on phone |

**Default pick:** mobile utility list → push; mobile sheet → modal; web productivity → side panel; tablet → master-detail.

---

## Multi-select / bulk action

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Long-press to select (mobile)** | iOS / Android natives | Hidden until learned |
| **Checkbox always visible** | Web, frequent bulk actions | UI noise |
| **Edit mode toggle** | When bulk is rare | Modal-feel, can be confusing |
| **Drag-select (desktop)** | Power users | Doesn't work on mobile |

**Default pick:** mobile utility → long-press; web productivity → checkbox always; admin → checkbox always with bulk action bar.

---

## Sync / multi-device

| Pattern | Best for | Tradeoff |
|---|---|---|
| **Account-based cloud sync** | Cross-device users | Requires auth |
| **iCloud / Google Drive** | Single-platform users | Vendor lock-in |
| **Local-only** | Privacy-first, single device | No backup, no sync |
| **CRDT-based real-time** | Collaboration tools | Engineering complexity |
| **Optional sync (opt-in)** | Privacy-conscious | Two code paths |

**Default pick:** consumer mobile utility → optional sync, iCloud-friendly; productivity → required cloud sync; extension → optional sync via account.

---

## How to use this file

In **Phase 3** (targeted follow-ups): when you ask about a pattern decision, mention 2-3 options inline. E.g., "Auth: email/password (default), magic link (lower-friction), or social (Google/Apple)?"

In **advisor mode**: pull from the relevant section to construct 2-3 differentiated options with tradeoffs.

In **Phase 4** (per-screen briefs): use these patterns to populate "Interactions" and "Key elements" fields. E.g., for a productivity workspace, "command palette (cmd-k)" is a default key element.

In **§5 of the spec** (per-screen briefs) and **§8.5 (Tweaks)**: explicitly call out which pattern was chosen so huashu doesn't re-decide.

---

## What this file is NOT

- Not a visual design guide — that's huashu's territory
- Not exhaustive — covers ~20 most common decisions, not 200 edge cases
- Not opinionated about specific copy / interactions — that's per-spec

If a pattern decision comes up that isn't here, infer from first principles or ask the user, but don't paste a meta-discussion of patterns into the spec.
