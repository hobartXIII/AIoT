# L01 課程介紹（2026.9.8）

來源：AIoT-DA L01（Notion，Chen Huan 課程頁）

## 評分比重

| 項目 | 配分 |
|---|---|
| HW | 10 × 3 |
| 點名 | 5 |
| Quiz | 8 × 2 |
| DIC | 6 × 4 |
| Final project | 10 + 15 |

## Part 1 — 趨勢分析：四大革命 + ANI / AGI / ASI

三層架構，兩條軸不要混在一起：

**Level 1｜文明史：人類能力的逐步解放**

| 革命 | 核心突破 | 解放／擴張 |
|---|---|---|
| ① 工業革命 | 機械化 | 體力 |
| ② 能源革命 | 電力、石油 | 動力 |
| ③ 資訊革命 | 電腦、網路、數位化 | 資訊能力 |
| ④ AI 革命 | 機器智慧 | 認知、創造與決策 |

**Level 2｜AI 智慧層級**
- **ANI 狹義 AI**：特定任務／領域（人臉辨識、推薦系統、ChatGPT、Coding Agent）。現在的主體。
- **AGI 通用 AI**：跨領域理解 → 學習 → 推理 → 規劃 → 遷移知識 → 用工具 → 解決陌生問題。
- **ASI 超人工智慧**：幾乎所有認知領域都超越最優秀的人類。

**Level 3｜ANI 內部的範式演進**
1. **Descriptive / Analytical AI** — 幫我「判斷」（分類、預測、異常偵測）
2. **Generative AI** — 幫我「創造」（文字、圖片、影片、程式碼）
3. **Agentic AI** — 幫我「完成任務」：Goal → Plan → Use Tools → Execute → Observe → Revise → Complete

> 結論：AI 的遠景是 ANI → AGI → ASI；我們正在經歷的是 ANI 內部「分析式 → 生成式 → 代理型」的快速範式轉移。

### 圖片補充

**四大革命對照表**（投影片 Part 2.1，加上 NotebookLM 簡報）

| | 工業革命 | 能源革命 | 資訊革命 | AI 革命 |
|---|---|---|---|---|
| 核心技術 | 蒸汽機、機械化 | 電力、內燃機 | 電腦、網際網路、雲端 | 機器學習、深度學習、NLP、電腦視覺 |
| 解放維度 | 勞動力（Labor） | 物理空間（Space & Mobility） | 知識與溝通（Knowledge） | 認知與創造（Cognition） |
| 社會影響 | 工廠制度、都市化、專業分工 | 全球化與交通 | 知識民主化、全球協作 | 降低創作門檻、決策輔助與自動化 |

- 核心命題：人類進步的本質，是系統性地移除物理與認知上的限制。
- 前三次革命突破的是「物理」限制（肌肉的延伸 → 距離的壓縮 → 數據的傳輸）；AI 革命是第一次針對「認知與創造」邊界的解放（思維的擴增）。

**AI 發展史**
- 1940–1950s 萌芽：Turing test、1956 Dartmouth 會議正式提出「人工智慧」。
- 1970–1980s AI 寒冬：運算力與資料不足，期望過高導致經費撤出。
- 2000s 至今深度學習爆發：Big Data + GPU → 資料驅動典範（Backpropagation）→ 開源框架（TensorFlow 等）→ 影像、語音、語言生成的商業化。

**ANI / AGI / ASI 各階段重點**
- **ANI**：資料驅動、高度專精、缺乏通用理解；限制是無法跨領域遷移知識。
- **AGI**：真正的泛化、常識推理、自主學習；工程挑戰是多模態學習、情感理解、倫理約束。
- **ASI**：可能出現「智慧爆炸」（遞迴自我改進）；最大課題是 **alignment（對齊）**，確保比人類更聰明的系統仍安全可控。

**ANI 三個 Stage 的 AIoT 範例**

| Stage | 角色 | AIoT 範例 | 應用 |
|---|---|---|---|
| 1. Descriptive / Analytical AI | 判斷、預測 | 智慧環境監測：用感測器資料預測未來 24 小時 PM2.5、做異常偵測 | 空品預測、設備異常偵測、能源用量預測、影像辨識 |
| 2. Generative AI | 創造內容 | 智慧農業報告：土壤濕度、溫度、光照、作物影像 → AI 生成分析報告與建議 | 生成分析報告、數據圖表、監控摘要、程式碼 |
| 3. Agentic AI | 完成任務 | 智慧城市事件處理：發現河川水位異常 → Agent 自主規劃：查詢即時 API、分析風險、發送 Line/Email 通知、控制閘門、持續監控 | 自動資料收集、設備控制、異常處理通報、長期監控 |

## Part 2 — Prompt 提示詞怎麼寫

- **二階段 prompt**：先粗略寫出需求，再請 GPT 優化成結構化 prompt。
  `give me best prompt to do <your simple prompt>`
- 範例：`give me best prompt to write sklearn to solve iris classification problem, following crisp-dm method`
- **Grill me before implement**：實作前先讓 AI 反問、拷問你的需求。
- 好習慣：`summarize your work in 資訊圖表`
- 寫作框架其實就是 **5W2H**。
- 其他技巧：加描述詞「一步一步」、要求自我檢核、激將法（「我賭你不行」）、AI 委員會（assume 你是專家）、語音輸入交代任務。

