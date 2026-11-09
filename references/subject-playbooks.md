# Subject playbooks

SKILL.md step 0 identifies the subject; this file holds the per-subject playbook. Read only the section you need. Every playbook sits on top of the shared workflow (read everything → map scope → build → QA → deliver → ask about deploy) and only swaps the middle.

## 語文 (Chinese) — proven end-to-end

### What the exam looks like

- **默寫** is the core: 理解性默寫 (comprehension prompt → quote, e.g. 「寫知道滿足的人富有…」→「知足者富」) and 上下句默寫 (given 上句 write 下句; often 連默超過100字 = whole-poem recall). These are guaranteed marks — highest priority.
- 文學常識選擇 (作家作品/戲劇/詩歌常識, often as 判斷題✓✕ practice), 字音字形/成語/語病, 文言文閱讀 (課外), 名著閱讀 (按口語考試題出題!), 小作文.
- Typical material mix: 範圍單照片 (sections + 點數 + 篇目), 默寫練習 DOCX(附答案), 文學常識判斷題 DOCX, 口語考試題 DOCX, 課本/練習冊照片 (紅筆答案 = 答案卷), 網上題庫的長截圖.

### Tabs that work (dark-hub template order)

背誦教學 (原文+白話分層, 詩用襯線體, 重點句高亮) → 背誦助手 → ⚡速記卡 (翻面卡) → 理解性默寫 → 上下句默寫 → 判斷快閃 → 選擇題庫 (分組: 作家/戲劇/詩歌/語基/課文) → 名著閱讀 (答題框架) → 考場策略 (分值表+時間分配+詞解四法+作文公式+素材卡).

### 背誦助手 (memorization helper) — highest-value language tab

Data shape (`MEM` array, engine built into dark-hub template):

```js
{ id, tab /* 對應打字實測頁 */, name, how /* 分組思路一句話 */,
  chunks: [ { label /* 畫面或情緒 */, cue /* 首字串 */, rhy /* 韻腳, 詩歌必加 */,
    lines: [ { t /*原文*/, f /*首字*/, e /*白話/考點,可選*/ } ] } ] }
```

- Chunk the text: 詩 = 每4句一組 (韻腳每4句一換, 跟韻走不串段); 詞 = 按上下片/情感線; 文言 = 一段一組.
- 三步閉環 per chunk: ①讀 (原文+白話) → ②首字背 (原文蓋斜紋條, 點擊揭曉對答案) → ③硬背 (白話也藏). 背完「標記已背」→ localStorage 進度 (try/catch 包裹), 頂部總進度條 + 每篇計數.
- 全局「顯示/隱藏全部」按鈕作用於當前篇目所有分組; 「①讀」模式分組不受全局隱藏影響; 按鈕標籤隨任何來源的變化自動同步.
- 每篇頭部放「去打字實測 →」直達對應默寫頁 — 背完立刻用考試形式檢驗 (檢索練習 > 重讀).
- 教學頁/助手裡寫明故事線: 詩歌用「鏡頭移動」(如春江花月夜: 月升起→中天→西斜→落下), 韻文用情感線 (如將進酒: 悲→歡→勸→憤→狂→愁).

### Language specifics

- **繁簡輸入歸一**: cloze 答案比對前過一張 S2T 字符映射 (dark-hub 引擎內建) — 學生繁簡混打都判對.
- 理解性默寫題幹**照抄老師的練習** (考點一一對應), 答案以 DOCX 附答案版為準.
- 名著閱讀: 口語考試題 = 題庫. 逐題給「觀點→引原文→分析→聯繫自身/當代」框架 + 填空式示例, 字數要求標在徽章上. 每張框架卡頂部放可摺疊的「原題」塊 (`<details class="origq">`: 材料+語錄原文+小題+字數要求高亮) — 學生先看原題再對照框架背.
- 練習冊原題頁做成**可作答題組**(復用 CFG 引擎: 計分+重做), 主觀題參考答案默認摺疊(先寫再點開)——學生能做, 不只是看.
- 文言文閱讀是課外題: 不猜原文, 只做策略頁 — 詞解四法 (組詞/課內遷移/字形/代入) + 課內實詞遷移表 + 翻譯六字訣 (留刪補換調貫).
- 小作文: 字數公式 (開頭30+3×75+結尾35) + 萬能素材卡 — 素材從材料裡提煉並標明出自哪篇/哪本.
- 判斷題✓✕合集轉成「判斷快閃」(一次一題+解析) 和「紅燈陷阱速記卡」——✕項就是出題者的坑.

## STEM (Math/Physics/Chem) — skeleton, adapt freely

- Tabs: 教學 (worked examples, 一步一步) → ⚡公式速記卡 → 公式默寫 (給公式名寫公式) → 題型練習 (逐步解答 + 「辨陷阱」MCQ) → 考場策略 (單位換算、答題步驟、常用數值表).
- 背誦助手改為「公式推導卡」: cue = 公式名, lines = 推導步驟 (f = 每步首詞).
- 高頻失分: 單位、符號、定義域 — 做成紅燈卡.

## Humanities/History — skeleton, adapt freely

- Tabs: 教學 (時間線 + 因果鏈) → ⚡速記卡 → 時間排序練習 → 因果配對 → 術語閃卡 → 史料分析 MCQ → 考場策略 (材料題答題公式: 觀點+引料+分析).
- 背誦助手: cue = 事件名, lines = 年份/影響; 分組 = 時代.

## English (foreign language) — skeleton, adapt freely

- Tabs: 教學 (語法點對比表 + 例句) → ⚡速記卡 (不規則動詞/搭配/連接詞) → 填空 cloze → 閱讀理解 MCQ → 寫作範本 (句型庫 + 範文框架 + 檢查清單) → 考場策略.
- 背誦助手用於範文段落背誦: cue = 每句首詞, 分組 = 段落功能 (開頭/論點/結尾).
