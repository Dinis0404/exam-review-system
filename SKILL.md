---
name: exam-review-system
description: Turn a student's own exam/study materials (photos of worksheets & handouts, textbook pages, exam outlines, PDFs, DOCX) into ONE interactive, offline HTML review system — teaching from zero, memorization cards, auto-graded practice (cloze blanks, MCQ with explanations, classification drills), exam-day strategy, and writing guides. Use whenever the user provides study materials and wants a 復習/复习/review page, study website, exam prep, practice questions from their worksheets, a topic deep-dive, or says things like "幫我做一個復習的系統/網頁", "把我的資料變成練習", "make a study guide from these photos" — even if they just dump a folder of handout photos without saying "review".
---

# Exam Review System Builder

Build ONE interactive, offline HTML page from the student's own materials. Quality bar: a student with the exam tomorrow opens it, learns from zero, drills exactly what will be tested, and walks in with an exam-day plan. Everything on the page must trace back to their materials.

## Why it works this way

- **Their materials are the syllabus.** Photos of marked worksheets contain the answer key; the exam outline names which items are "seen" (free marks). Content sourced from these beats any generic content you could invent.
- **One standalone file.** No CDN, no network, no build step — it opens by double-click, works on a phone, survives the exam room, and can be published as a shareable site later.
- **Teach → memorize → drill → strategy.** Students who "don't understand anything" need teaching first; what must be memorized needs condensing; then drilling; then exam tactics.

## Workflow

### 1. Read every page before writing anything

- Collect the material paths the user names. Photos are usually dumped in one folder — `ls` it first.
- Read images in batches of 3–4 per `Read` call; keep going until every image is read. Never guess what a page says.
- Big phone photos (3000×4000, 4–6 MB) often arrive illegible or time out; tall web captures (e.g. 1920×11301) arrive shrunk to unreadable, and sideways shots are rotated. Preprocess with PIL before reading: downscale to ≤1600px on the long side, slice anything taller than ~2:1 into segments, rotate 90° and retry when text reads sideways. One batch of preprocessing beats three failed reads.
- Read the exam-scope sheet first — it names the sections, mark weights and exact texts; everything else is supporting material for those targets.
- **Handwritten answer markings on worksheets are the answer key.** Transcribe them (e.g. `1.B 2.B 3.C…`). Double-check any marking you're unsure about; ambiguity → see the fidelity rule below.
- From the exam outline, extract: sections, mark weights, and which items are **seen** (from their own worksheets/passages — flag these as "free marks") vs **unseen**.

### 2. Map scope before building

Make a quick internal table: exam section → material source → seen/unseen → practice form. State it in your reply so the student sees the plan. Prioritize "seen" items — they are guaranteed questions.

### 3. Pick the section mix (subject-adaptive)

Default tab order — teaching first, memorization right after, then practice:

