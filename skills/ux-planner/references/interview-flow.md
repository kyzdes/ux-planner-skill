# Interview Flow

The interview is **hybrid**: one batch of 5 questions upfront, then adaptive targeted follow-ups (max 5 per round). Don't pelt the user with 30 atomic questions — they'll answer half and quit.

---

## Batch 1 (Phase 1) — 5 questions in one message

Use this template verbatim. Match the user's input language for the question text — Russian if they wrote Russian, English if English. **The output spec is always English; only the questions match the user's language for friendliness.**

### Russian version

```markdown
Чтобы собрать UX-спеку, отвечу один раз на эти 5 вопросов (можно коротко, в свободной форме):

1. **Что делаем?** mobile app / web app / dashboard / landing / internal tool / browser extension
2. **Для кого?** короткий портрет основной аудитории (роль, контекст использования)
3. **Какую боль решаем?** одно-два предложения. Идеально в формате: "когда X, хочу Y, чтобы Z"
4. **Платформа?** iOS only / Android only / both / web responsive / desktop / native + web
5. **Дизайн-контекст:** есть ли существующая design system, брендбук, референсы конкурентов? Если есть — пути/ссылки. Если нет — так и пиши "ничего нет".
```

### English version

```markdown
To build the UX spec, answer these 5 questions in one go (short and free-form is fine):

1. **What are we building?** mobile app / web app / dashboard / landing / internal tool / browser extension
2. **For whom?** short portrait of the primary audience (role, usage context)
3. **What pain are we solving?** one or two sentences. Ideal format: "When X, I want Y, so that Z"
4. **Platform?** iOS only / Android only / both / web responsive / desktop / native + web
5. **Design context:** any existing design system, brand book, competitor references? Give paths/links if yes. If no, just write "nothing yet".
```

### Skip rules for Phase 1

