# exam-review-system

**ZCode skill** — 把你自己的考試材料(講義照片、試卷、課本頁、考試範圍)變成**一個互動式、離線可用的 HTML 復習系統**。
**A ZCode skill that turns your own exam materials (photos of worksheets, textbook pages, exam outlines, PDFs) into ONE interactive, offline HTML review system.**

> 實戰出身:由一次真實的考前復習流程提煉而成——讀完 21 頁材料照片、轉錄手寫答案、生成教學+速記+61 個挖空+83 題練習的深色復習網頁,部署上線並完成桌面/手機雙尺寸實測。
> *Battle-tested: distilled from a real exam-eve session (21 pages of material photos, handwritten answer keys transcribed, 61 cloze blanks + 83 practice items generated, deployed, and verified at desktop & mobile widths).*

## ✨ 產出什麼|What you get

一個雙擊即開、無需網絡的單檔 HTML 頁面(支援手機/深色模式/列印):

| 區塊|Section | 內容 |
|---|---|---|
| 🏠 首頁樞紐 Home hub | 8 張功能卡片,圖標+名稱+一句說明,幾秒內找到要復習的內容 |
| 📖 教學 Teaching | 預設你什麼都不會,原文+白話分層展示,重點句高亮 |
| ⚡ 速記卡 Flip-card deck | 可翻面的卡片疊:上一張/下一張/進度條,紅字易錯點,支持一覽全部 |
| 📝 挖空練習 Typed cloze | 考試原文挖空打字默寫,每空有中文提示、即時批改,**簡體/繁體輸入都判對** |
| ✅ 判斷快閃 True/False flash | 一次一題的閃卡,答後立即顯示判定與解析 |
| ❓ 選擇題 MCQ | 分組展示,選項全寬易點擊,即時判定+講解+計分 |
| ✍️ 作文/策略 Strategy | 分值結構表、時間分配、答題模板、素材卡、考試日清單 |
| 🚀 部署 Deploy | 交付後主動詢問是否部署到 ZCode Sites 生成分享連結 |

兩套可選視覺:`assets/quiz-engine-template.html`(分頁式,自動適配淺/深色)、`assets/dark-hub-template.html`(深色優先,首頁樞紐+翻面卡,現代生產力軟件質感)。

## 📦 安裝|Install

把整個資料夾放進 ZCode 的技能目錄:

```bash
git clone https://github.com/Dinis0404/exam-review-system.git ~/.zcode/skills/exam-review-system
# 或跨工具通用位置:
# git clone https://github.com/Dinis0404/exam-review-system.git ~/.agents/skills/exam-review-system
```

重啟 ZCode 後即可用。**Restart ZCode, then it triggers automatically.**

## ⬆️ 更新|Update

已安裝的用戶,進入技能資料夾拉取最新版:

```bash
cd ~/.zcode/skills/exam-review-system   # 或你安裝時用的目錄
git pull
```

然後重啟 ZCode 生效。**Pull the latest changes and restart ZCode.**

## 🚀 用法|Usage

直接丟材料+一句話,例如:

- 「這個資料夾是我明天的考試範圍,幫我做一個復習的網頁」
- 「Here are photos of my chemistry notes and past papers — make me a review page」
- 「單獨出一個關於倒裝句的詳細教學+練習」(單主題深挖)
- 「幫這個復習頁做一次 UI 重新設計,深色現代風格」(保留全部內容,只換界面)
- 做完後:「部署到 zcode」(主動詢問+配合 site-deploy skill 上線)

Skill 會:**預處理大圖/長截圖 → 讀完每一頁材料 → 轉錄手寫答案 → 標出「讀過=送分題」→ 生成網頁(教學→速記→練習→策略)→ 自動 QA(語法/元素/題數+雙尺寸瀏覽器實測)→ 存到你的材料資料夾 → 詢問是否部署**。

## 🧩 技術|Tech notes

- 單檔、零依賴、UTF-8;深色優先或 `prefers-color-scheme` 自適應;320px 起響應式;列印時自動展開全部頁籤
- 資料驅動引擎:`QUIZZES`(MCQ+講解)、`CLOZES`(`{{answer|hint}}` 挖空標記)、翻面卡疊(DECK)、判斷閃卡(JUDGE_Q)
- 中文答案繁簡自動歸一(S2T 映射),兩種輸入習慣都判對
- 內建答案保真規則:手寫答案標記為準,但明顯錯誤的標記不會照搬
- 重新生成/重新設計時強制內容保真:題數/空數/卡數必須與上一版一致

## 📁 結構|Structure

```
exam-review-system/
├── SKILL.md                        # 觸發描述 + 完整工作流程
├── references/
│   └── page-patterns.md            # 頁面結構目錄、設計規範、答案保真規則
└── assets/
    ├── quiz-engine-template.html   # 分頁式引擎模板(淺/深自動適配)
    └── dark-hub-template.html      # 深色優先樞紐引擎模板(首頁卡片+翻面卡+閃卡)
```

## 📝 更新日誌|Changelog

完整更新日誌見 [**Releases**](https://github.com/Dinis0404/exam-review-system/releases) —— 每個版本有獨立發佈說明,Watch 倉庫即可收到新版本通知。

- **v1.2** — 科目優先分流(步驟0 先認科目 + `references/subject-playbooks.md` 科目手冊,語文完整實戰);dark-hub 模板新增「背誦助手」引擎(分組首字背/三步閉環/進度保存/全局顯示隱藏)
- **v1.1** — 深色樞紐模板、交付後詢問部署、實戰 QA(內容保真核對+雙尺寸實測)、大圖預處理
- **v1.0** — 初版:分頁式引擎、教學→速記→練習→策略工作流、答案保真規則

## License

[MIT](LICENSE)
