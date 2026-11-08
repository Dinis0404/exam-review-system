# exam-review-system

**ZCode skill** — 把你自己的考試材料(講義照片、試卷、課本頁、考試範圍)變成**一個互動式、離線可用的 HTML 復習系統**。
**A ZCode skill that turns your own exam materials (photos of worksheets, textbook pages, exam outlines, PDFs) into ONE interactive, offline HTML review system.**

> 實戰出身:由一次真實的考前復習流程提煉而成——讀完 21 頁材料照片、轉錄手寫答案、生成教學+速記+70 個挖空+58 題選擇題的復習網頁,並驗證上線。
> *Battle-tested: distilled from a real exam-eve session (21 pages of material photos, handwritten answer keys transcribed, 70 cloze blanks + 58 explained MCQs generated, deployed and verified).*

## ✨ 產出什麼|What you get

一個雙擊即開、無需網絡的單檔 HTML 頁面(支援手機/深色模式/列印):

| 區塊|Section | 內容 |
|---|---|---|
| 📖 教學 Teaching | 預設你什麼都不會,按對比表 + 例句教起,例句取自你自己的講義 |
| ⚡ 速記卡 Memo cards | 考試門口最後一分鐘要背的:公式、觸發詞清單、易錯紅燈、當日提醒 |
| 📝 挖空練習 Typed cloze | 把「考試會考的原文」整篇挖空讓你打字默寫,每空有中文提示、即時批改 |
| ✅ 選擇題 MCQ | 按題組分類,每題附講解與計分;含「分類快測」訓練判斷力 |
| ✍️ 作文/策略 Writing & strategy | 題目解析、結構範本、加分句型、考試當日清單 |

## 📦 安裝|Install

把整個資料夾放進 ZCode 的技能目錄(全域):

```
~/.agents/skills/exam-review-system/
```

```bash
# clone into your personal skills dir
git clone https://github.com/Dinis0404/exam-review-system.git ~/.agents/skills/exam-review-system
```

重啟 ZCode 後即可用。**Restart ZCode, then it triggers automatically.**

## 🚀 用法|Usage

直接丟材料+一句話,例如:

- 「這個資料夾是我明天的考試範圍,幫我做一個復習的網頁」
- 「Here are photos of my chemistry notes and past papers — make me a review page」
- 「單獨出一個關於倒裝句的詳細教學+練習」(單主題深挖)
- 做完後:「把它發佈成分享連結」(配合 site-deploy skill 上線)

Skill 會:**讀完每一頁材料 → 轉錄手寫答案 → 標出「讀過=送分題」→ 生成網頁(教學→速記→練習→策略)→ 自動 QA(JS 語法/元素/題數)→ 存到你的材料資料夾**。

## 🧩 技術|Tech notes

- 單檔、零依賴、UTF-8;`prefers-color-scheme` 深色模式;320px 起響應式;列印時自動展開全部頁籤
- 資料驅動引擎:`QUIZZES`(MCQ+講解)、`CLOZES`(`{{answer|hint}}` 挖空標記)、classification drill
- 內建答案保真規則:手寫答案標記為準,但明顯錯誤的標記不會照搬

## 📁 結構|Structure

```
exam-review-system/
├── SKILL.md                        # 觸發描述 + 完整工作流程
├── references/
│   └── page-patterns.md            # 頁面結構目錄、設計規範、答案保真規則
└── assets/
    └── quiz-engine-template.html   # 可直接改資料即用的互動引擎模板
```

## License

[MIT](LICENSE)