### 圖片補充：12 大提示策略全景圖（由簡到進階）

| # | 策略 | 核心概念 | 範例 / 適用場景 |
|---|---|---|---|
| 1 | 直接指令 Direct Instruction | 直接告訴模型要做什麼 | 「總結這篇文章重點的 3 個要點」；簡單任務 |
| 2 | 角色設定 Role Prompting | 賦予模型特定角色 | 「你是一位資深資料科學家…」；專業領域問答 |
| 3 | 範例學習 Few-Shot | 給範例讓模型學模式 | A→B、C→D、E→?；格式模仿、分類標註 |
| 4 | 結構化輸出 Structured Output | 指定 JSON / 表格格式 | 自動化處理、程式整合 |
| 5 | 限制條件 Constraint-Based | 加入明確限制 | 「5 個要點、每點不超過 20 字」 |
| 6 | 任務拆解 Instruction Decomposition | 複雜任務拆成多步 | 步驟 1 分析問題 → 2 提取資訊 → 3 生成答案 |
| 7 | 思維鏈 Chain-of-Thought | 要求逐步推理 | 「請一步一步思考，先說明推理過程」；數學、邏輯 |
| 8 | 一致性檢驗 Self-Consistency | 多條推理路徑取最一致答案 | 「用 3 種不同方法解題，選最可靠的答案」 |
| 9 | 反思優化 Reflection / Critique Loop | 生成 → 批判 → 改進迭代 | 高品質寫作、研究報告 |
| 10 | 樹狀思考 Tree-of-Thought | 探索多種解法並評估 | 策略規劃、決策分析 |
| 11 | 推理 + 行動 ReAct | Thought → Action → Observation 循環，交替推理與工具使用 | 工具調用、查 API、智能代理 |
| 12 | 提示系統架構 Prompt System Architecture | 整合角色、拆解、思維鏈、限制、結構化輸出等，形成系統化流程 | 複雜工作流、大型專案 |

- 進階路線：明確目標 → 選擇策略 → 組合應用 → 測試迭代 → 建立系統。
- 核心洞見：從「使用 AI」到「設計 AI 系統」，關鍵在策略的選擇、組合與持續優化。

**漫畫「Prompt 不會寫？用 GPT 寫 prompt！」**：大家都在搞 CoT、ToT、Agent、Pipeline，把 prompt 寫得像論文；其實用二階優化就好 — 第一階段你寫粗略需求，第二階段請 GPT 優化成含角色、任務、輸入、輸出、限制、步驟、範例的最佳 prompt。
- 心法：「常常用 AI 笑著笑著就哭了（花拳繡腿）；先練好基本功，哭著哭著就笑了」— 基本功 = 概念 × 理論 × 軟體工程 × Domain Know-How。

## Part 3 — 課程專題說明：智慧物聯網資料分析與應用代理人

期末目標：**Build Your AI Professional** — 打造一位能替人解決問題的 AI 專業代理人。
主線：真實問題 → 人機互動 → AI Agent → 資料與工具 → 分析 → 呈現 → 領域應用。

1. **使用者與情境**：先定義我的 Agent 是誰、服務誰、解決什麼真實問題。
2. **互動介面**：自然語言、語音、行動裝置、Web App、AR/VR。
3. **Domain Data Agent 工作流程**：Understand → Plan → Acquire → Analyze → Generate → Interact
4. **六大能力**（可作為評分指標）：TALK × KNOW × SENSE × ANALYZE × THINK × SHOW
5. **資料來源與工具**：IoT（ESP32、MQTT…）、內部資料（SQL、CSV、ERP）、外部資料（Open Data、天氣、API）、知識庫（RAG、Vector DB）、分析工具（Python、ML、Time Series…）
6. **分析引擎**：LLM 負責理解、推理、解釋；Analytics Engine 負責計算、驗證、提供證據。避免只做出換皮 ChatGPT。
7. **輸出呈現**：Chat UI、Dashboard、圖表、地圖、報告、3D、警示。
8. **領域應用範例**：AI Plant Doctor、Energy Detective、Warehouse Manager、Museum Curator、Factory Doctor 等。
9. **實作難度分級**：
   - Level 1：LLM + Data（對話 + CSV/DB/API + Python 分析 + 圖表）
   - Level 2：Knowledge Agent（+ RAG、Web Search、多步推理）
   - Level 3：IoT Autonomous Agent（+ 即時感測資料、MQTT、主動警示）
10. **評量重點**：解決真實問題、資料來源多元、分析有深度、引用專業知識、介面有創意、對使用者有價值。

> From Data to Insight, From Insight to Action.

（原頁面附一張完整架構圖「智慧物聯網資料分析與應用代理人」，內容跟上面 ①–⑩ 一致。）

## Part 4 — 學生總整理範例

- CRISP-DM 學習筆記：https://jamessun0919-ops.github.io/Machine-Learning-Study/chapters/crisp-dm/
