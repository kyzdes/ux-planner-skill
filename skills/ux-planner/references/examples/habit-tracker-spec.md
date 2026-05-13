# Streak · UX Spec

> Generated: 2026-04-28 · Source language: ru
> Archetype: mobile-utility

## 1. Product Framing

- **Type:** Mobile app
- **Audience:** Self-improvement-curious adults (25-40) who've tried generic todo apps and bounced. They want to build 2-5 habits (workout, meditate, read) but not 20. Slightly tech-literate, willing to pay for design quality and frictionless UX.
- **Core JTBD:** "When I want to build a new habit and not lose motivation by week 2, I want a friction-free way to log it daily plus visual feedback that I'm making progress, so I can stay consistent past the 21-day mark."
- **Success metric:** D30 retention (% of users still logging habits 30 days post-install). Target: 40%.
- **Out of scope:** Social/sharing features, gamification with leaderboards, integration with Apple Health / fitness data, AI coaching, in-app community.

## 2. Functional Scope

### Must-have features (v1)

- Create up to 5 habits with name, color, icon, frequency (daily / weekdays / N times per week)
- Log habit completion with single tap from home screen
- View today's habit list with progress (today / streak / weekly completion %)
- Detail view per habit: full history grid (calendar heatmap), streak count, completion rate
- Daily push notification reminder (configurable time per habit)
- Settings: notification times, theme (light/dark/system), iCloud sync toggle
- Onboarding: 2-step (name first habit + set time)
- Empty state: "Tap + to add your first habit" with example illustrations

### Nice-to-have (v1.5+)

- Weekly summary push notification ("you completed 4/5 habits this week")
- Widget on iOS home screen showing today's habits
- Apple Watch complication
- Streak recovery (1 free skip per week without breaking streak)

### Explicitly out of scope

- Multi-user / shared habits — adds invite/permission complexity, not in core JTBD
- Habit templates marketplace — premature feature, dilutes brand
- Detailed analytics (correlations, predictions) — over-engineering for v1
- Sleep / health data integration — separate product

## 3. User Flows

### Flow 1: First-launch onboarding [primary]

1. **Entry:** Fresh install → app opens to welcome screen
2. Welcome screen: "Build a habit you'll keep" + Continue button
3. Habit creation: name field (placeholder: "Morning meditation") + icon picker + color picker → Continue
4. Reminder time: time picker (default 8am) + skip option
5. Permission ask: notification permission system dialog
6. **Outcome:** Land on home screen with the new habit ready to be tapped

**Failure paths:**
- User denies notification permission → habit created, banner on home: "Reminders are off, enable in Settings"
- User skips reminder time → habit created with no reminder, can add later in detail view

### Flow 2: Daily habit logging

1. **Entry:** Push notification at scheduled time → tap → opens app to home
2. (Or: open app from home screen, no notification)
3. Home screen shows today's habits as tappable cards
4. Tap a habit → instant haptic feedback + animation (card fills with color, checkmark appears)
5. (Optional) Tap habit again to undo
6. **Outcome:** Habit marked complete for today, streak count visible

**Failure paths:**
- User taps wrong habit → undo via second tap (no confirmation modal — too friction-heavy)
- User wants to log retroactively for yesterday → goes to habit detail → calendar grid → tap missed day to log

### Flow 3: Review progress

1. **Entry:** Tap habit card → habit detail screen
2. View streak count (big number, hero)
3. Scroll: calendar heatmap (12 months)
4. Scroll: completion rate stats (this week, this month, all time)
5. Scroll: edit / delete buttons
6. **Outcome:** User has visual feedback of progress

**Failure paths:**
- Detail screen feels empty for new habits (1-2 days old) — show motivational text instead of empty stats

## 4. Screen Inventory

| ID | Screen | Purpose | Entry points | Key actions |
|----|--------|---------|--------------|-------------|
| S1 | Home (Today) | Main daily logging surface | App launch, push tap | Tap to log habit, swipe for tomorrow preview |
| S2 | Habit detail | View progress, edit habit | Tap habit card on home | View stats, edit, delete |
| S3 | Add / edit habit | Create or modify habit | + button on home, edit on detail | Set name, icon, color, frequency, reminder |
| S4 | Stats / overview | Aggregate view of all habits | Bottom tab "Stats" | View grid of all habits, week summary |
| S5 | Settings | App preferences | Bottom tab "Settings" | Toggle notifications, theme, sync, paywall entry |
| S6 | Onboarding | First-launch intro | First app launch | Create first habit, set reminder |
| S7 | Paywall | Convert to premium | Trying to add 6th habit, advanced stats | View tiers, upgrade |