1. **Teaching** — one tab per major topic, written for someone who knows nothing. Reuse the formulas/rules/examples from their own handouts so the page feels familiar.
2. **⚡ Memorization cards** — the condensed must-remember lists (formulas, trigger words, Q&A rules, common-trap red lights, exam-day checklist). This is the tab they re-read at the exam door.
3. **Practice** — see the catalog below. Every item gets an explanation in the student's language.
4. **Exam strategy** — day-of checklist (what's seen/unseen, word limits, marking rules) + tactics for unseen question types (process of elimination, comma-splice checks…).
5. **Writing guide** (if essays are tested) — structure, reusable sentence patterns (bonus: patterns using the grammar just taught), sample outline, pre-submit checklist.

Subject adaptation:

| Subject | Practice forms that fit |
|---|---|
| Languages (English/Chinese…) | typed cloze from seen passages (many meaningful blanks), MCQ grammar/vocab with explanations, sign/notice reading, sentence joining, essay guide |
| STEM (Math/Physics/Chem) | worked examples step-by-step, formula cards, "spot the trap" MCQs, unit-conversion drills |
| Humanities/History | timeline ordering, cause→effect matching, key-term flashcards, source-analysis MCQs |

### 4. Build the HTML

- Start from `assets/quiz-engine-template.html` — a proven engine (tabs, MCQ renderer, typed-cloze renderer, classification drill, dark mode). Edit its data arrays; keep the engine code.
- For a dark-mode-first modern UI (user asks for 現代/深色/好看/重新設計), start from `assets/dark-hub-template.html` instead: home hub of function cards, flip-card deck for memorization, sequential true/false flash, grouped MCQ, typed cloze with 簡繁-tolerant checking. Same rule — swap the TODO data arrays, keep the engine.
- Read `references/page-patterns.md` when you need the section catalog details, design conventions, or content-sourcing rules.
- Non-negotiables: UTF-8; zero external requests; responsive down to 320px; dark mode via `prefers-color-scheme`; print CSS hides buttons and shows all tabs; data-driven JS (`QUIZZES`, `CLOZES` arrays); cloze blanks use `{{answer|Chinese hint}}` markers; target < 300 KB.

### 5. QA before delivering

Run this every time — silent breakage is the #1 failure mode:

```bash
python -I - <<'EOF'
import re, io
s = io.open(r"<FILE>", encoding="utf-8").read()
js = re.search(r"<script>(.*?)</script>", s, re.S).group(1)
io.open(r"<TMP>/check.js", "w", encoding="utf-8").write(js)
html_ids = set(re.findall(r'id="([^"]+)"', s))
js_ids = set(re.findall(r"getElementById\('([^']+)'\)", js))
print("missing:", sorted(i for i in js_ids if i not in html_ids))
EOF
node --check <TMP>/check.js && echo "JS OK"
```

- Also count blanks/questions in the data vs what you intended, and check every quiz container + score element exists.
- If you regenerated an existing page, verify content preservation: question/blank/card counts must match the previous version, and spot-check a few answers and explanations.
- Recommended browser smoke test (JS can pass `node --check` yet still break at runtime): serve the folder over local HTTP, open at 1280px and 390px, switch through every tab, submit one answer per practice type, and assert `document.scrollingElement.scrollWidth <= window.innerWidth` on mobile (no horizontal overflow).
- Fix everything before reporting. If `node` is unavailable, review the script by hand for unclosed brackets and quote mismatches.

### 6. Deliver

- Save the file **next to the student's materials** (same folder they dumped the photos in), named in their language, e.g. `英文考試復習.html`.
- Report: path, section map, counts (blanks/questions), and a suggested study order for tonight.
- **Then ask once whether to deploy**: 「要不要部署到 ZCode Sites(代碼托管),生成一條手機也能開的分享連結?」Ask proactively after delivering — don't wait for the user to mention sharing. If they say yes, load the `site-deploy` skill and deploy as a static site; if no, move on without nagging.
- Deploy recipe (single standalone HTML, no build step): stage root holds `index.html` (the review page itself), `static-server.mjs` (from the site-deploy templates), and `start.sh` with `SITE_DIR=.` and `SPA_FALLBACK=0`; then `zcode-site check .zcode-site/stage` and `zcode-site deploy .zcode-site/stage --new --name "<display name>" --prefix <english-prefix>`. Pick a prefix from the subject (e.g. `chinese-exam-review`), not `app`.
- Confirm before uploading: the page is public to anyone with the link — never include personal data (names, class numbers) from the materials. One yes covers redeploys of the same site in that conversation.

### 7. Variants (same engine, different focus)

- **Single-topic deep dive** (e.g. "just inversion, as detailed as possible"): one scroll page, no tabs; teach the taxonomy the student's teacher used, add a classification drill + grouped MCQ.
- **Question bank only**: skip teaching tabs; group MCQ by topic with explanations and scores.
- **Flashcards only**: memorization cards + reveal-to-check interactions.
- **Update an existing page**: regenerate the whole file (never patch fragments), keep the same filename so bookmarks/links survive.
- **UI redesign** ("優化界面/重新設計/深色現代"): keep every question, option, answer, explanation and memo **verbatim** — copy the data arrays, replace only markup/CSS/engine presentation. Proven patterns: home hub of function cards (icon + name + one-line desc); flip-card deck (prev/next, progress bar, long lists scroll inside the card); sequential true/false flash (one card at a time → verdict + explanation + next); cloze answers tolerant of both 簡體 and 繁體 input (normalize through a small S2T char map before comparing). Unseen content (exam-scope table, study order) moves to the strategy/home page — never delete it. Always re-run the browser smoke test at desktop and mobile widths.
- **Deployed page update**: rebuild the stage from the new file and `zcode-site deploy` again — same site, same URL; one earlier yes covers it in that conversation.

## Answer-key fidelity

Worksheet answer markings win — students will cross-check against them. But if a marked answer is clearly ungrammatical or the question is ambiguous, silently prefer the correct answer with a teaching explanation, or skip the item. Never teach something wrong to match a marking.
