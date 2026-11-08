# Page Patterns & Conventions

Details for building the review page. The engine code lives in `assets/quiz-engine-template.html`; this file covers content structure and sourcing.

## Page skeleton

```text
<header>  title, scope chips (one chip per exam section, seen items bolded)
<nav>     tab row (sticky, horizontally scrollable on phones)
<tabs>    ① teaching topics  ② ⚡ memorization cards  ③ practice (cloze)
          ④ practice (MCQ)   ⑤ reading/notices       ⑥ writing guide
<footer>  material sources line
```

For a **single-topic deep dive**, drop tabs: one scroll page with `h2` sections — comparison table first (the taxonomy the student's teacher uses), detailed rules, trap list, exam-sentence patterns, then drills.

## Teaching sections that worked

- **Assume zero knowledge.** Start with "what is X" in 3 numbered rules before any example.
- One comparison/summary table per concept (e.g. certainty ladder, formality ladder, full-vs-partial inversion). Tables beat paragraphs for revision.
- Every rule gets 1–3 example sentences copied **from the student's own handout or textbook** (they will recognize them — that familiarity is the value).
- Mark every non-obvious trap with a red `⚠️` inline warning inside the rule box.
- End each teaching tab with a highlighted `.memo` box: "要背" formulas condensed to ~10 lines.

## Memorization cards tab

7±2 cards, each one compact table or list:

1. Core formulas list (numbered, one line each)
2. The hardest distinction as a mini-table with emoji/feeling anchors (e.g. `must have done` = "certainly did 😎")
3. Q&A response rules (`Must I…? → No, you needn't`)
4. Trigger-word checklist (memorize verbatim)
5. "Red lights" — the traps they'll actually hit, stated as ❌→✓ pairs
6. Exam-day reminders (which sections are seen = free marks)
7. Unseen-question strategy (one tactic per unseen section)

## Practice catalog

| Form | When to use | Engine |
|---|---|---|
| **Typed cloze from seen passages** | passages the exam quotes; make MANY meaningful blanks (content words from the tested vocab list), each with a Chinese hint | `CLOZES` |
| **MCQ with explanations** | worksheets they already have (pick the instructive ones) + self-made items covering gaps; group by topic, ~8–25 per group | `QUIZZES` |
| **Classification drill** | when the topic has a 2–3 way taxonomy (full/partial/no inversion; active/passive; which tense) — trains the judgment step | classify mode |
| **Ordering / matching** | timelines, event sequences — implement as MCQ with the correct order as an option | MCQ |

Content-sourcing rules:

- Cloze answers must be the **exact word in the original text**; hint = the Chinese meaning plus (for grammar-bearing words) the required form (`影響(單數動詞)`).
- Reuse their worksheet questions when the marking is clear; write fresh items for gaps the worksheet doesn't cover.
- Every MCQ explanation: the rule name + why the right option wins + why the tempting wrong one loses. One to two sentences.
- Badge items taken verbatim from a worksheet (the student recognizes them: "we did this one in class").

## Answer-key fidelity

Handwritten markings on worksheets are the answer key — students cross-check. Confidence order: clear marking → standard-grammar answer → skip. If a marked answer is ungrammatical (e.g. a wrong option circled), either correct it in the explanation or drop the item entirely. Never present a wrong rule as right.

## Design conventions (matches the bundled template)

- CSS custom properties, light + dark via `@media (prefers-color-scheme: dark)`; nothing hardcodes light or dark colors.
- Accent color varies per deliverable (indigo for full review, violet for topic deep-dive) so the student can tell pages apart.
- Font stack: `"Segoe UI","Microsoft JhengHei","PingFang TC","Noto Sans TC",system-ui`.
- Interactive targets ≥ 44px on touch; option buttons full-width on narrow screens (`grid minmax(240px,1fr)`).
- `@media print`: hide buttons/nav, force all panels visible — some students print the memo cards.
- Score chips use `aria-live="polite"`; results text in the student's language.
- Inline `<input>` cloze blanks: underline style, `Enter` jumps to next blank, auto-check trims+lowercases, wrong-but-filled marks red, "顯示答案" fills dashed-amber.

## Deliverable checklist

- [ ] Every material page read (count images vs reads)
- [ ] Seen/unseen mapping stated in the reply
- [ ] Teaching assumes zero knowledge; memo cards present
- [ ] Every practice item has an explanation; answers match the key
- [ ] QA script passes (JS syntax + IDs + counts)
- [ ] File saved next to materials, named in the student's language
- [ ] No personal data (names/class numbers) baked into a page that will be shared