- **Skip Q1** if Phase 0 archetype detection is unambiguous (e.g., user said "сделай dashboard для сервера" → it's clearly a dashboard)
- **Skip Q4** if archetype implies platform (e.g., landing → web, browser extension → web)
- **Never skip Q2, Q3, Q5** — those are about audience, JTBD, and existing context, which can't be inferred reliably

If you skip a question, say so explicitly in the batch message: "I'll skip Q1 because you already said 'dashboard'. So 4 questions:..."

### When user answers partially

If user answers 3 of 5: don't ask the missing 2 immediately. Use what you have, infer the rest, mark as **Assumption** in the draft, and let them push back during Phase 2.

If user answers 0-2: gently re-ask, but only the most-load-bearing question (usually Q3 JTBD).

---

## Inferred draft (Phase 2) — present as a single block

After Phase 1, generate a draft. Use archetype defaults from `product-archetypes.md` heavily. Show this format:

```markdown
On the basis of your answers I drafted this. Edit / add / remove anything, or say "ok, next":

**Must-have features (v1):**
- <feature 1>
- <feature 2>
- <feature 3>
- <feature 4>
- <feature 5>

**Nice-to-have (v1.5):**
- <feature>
- <feature>

**Main flows:**
1. **<Flow 1 name>:** entry → step → step → outcome
2. **<Flow 2 name>:** entry → step → step → outcome
3. **<Flow 3 name>:** entry → step → step → outcome

**Screens (~N):**
- S1 · <Screen> — <one-line purpose>
- S2 · <Screen> — <one-line purpose>
- ...

**Inferred assumptions** (push back if wrong):
- <thing inferred>
- <thing inferred>

Edit anything or say "next".
```

### Phase 2 tone

- Russian when user wrote Russian; English when English. Internal feature names stay English.
- Be confident, not tentative. "Drafted this" not "Maybe we could".
- Cap at ~80 lines of draft total. More than that → user disengages.

### Handling Phase 2 edits

- Small edits ("уберём stats screen") → fold in silently, show only the changed bullet
- Big edits ("вообще не то, я хочу B2B инструмент") → restart from Phase 0/1 with new archetype
- Edits that introduce new product shape ("а ещё бы паблик API") → note in §2 nice-to-have or §6, don't blow up scope

---

## Phase 2.5 — Mobile / Responsive scope confirmation (single question)

After the user confirms the Phase 2 draft, ask exactly one yes/no question. This is a one-message interrupt before Phase 3 follow-ups.

**Trigger:** archetypes `web-productivity`, `dashboard`, `landing`, `internal-tool`. **Skip** for `mobile-utility` and `mobile-content` (already mobile-primary).

### Russian version

```markdown
Хочешь чтобы я спроектировал отдельную мобильную/респонсив-версию для этих экранов? (Y/N)

Если **Yes** — в спеке появится §10 Mobile Block: per-screen мобильное поведение, тач-взаимодействия, mobile-only состояния, и в каждый brief в §5 добавится sub-bullet "Mobile adaptation".

Если **No** — §6 всё равно содержит per-breakpoint feature parity (это всегда), но §10 не генерируется. Mobile UX можно будет проработать отдельной сессией позже.
```

### English version

```markdown
Want me to design a separate mobile/responsive UX track for these screens? (Y/N)

**Yes** adds §10 Mobile Block: per-screen mobile behavior, touch interactions, mobile-only states, plus a "Mobile adaptation" sub-bullet on every §5 brief.

**No** keeps the per-breakpoint feature parity table in §6 (always required) but skips §10. Mobile UX can be handled in a follow-up session later.
```

### Default behavior on ambiguous answer

If user replies with anything other than a clear Yes / No / Y / N / да / нет / "пропусти" / "skip", ask the question once more in shorter form. After that, default to **No** and add an entry to §9 Open Questions: "Mobile UX deferred — covered in a follow-up session if needed."

### What changes when mobile track = Yes

- §5 Per-Screen Briefs: each brief gets a `**Mobile adaptation:**` sub-bullet
- §6 Constraints: full per-breakpoint feature parity table (mandatory anyway, but with extra detail)
- §8.3 Position-4: add "Audience distance (mobile)" column with explicit 10cm-phone reasoning
- §10 Mobile / Responsive Design Block: full section per the spec template
- Hand-off phrase: append `Honor §10 mobile/responsive specifications when designing mobile/tablet variants.`

---

## Adaptive follow-ups (Phase 3) — max 5 per round, max 2 rounds, ≤8 total

Read the draft. Ask targeted questions about specific gaps. Use this trigger table:

| Detected gap | Question to ask | Why it matters |
|---|---|---|
| Auth not specified | "Auth: email / social / passkey / magic link / guest-mode-only?" | Onboarding length, first-launch screen design |
| Onboarding not specified | "Onboarding: skippable tour / forced data collection / sample data / nothing?" | First-run UX, time-to-value |
| Monetization not specified (consumer app) | "Free / freemium / subscription / one-time / no monetization v1?" | Where paywalls appear, settings layout |
| Empty states not addressed | "What does a brand-new user see on first home-screen launch?" | Hero design, CTA placement |
| List + detail product without filtering | "Search and filters needed v1? If yes, by what fields?" | List screen complexity |
| Settings unclear | "What settings categories do you expect? (account / notifications / appearance / data / about / etc.)" | Settings IA depth |
| Notifications not specified | "Push / email / in-app only / none?" | Permission flows, notification screens |
| Data sources unclear | "Data: user-entered / synced / external API / manual import?" | Empty states, sync indicators |
| Multi-user / collaboration unclear | "Single user / multi-device sync / collaborative / org-level?" | Auth complexity, share flows |
| Offline behavior unclear | "Offline: full / read-only / online-required?" | Loading patterns, error states |
| Internationalization | "Languages v1? Future-ready for more?" | Layout flexibility, text expansion |

### Phase 3 rules

1. **Hard caps:** max **5 questions per round** AND max **2 rounds total** (≤8 questions across the whole interview). After round 2, any remaining gaps become `**Assumption:**` markers in the spec or entries in §9 Open Questions.
2. **Group related questions.** Ask all auth + onboarding together, not separately.
3. **Skip if user explicitly opted out.** If user said "не задавай вопросы, делай" → respect it. Make best-judgment assumptions, list them in §9.
4. **Two rounds maximum.** After round 2, move to Phase 4. Don't open round 3 — surface remaining gaps in §9 Open Questions instead.

### Phase 3 message format

```markdown
Quick targeted questions about gaps in the draft (answer in any order):

1. **Auth:** ...?
2. **Onboarding:** ...?
3. **Monetization:** ...?
4. **Empty state on first launch:** ...?
5. **Notifications:** ...?

If you don't know — write "skip" and I'll make an assumption you can verify later.
```

---

## Advisor mode (when to switch)

### Triggers

Switch to advisor mode when:

- User says: "не знаю", "не понимаю", "помоги", "как лучше", "что обычно делают", "I don't know", "help me figure out", "what would you recommend"
- Phase 1 returns 3+ "не знаю / no idea" answers
- Phase 0 archetype detection is fully ambiguous after reading user message
- User cycles back through Phase 2 edits without converging (3+ contradictory edits in a row)

### Action

Propose **2-3 differentiated options** with one-line tradeoffs. **Not more than 3** (choice paralysis). **Not just 1** (that's prescription, not advisory).

### Format

```markdown
Несколько направлений в которые это может пойти:

**(a) <Option A name>** — <archetype>. Сильные стороны: <X>. Цена: <Y>.
**(b) <Option B name>** — <archetype>. Сильные стороны: <X>. Цена: <Y>.
**(c) <Option C name>** — <archetype>. Сильные стороны: <X>. Цена: <Y>.

Какой ближе?
```

### Advisor examples (UX-level, not visual)

**Example 1: vague finance app**

> (a) **Personal expense tracker** — mobile-utility. Сильные: быстрый ввод, понимание тратишь / зарабатываешь. Цена: не помогает с инвестициями.
> (b) **Investment dashboard** — dashboard. Сильные: следить за портфелем, ребалансинг. Цена: ввести портфель — это onboarding-ад.
> (c) **B2B accounting tool** — internal-tool. Сильные: счета / клиенты / налоги. Цена: огромный scope, multi-user, compliance.

**Example 2: vague "что-то для здоровья"**

> (a) **Habit tracker** — mobile-utility. Сильные: ежедневный лог, стрики, простота. Цена: однообразный паттерн, мотивация падает через месяц.
> (b) **Sleep / activity dashboard** — mobile-utility + dashboard hybrid. Сильные: данные с Apple Health / Google Fit. Цена: зависишь от качества интеграций.
> (c) **Telehealth booking** — mobile-app или web-app. Сильные: реальная польза, монетизация очевидна. Цена: B2B-сложности, регуляторика.

**Example 3: vague onboarding question**

> Onboarding-варианты:
> (a) **Interactive tour** — tooltip каждый шаг. Сильные: пользователь учится в контексте. Цена: 30% скипают, дольше TTV.
> (b) **Sample data on first launch** — пустой UI пугает, наполняем демо-данными. Сильные: сразу видно как продукт выглядит. Цена: сложно отличить демо от реальных данных.
> (c) **Empty + tooltips on first action** — UI пустой, hint появляется когда юзер тыкает. Сильные: быстро TTV, нет навязывания. Цена: нужно сразу понять какие первые действия очевидны.

### After user picks

Lock in the choice and continue from where you were. Don't re-advisor unless they bounce again.

---

## Re-asking handling

If the user revises an earlier answer mid-flow ("вообще-то это не для разработчиков а для дизайнеров") — **fold it in, don't restart**. Update the affected sections of the draft and continue. Mention what changed:

> "Got it — flipped audience to designers. Updated draft features (3 changed), keeping flows. Continuing with Phase 3."

---

## Skipping the interview entirely

If user provides a fully detailed brief in their first message (audience + JTBD + features + design context all clearly stated), **skip Phase 1** and go straight to Phase 2 draft confirmation. Tell them:

> "Brief is detailed enough — skipping Q&A. Drafted spec from your description, review and edit:"

Don't mechanically run Phase 1 if it's redundant.

---

## Language of the conversation vs. spec

| Element | Language |
|---|---|
| Phase 1-3 questions | Match user (RU or EN) |
| Phase 2 draft | Match user (RU or EN) |
| Phase 4-5 internal reasoning | Either |
| Phase 6 final saved spec file | **Always English** |
| Phase 7 hand-off message | Match user, but the quoted huashu command is English |

This keeps the user comfortable and makes huashu's input consistent (huashu reads English best per its own training mix).
