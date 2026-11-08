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
- Fix everything before reporting. If `node` is unavailable, review the script by hand for unclosed brackets and quote mismatches.

### 6. Deliver

- Save the file **next to the student's materials** (same folder they dumped the photos in), named in their language, e.g. `英文考試復習.html`.
- Report: path, section map, counts (blanks/questions), and a suggested study order for tonight.
- If the user wants to share it, deploy as a static site (`dist/index.html` + the site-deploy skill flow). Confirm before uploading; the page is public to anyone with the link — never include personal data (names, class numbers) from the materials.

### 7. Variants (same engine, different focus)

- **Single-topic deep dive** (e.g. "just inversion, as detailed as possible"): one scroll page, no tabs; teach the taxonomy the student's teacher used, add a classification drill + grouped MCQ.
- **Question bank only**: skip teaching tabs; group MCQ by topic with explanations and scores.
- **Flashcards only**: memorization cards + reveal-to-check interactions.
- **Update an existing page**: regenerate the whole file (never patch fragments), keep the same filename so bookmarks/links survive.

## Answer-key fidelity

Worksheet answer markings win — students will cross-check against them. But if a marked answer is clearly ungrammatical or the question is ambiguous, silently prefer the correct answer with a teaching explanation, or skip the item. Never teach something wrong to match a marking.