## 5. Per-Screen Briefs

### S1 · Home (Today)

- **Information hierarchy:**
  - H1: Today's date + day name (large, top)
  - H2: Habit cards (the hero — 70% of screen)
  - H3: Bottom tab bar
- **Key elements:**
  - Date header with motivational text ("3 habits to go" / "All done — nice!")
  - Habit cards: icon, name, current streak count (e.g., "🔥 5"), tap-to-log
  - + button (top right or floating) to add new habit
  - Bottom tab: Today / Stats / Settings
- **States:**
  - **Empty:** Illustration of an empty calendar + "Tap + to add your first habit" + Add button
  - **Loading:** Skeleton cards (shouldn't take >100ms — local data)
  - **Error:** Sync error inline banner "Last synced 5 min ago, tap to retry"
  - **Success:** All habits logged → confetti animation + "All done today" celebration text
- **Interactions:**
  - Tap habit card → log + haptic + color fill animation
  - Long-press habit card → drag to reorder
  - Tap habit card while logged → undo
  - Swipe left/right → preview tomorrow / yesterday (read-only)

### S2 · Habit detail

- **Information hierarchy:**
  - H1: Habit name + icon (top)
  - H2: Current streak count (huge number)
  - H3: Calendar heatmap
  - H4: Stats and edit buttons
- **Key elements:**
  - Streak count with flame icon, formatted as "🔥 12 days"
  - Calendar heatmap: 12-month grid, colored cells = completed days
  - Stats: completion % (this week, this month, all time)
  - Reminder time display + edit
  - Edit / Delete buttons at bottom
- **States:**
  - **Empty (new habit, no data):** "First day! Log it on home screen to start your streak"
  - **Loading:** Skeleton heatmap
  - **Error:** Heatmap failed to load → inline retry
  - **Success:** Full heatmap visible with streak
- **Interactions:**
  - Tap heatmap cell → log/unlog that specific day (retroactive)
  - Tap reminder time → time picker
  - Tap edit → S3 edit mode
  - Tap delete → confirmation modal "This will erase your <streak> day streak"

### S3 · Add / edit habit

- **Information hierarchy:**
  - H1: "New Habit" / "Edit Habit"
  - H2: Name field (the primary input)
  - H3: Icon, color, frequency, reminder
- **Key elements:**
  - Name text field (auto-focus on add)
  - Icon picker: 24 SF Symbol icons in grid
  - Color picker: 6 colors in row
  - Frequency: segmented control (Daily / Weekdays / Custom)
  - Reminder time: time picker (optional)
  - Save button (sticky bottom)
- **States:**
  - **Empty:** All defaults selected, name field empty
  - **Loading:** N/A (instant local op)
  - **Error:** Validation: empty name → inline red text "Give your habit a name"
  - **Success:** On save → dismiss to home, new habit visible
- **Interactions:**
  - Auto-keyboard on entry (name field focus)
  - Tap icon/color → instant selection feedback
  - Frequency picker tap → segmented switch
  - Cancel button (X top-left) → dismiss without saving (with "Discard?" confirm if changes)

### S4 · Stats / overview

- **Information hierarchy:**
  - H1: Period selector (this week / this month / all time)
  - H2: Aggregate stats (total habits, completion %)
  - H3: Per-habit grid
- **Key elements:**
  - Period segmented control
  - Aggregate: "X habits, Y% completion this week"
  - Grid of all habits with mini-heatmaps (last 4 weeks)
  - Trends: "best streak", "most consistent habit"
- **States:**
  - **Empty (no habits yet):** "Add a habit to see your stats" + CTA to S3
  - **Loading:** Skeleton grid
  - **Error:** Inline retry
  - **Success:** Full data visible
- **Interactions:**
  - Tap habit in grid → S2 detail
  - Tap period → segmented switch updates everything
  - Pull to refresh

### S5 · Settings

- **Information hierarchy:**
  - Grouped sections: Account / Notifications / Appearance / Data / About
- **Key elements:**
  - Account: signed-in email or sign-in CTA
  - Notifications: master toggle, per-habit list with times
  - Appearance: theme picker (light / dark / system), accent color
  - Data: iCloud sync toggle, export, import, delete all
  - Premium: upgrade CTA (if free tier) or "Premium" badge
  - About: version, privacy, terms, contact
- **States:**
  - **Empty (not signed in):** Account section shows "Sign in to sync"
  - **Loading:** N/A (settings are local + cached)
  - **Error:** Sync errors inline per row
  - **Success:** Standard settings list
- **Interactions:**
  - Tap row → drill into sub-screen (notifications detail, appearance detail) or modal
  - Toggles inline

### S6 · Onboarding

- **Information hierarchy:**
  - 2 sequential screens, hero-style each
  - Screen 1: welcome
  - Screen 2: first habit creation (= simplified S3)
- **Key elements:**
  - Welcome: large illustration, headline, sub-headline, Continue button
  - Habit creation: name + icon + color + reminder time, Continue button
  - Permission ask: native iOS notification dialog
- **States:**
  - **Empty:** N/A (always fresh)
  - **Loading:** N/A (fast)
  - **Error:** N/A (no network needed)
  - **Success:** → home screen with new habit
- **Interactions:**
  - Continue button progresses
  - Skip available on Screen 2 (creates default sample habit)

### S7 · Paywall

- **Information hierarchy:**
  - H1: Value prop ("Unlimited habits + advanced stats")
  - H2: Tier comparison
  - H3: Restore purchases / dismiss
- **Key elements:**
  - Hero illustration
  - Free tier: "5 habits, basic stats"
  - Premium tier: "Unlimited habits, advanced stats, widgets, priority support"
  - Pricing: monthly / yearly (yearly highlighted with "save 40%")
  - CTA: "Start free trial"
  - Small links: Restore purchases, Privacy, Terms
- **States:**
  - **Empty:** N/A (always pre-populated)
  - **Loading:** Loading prices from StoreKit (skeleton)
  - **Error:** "Couldn't load prices, retry" inline
  - **Success:** Full paywall visible
- **Interactions:**
  - Tap CTA → StoreKit purchase sheet
  - Tap dismiss (X) → back to where they came from
  - Tap monthly/yearly → toggle highlight

## 6. Constraints & Context

- **Platform:** iOS only (v1)
- **Devices:** iPhone 12+ (iOS 17+). iPad shows iPhone-sized layout (no iPad-specific UI v1).
- **Accessibility:** Dynamic Type support, VoiceOver labels on all interactive elements, color blindness-safe color picker (test with Sim Daltonism).
- **Localization:** English v1. ru / es / zh-CN planned for v1.5 — i18n hooks ready.
- **Performance budget:** App cold start < 500ms, habit log feedback < 16ms (one frame).
- **Auth model:** Optional account (email or Sign in with Apple) for iCloud sync. App fully functional without auth (local storage).
- **Data sources:** Local Core Data + optional CloudKit sync.
- **Offline behavior:** Full offline. Sync conflicts resolved last-write-wins per habit.

## 7. Design Context (for huashu)

- **Existing design system:** No
- **Brand assets available:**
  - Logo: needs collection (recommend simple wordmark)
  - Colors: needs collection
  - Fonts: needs collection (recommend SF Pro for system feel + serif accent for hero numbers)
  - Product images / UI screenshots: N/A (this is design from scratch)
- **References / inspiration:** Streaks (the gold standard), Productive (overdone), Done (too minimal), Apple Health (Apple-native feel)
- **Design direction known:** No — recommend huashu fallback advisor mode (suggest 3 directions: Apple-native warm / minimal monochrome / playful illustrated)
- **Brand voice / tone:** Warm but not infantile. Encouraging but not preachy. Uses the user's first name only sparingly.

## 8. Hand-off to huashu-design

### 8.1 Recommended delivery format

- [x] **`cjm-canvas`** — 7 screens, 3 flows (onboarding / daily logging / review progress), with paywall + auth state branches. Fails all 3 skip-conditions, so cjm-canvas is the right call.
- [ ] `hi-fi-static`

**Reasoning:** 7 screens with shared visual language and 3 distinct flows. CJM canvas lets the user click through onboarding → home → detail → paywall in one HTML file, toggle streak-viz / color-system / home-layout variants per screen, and copy a lock-in prompt back to Claude when picks settle.

### 8.2 Information density type

- [ ] Restrained
- [x] **High-density** — habit trackers earn their keep by surfacing streaks, stats, heatmaps. Plain "list of names" feels generic.

**Reasoning:** Without dense data (streak counts, heatmaps, stats), the app looks like a generic todo. The product's edge is data visualization.

### 8.3 Per-screen position-4 answers

| Screen | Narrative role | Audience distance | Visual temperature | Capacity check |
|--------|---------------|-------------------|---------------------|----------------|
| S1 Home | hero | 10cm phone | warm-encouraging | high-density risk |
| S2 Habit detail | data | 10cm phone | analytical-warm | OK |
| S3 Add / edit | transition | 10cm phone | neutral-focused | OK |
| S4 Stats | data | 10cm phone | analytical | OK |
| S5 Settings | end | 10cm phone | calm | OK |
| S6 Onboarding | hero | 10cm phone | inviting | OK (max breathing) |
| S7 Paywall | hero | 10cm phone | confident-warm | OK |

### 8.4 Variation dimensions to explore

- **Dimension 1: Home layout** — list of cards (default) / grid 2-col (more habits visible) / single hero card with carousel (focused)
- **Dimension 2: Streak visualization** — flame + number (Streaks-style) / progress ring / calendar dots
- **Dimension 3: Color system** — single accent (calm) / per-habit colors (playful) / monochrome with accent only on completed (refined)

**Variation count recommendation:** 3

**Reasoning:** Home layout drives the entire feel. Streak viz is the emotional hook. Color system signals brand personality.

### 8.5 Tweaks worth exposing

- Theme (light / dark / system) [scope: global]
- Accent color (5 options) [scope: global]
- Streak visualization style (flame / ring / calendar) [scope: S1, S2, S4]
- Card density (compact / comfortable / spacious) [scope: S1, S4]
- Home layout (list / grid 2-col / hero carousel) [scope: S1] — §8.4 DIM 1
- Color system (single accent / per-habit / monochrome) [scope: S1, S2, S4] — §8.4 DIM 3
- Heatmap range on detail (12mo / 6mo / 3mo) [scope: S2]
- Paywall tier emphasis (yearly highlighted / monthly highlighted / equal) [scope: S7]

### 8.6 Brand asset checklist

- [ ] Logo provided / found
- [ ] Product images / UI screenshots provided (N/A — designing from scratch)
- [ ] Colors specified
- [ ] Fonts specified
- [ ] Reference inspiration provided (Streaks, Productive)
- [x] **Recommend huashu run §1.a Core Asset Protocol** for logo design + reference UI screenshots from competitors

### 8.7 Canvas construction hint (for huashu)

Start from `skills/ux-planner/assets/canvas-scaffold.html` and replace the HUASHU-marked blocks with the content below. Do NOT change the canvas-shell logic (state model, `buildLockInPrompt`, `CopyLockInButton`) — that's the v1.3 contract.

cjm-canvas single HTML (React + Babel via CDN). Stage area: iframe-wrapped iPhone frame (replace browser chrome from scaffold with an iOS-frame variant). Above frame: pill `S<id> · <NAME>`. No browser chrome (mobile app, not web).

Right sidebar (~360px sticky, 4 blocks):
1. **Tweaks** — filter §8.5 by active screen `[scope]`. Tweaks tagged with `§8.4 DIM <n>` headings render as primary toggle groups; non-DIM tweaks below in a "Polish" section.
2. **Flow steps** — three flows from §3 (Onboarding / Daily logging / Review progress), each as a numbered list with active step highlighted. Default flow on canvas open = Daily logging starting at S1.
3. **Alternate states** — sourced from §5 states + §9 risks. Examples:
   - `S1 Home · Empty (first-launch)` — VARIANT 1
   - `S1 Home · All done celebration` — VARIANT 2
   - `S2 Detail · New habit no data` — VARIANT 3
   - `S7 Paywall · StoreKit error` — VARIANT 4
   - `Sync conflict banner` — SWAP (overlays any screen)
4. **Meta footer** — `SOURCE · ux-spec-2026-04-28-habit-tracker.md` / `SYSTEM · — (no design system)` / `DENSITY · HIGH-DENSITY`.

Sticky bottom button: **"Copy lock-in prompt"** — accumulates every tweak the user explicitly touched across every screen they visited in this session (cross-screen `tweakState` + `touchedKeys` Set, persists when navigating via Flow steps). Button label carries a live counter `Copy lock-in prompt · N picks across M screens`. Empty-state click (`touchedKeys.size === 0`) shows toast `Pick at least one tweak before copying` and skips clipboard write. Success: 2-second toast `Copied · N picks across M screens` with `document.execCommand('copy')` fallback for `file://` permission issues.

### 8.8 Lock-in prompt template (for the Copy button)

```
Lock these design choices into the UX spec at /Users/me/Desktop/projects/habit-tracker/ux-spec-2026-04-28-habit-tracker.md:

Global:
- §8.4 DIM <n> <NAME>: <SELECTED-VARIANT>
- <Tweak label>: <SELECTED-VARIANT>
(emit one line per touched key with [scope: global]; omit Global block if zero global picks)

Screen <S-id-A> · <SCREEN-NAME-A>:
- §8.4 DIM <n> <NAME>: <SELECTED-VARIANT>
- <Tweak label>: <SELECTED-VARIANT>
(emit one line per touched key with that screen in [scope])

Screen <S-id-B> · <SCREEN-NAME-B>:
- <Tweak label>: <SELECTED-VARIANT>
(repeat one block per touched screen; ordered by S-id ascending)

Action: update §8.4 — mark these variants as "locked" (globally for Global block, per-screen for Screen blocks) and move non-chosen variants to §9.5 Considered Alternatives. If the same DIM is locked to different variants across blocks, do NOT lock — record the conflict in §9.5 and surface it back to the user. Re-run §6 self-review and regenerate the §8 hand-off phrase.
```

## 9. Open Questions & Assumptions

### Assumptions made (verify these)

- **Assumption:** Free tier limited to 5 habits, premium unlimited. If business model differs, S7 paywall changes.
- **Assumption:** Habits are private (no sharing). Confirmed in §2 out-of-scope.
- **Assumption:** Notification permission asked during onboarding. Could ask just-in-time on first habit log instead.
- **Assumption:** iOS 17+ minimum. If supporting older iOS, some animation patterns change.

### Open questions (need user input later)

- **Q:** Pricing — what are the actual monthly / yearly numbers? — **why it matters:** Paywall copy needs real numbers; "save 40%" depends on actual ratio.
- **Q:** What's the exact onboarding language? — **why it matters:** Onboarding tone sets brand personality.
- **Q:** Premium-only features list — currently inferred (unlimited habits, advanced stats, widgets, priority support). Confirm? — **why it matters:** Determines paywall value prop.

### Inferred from archetype defaults

- Bottom tab nav (3 tabs)
- Modal sheet for add/edit
- Local storage + optional cloud sync
- Push notifications as primary re-engagement
- Long-press to reorder, tap-to-log

### Product Risks

- **CloudKit sync conflicts:** two devices logging same habit on the same day produces duplicate entries or last-write-wins data loss. Mitigation — per-day idempotency key + reconcile on open.
- **Notification permission denied:** core re-engagement loop dies silently. Mitigation — soft pre-prompt before system dialog + visible "reminders off" banner with 1-tap re-enable from Settings.
- **Streak break frustration:** missing one day after a long streak triggers user churn. Mitigation — v1.5 streak-recovery feature + soft empathetic copy on detail screen the day after a break.
- **StoreKit pricing failure:** prices fail to load → paywall is empty → conversion lost. Mitigation — cached price snapshot + retry-on-foreground.
- **Free→Premium downgrade silently:** user with 8 habits downgrades to free (5-cap) → which 3 to disable? Mitigation — explicit pick-3 modal on downgrade, never auto-truncate.
- **Notification spam from too many habits:** 5 habits × daily reminders feels noisy. Mitigation — single "today's habits" digest notification at user-chosen time, not per-habit.

### Considered Alternatives (§9.5)

> Empty at first generation. Will populate when the user runs the cjm-canvas Copy button and pastes the lock-in prompt back.
