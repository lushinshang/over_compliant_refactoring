# 再重構一下就好？AI 寫程式代理人為什麼停不下手（論文深度導讀）

《Your Wish Is My Commit: Over-Compliant Refactoring in AI Coding Agents》（Jiang、Sabouri、Haghighi、Chattopadhyay；University of Southern California 與 Open Science Lab；arXiv:2610.36346v1，2026-10-26）的臺灣繁體中文深度導讀。論文用 60 份刻意乾淨的 Java 程式碼庫、2 種 coding agent、5 個模型、每次連續 10 輪（共 3,000 輪）的重構實驗，定義 S1 至 S6 六個信號，量化 agent 在程式碼已經乾淨之後仍然持續修改的程度。

## 製作聲明

本導讀的分析與撰稿由 AI 模型完成，不是人類分析，也沒有經過人工逐句校稿。模型分工見 `research/workflow-log.md` 與文末〈製作說明〉：主控與撰稿為 Claude Sonnet 5.5；論文全文初步整理由 agy（gemini-3.8-flash）提供線索，所有數字回論文全文核對，不一致處以全文為準；審稿由獨立 AI 審稿者分事實、可讀性兩路進行；最後再由一位 Sonnet（Claude Sonnet 5.5）依總編輯準則潤稿並標示重點（粗體與「重點」引言）。資料圖以腳本產生 SVG，未使用生成式影像。

## 檔案清單

| 檔案 | 說明 |
|---|---|
| `over_compliant_refactoring.md` | 正式 Markdown 導讀 |
| `index.html` | 單檔 HTML（`build_guide_html.py --kind guide` 產生，可直接用瀏覽器開啟） |
| `images/fig-s1-churn-zh.svg`、`fig-s1-churn-zh-mobile.svg` | 圖：各模型停手前累計改動量（桌面與手機版） |
| `images/fig-overview-zh.svg`（及 `-mobile`） | 圖：全篇一頁地圖（三十秒版之後） |
| `images/fig-design-zh.svg`（及 `-mobile`） | 圖：實驗設計的每輪迴圈 |
| `images/fig-signals-zh.svg`（及 `-mobile`） | 圖：S1／S2／S3 時間軸示意（非實際資料） |
| `images/fig-tstar-zh.svg`（及 `-mobile`） | 圖：各模型第一次停手輪次 t* |
| `images/fig-relapse-zh.svg`（及 `-mobile`） | 圖：停手後改動量（S2）與說不用改仍動手比例（S3） |
| `images/fig-inflation-zh.svg`（及 `-mobile`） | 圖：新增與刪除的結構節點 |
| `images/fig-rq-map-zh.svg`（及 `-mobile`） | 圖：四個研究問題對應的訊號與章節（第二節 RQ1 至 RQ4 之後；腳本 `research/make_fig_rq.py`） |
| `research/` | requirements、workflow-log、source-ledger、lens 備忘、figure_plan（含逐節要不要圖的判斷）、SVG 產生腳本（make_fig_s1.py、make_figs_all.py）、論文 PDF 與全文文字檔、審稿紀錄 |
| `research/pre_editor_pass.md`、`research/editor_pass.md` | 總編輯潤稿前的備份，以及潤稿前後對照、放棄修改處與驗證輸出 |
| `qa/` | 版面驗收截圖與結果 |

## 原始資料

- 類型：論文（arXiv 預印本）。網址：https://arxiv.org/abs/2610.36346
- 本機：`research/pdf/2610.36346.pdf`、`research/txt/2610.36346.txt`（pdftotext 全文）。不適用逐字稿。

## 重要來源

論文本身是唯一的第一手來源；導讀引用的 Ambig-SWE、CODETASTE、Compliance Paradox、Wei 等人的緩解研究，只轉述論文對它們的描述，連結見文末來源清單。

## 同系列

文末連到 `../thinking_in_bets_sycophancy/index.html` 與 `../continue_or_restart_agents/index.html`；另在正文兩處加了各一句回指：第二節連到 thinking_in_bets_sycophancy（引該文〈兩種錯，缺一不可〉一節），第十一節連到 continue_or_restart_agents（引該文〈合併 AUC 與同題內 AUC〉一節）。這是本導讀系列的串接，不代表三篇論文互相引用，結果也不能互相套用。

## 狀態

已發布（2026-11-03）：https://lushinshang.github.io/over_compliant_refactoring/

## 綜合研判
三篇導讀共用同一節〈綜合研判（本文推論）〉，三篇的文字逐字相同，由本機工作庫的來源檔同步寫入。這一節是本導讀系列的跨篇歸納，不是任何一篇論文的主張。
