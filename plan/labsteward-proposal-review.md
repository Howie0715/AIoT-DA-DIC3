# LabSteward 提案評析報告

> 領域智慧代理人期末專題提案評析報告：LabSteward 實驗室財產管理 AI Agent
> 期末專題提案內容深度解析與合規評審報告

| 項目 | 內容 |
| --- | --- |
| 評析標準依據 | 《期末專題提案作業規範》、2020 A2Z Outline Example SOP |
| 受評專題名稱 | LabSteward 實驗室財產管理 AI Agent (Final Project Proposal) |
| 核心評量原則 | AI Agent + Domain Knowledge + Data Analytics + Real-World Application |
| 評析報告日期 | 2026 年 11 月 11 日 |

> **【核心審查結論】**
> LabSteward 提案嚴格遵循《期末專題提案作業規範》中「拒絕單純 Chatbot 或單純 Model 訓練」之指導原則，將專題錨定於具備「意圖理解、工具調用、法規推理與狀態可追溯」的 Domain AI Agent 架構。文稿在結構上高度契合 2020 A2Z Outline SOP，從 Abstract 之 A–F 故事鏈、緒論 G–L、相關研究 M–O 綜整表，到系統設計 PA–PM 具體落實了架構圖、流程圖與虛擬碼，是一份極具專業度與實作可行性的標竿提案。

## 一、專題核心定位檢視：四大支柱合規分析

| 維度指標 | 規範核心要求 | LabSteward 具體實作對應 | 合規評價 |
| --- | --- | --- | --- |
| AI Agent（智慧代理） | 具備意圖理解、自主調用工具、反思與多步驟任務拆解能力，非單向 Prompt 應答。 | 採用 ReAct 式推理與 Tool Calling；LLM 專職 Planner 負責解析與追問；運算與判斷交由確定性工具執行。 | **優秀**：避免單一 LLM 幻覺稅 |
| Domain Knowledge（領域知識） | 深入產業或行政專業，內建專屬法規、門檻規則與專有名詞，非通用問答。 | 整合《財物標準分類》總說明、大專校院財物管理辦法、政府採購法門檻（15 萬 / 150 萬）與報廢分級核定，建立 RAG 與 YAML 規則檔。 | **高度契合**：法規條文可精準追溯 |
| Data Analytics（資料分析） | 具備客觀數據分析、數值預測、統計或最佳化模型，有量化指標支撐決策。 | 建立雙軌殘值預估模型（直線法定折舊 + log-linear / Gradient Boosting 二手市場折舊）、商品規格 Entity Matching 與 TCO 總持有成本排序。 | **具體量化**：雙軌區間估計具創新性 |
| Real-World Application（真實應用） | 鎖定真實世界痛點，具備具體使用者場景、RBAC 權限與行政審核機制。 | 直接解決大學實驗室長期存在的「蚊子儀器」、重複購置、閒置無移轉管道與報廢退件痛點，產出具法律效力之送審公文草稿。 | **實用性高**：人機協同審核明確 |

## 二、A2Z SOP 架構落實深度剖析

### 1. 摘要 Abstract（A2Z 之 A～F 完整故事鏈）

規範要求摘要必須形成引人入勝且具備客觀數據的故事線，LabSteward 嚴格落實六段式展開：

| 段落要素 | A2Z 規範定義 | LabSteward 內容具體體現 |
| --- | --- | --- |
| A. Attention Getter | 以客觀數據與強烈動機引題，說明問題迫切性。 | 引述監察院調查（國立大學貴儀多台使用率為零或低於 60% 的蚊子儀器）、韓國 2026 年全國研究設備共享普查，以及 Deloitte 2026 報告（92% 採購長評估 Agentic AI）。 |
| B. But | 點出現有技術盲點、瓶頸與 Research Gap。 | 指出現有校內系統只登帳不決策、通用 LLM 端到端庫存決策多出 32% 成本（幻覺稅）、τ-bench 工具穩定度低於 25%，且現有電商 Agent 不懂採購法門檻。 |
| C. Cure | 提出明確的新穎解藥與系統名稱。 | 提出 LabSteward：結合財產法規知識與工具調用的實驗室財產管理與採購 AI Agent，涵蓋「管理」與「採購」雙模組。 |
| D. Development | 簡述系統如何結合 AI、資料與領域知識。 | 以 LLM Planner 協調 SQLite 財產庫、雙軌殘值預估模型、商品 Entity Matching、法規 RAG 向量庫與 YAML 規則引擎（Guardrail），輔以人工核准。 |
| E. Experiments | 說明資料集、評估基準與驗證指標。 | 規劃 50 個管理與採購情境任務，比較三種 Baseline（人工 Excel、單一 LLM、純 RAG），評估 pass^k 穩定度、殘值 MAPE、法規違規率等。 |
| F. Expected Outcomes | 預期效益條列（未完成實驗不得虛構結果）。 | 遵守規範誠實性要求：預期降低法規違規率、落實先查庫存節省開支、提供雙軌殘值區間、自動化產出可直接送審之公文草稿。 |

### 2. 第一章 緒論 Introduction（A2Z 之 G～L 展開）

緒論完整回答了「Why is this project worth doing?」的核心提問：

- **1.1 Attention Getter（G. General & H. However）**：以 2013 監察院報告（國科會年均補助 2.2 億但使用率偏低）、2026 韓國 ZEUS 政策（18 億韓元儀器因缺維修閒置）與 Gartner / Hackett 調查，論證「需求殷切但落地困難」。
- **1.2 Background**：定義財產標準（1 萬元以上且使用年限 2 年以上為財產）、採購金額分級（15 萬小額、150 萬公告金額）與三類核心使用者角色（實驗室成員、財產管理人、PI 指導教授）。
- **1.3 Problem**：明確定義四大痛點——資訊斷裂、價值不透明、先買再說（缺乏庫存優先檢核）、法規門檻高常被退件。
- **1.4 Existing Limitations（I. In literature & J. Judgement）**：評析校內系統、商用 ERP（SAP 等缺乏台灣採購法支援且昂貴）、通用 LLM 幻覺、網購 Agent 易錯與傳統殘值研究缺乏儀器資料。
- **1.5 Innovation & 1.6 Contributions（K. Keypoint & L. List organization）**：強調「LLM 只做協調不做判斷」、「先共享再採購演算法化」、「雙軌殘值與區間估計」與「法規條文與稽核紀錄可追溯」。

### 3. 第二章 相關研究 Related Work（A2Z 之 M～O 展開）

提案嚴格維持 1/2 AI Technology + 1/2 Domain Know-how 的均衡結構：

- **AI 領域（2.1 節）**：深入探討 ReAct 工具調用機制、τ-bench 規則遵守穩定度（pass^k 指標）、Duan et al. (2025) LLM + 最佳化求解器降低 32.1% 成本之混合架構、Agentic RAG 混合檢索，以及 DeepShop 與 LLM Entity Matching 之最新進展。
- **領域知識（2.2 節）**：深入掌握財物標準分類與最低使用年限、報廢分級核定、國立大學請購作業與共同供應契約、動產年齡與折舊率（筆電年貶 14.4%、手機 28.2%）、GPU 折舊爭論（雲端 6 年 vs. 實質 2–3 年）以及貴重儀器共享機制。
- **文獻彙總表（表 2-1）**：嚴格遵守規範要求的 8 欄格式，涵蓋 8 篇 2024–2026 年高品質文獻。
- **Research Gap 歸納（2.4 節）**：以 Existing Research → Limitations → Research Gap → Our Project 清晰定位出「缺乏一個懂台灣公立學校財物與採購法規、能先查庫存再比價、能估殘值且具備稽核紀錄的實驗室級 Agent」。

## 三、Deep Research 深度研究實質檢視

### Q1. 五大核心思維模式（Five Mindsets）

| 核心思維 | 領域意涵 | 客觀法規與政策依據 | 系統架構實作設計 |
| --- | --- | --- | --- |
| 帳實相符 (Record = Reality) | 帳上每件財產均須找得到、對得上保管人與存放位置。 | 各校財物管理辦法明定保管人責任、定期盤點與減損程序。 | 所有變更必須經工具執行並寫入 Audit Log；支援 QR 碼盤點比對。 |
| 全生命週期 (Lifecycle) | 財產從取得、使用、維護、移轉到報廢變賣是一條完整鏈條。 | 各機關奉准報廢財產之變賣及估價作業程序與減損規範。 | 建立五階段狀態機（在庫 → 借出 → 維修 → 待報廢 → 已報廢），非法狀態禁止轉移。 |
| 先共享，再採購 (Utilize before Acquire) | 採購前必須先確認既有或閒置資源能否滿足需求，杜絕浪費。 | 監察院貴重儀器調查報告；韓國 2026 年轉向全國設備共享制度。 | 採購模組第一步強制執行 `query_inventory()`，有可用設備即阻擋重複採購。 |
| 合規與分級授權 (Compliance by Threshold) | 相同採購或報廢事項因金額與年限不同，流程與簽核層級截然不同。 | 財產 1 萬 / 2 年定義；小額採購 15 萬、公告金額 150 萬；報廢分級核定。 | 金額與年限門檻抽離為獨立 YAML 規則檔，由規則引擎判定而非 LLM 自由心證。 |
| 帳面 ≠ 經濟價值 (Book vs. Economic) | 法定年限是會計與行政管理工具，不代表設備真實經濟價值或可用性。 | 主計總處財物標準分類規定（到期仍可用應續用）；GPU 折舊年限爭議。 | 殘值模組同時計算直線法定殘值與二手市場迴歸殘值，提供維修 / 報廢決策依據。 |

### Q2. 三大專業爭論（Three Controversies）

本專題落實「觀點 A vs. 觀點 B → Evidence → Consensus → Unresolved」之分析框架：

1. **折舊年限【法定會計年限 vs. 實際經濟壽命】**：會計規範追求一致可稽核，但 AI / 運算設備 12–18 個月即面臨技術迭代。共識為兩者皆須揭露；未解之處在於稀有實驗儀器缺乏二手成交數據。
2. **設備歸屬【集中共享 vs. 實驗室自有】**：集中共享可提升使用率，但若缺乏維修經費與專職人力反淪為高價閒置；共識為高價低頻設備共享、高頻常規設備自有；未解之處在於使用費率定價與跨校移轉機制。
3. **AI 自主權【全自動 Agentic vs. Human-in-the-loop】**：全自動可大幅提高採購效率，但 τ-bench 等研究證實 LLM 在多工具長鏈任務中存在嚴重不穩定性與責任歸屬爭議。共識為 LLM 負責協調與自然語言互動，高風險法規與帳務操作一律經人工確認。

### Q3. 十大深度理解問題（Ten Deep Questions）

題目涵蓋 Why（金額與年限雙門檻必要性）、What-if（超年限仍可用、拆單請購偵測）、Trade-off（跨門檻流程成本、借用管制嚴格度 vs. 登記意願）、Failure Case（異常低價 40% 的比價防詐）、Evidence（申購新主機之使用率佐證資料）、Comparison（直線折舊 vs. 指數折舊適用情境）與 Design Decision（規則引擎 vs. LLM 法規推理）。題目設計深刻反映出真實領域的邊界條件與行政邏輯。

## 四、系統設計與技術落實評析（Proposed Scheme）

### 1. 系統架構（System Architecture：①～⑧ 模組結構）

| 模組編號與名稱 | 職能職責與技術選型 | 解決之關鍵痛點 |
| --- | --- | --- |
| ① User Scenario 使用者情境與權限 | 定義實驗室成員、財產管理人與 PI 指導教授三類角色，落實 RBAC 權限控管。 | 防止學生未經授權報廢財產或擅自核准採購。 |
| ② Query / Input 多模態輸入與介面 | 支援自然語言文字、發票 / 報價單 PDF 與財產標籤拍照，以 Streamlit Web 實作。 | 降低學生與行政人員登帳與查詢的操作門檻。 |
| ③ Planner Agent 核心規劃代理人 | 採用 ReAct 思考模式與 Tool Calling，負責意圖解析、參數追問與任務分派。 | 將模糊的自然語言需求轉化為結構化之執行參數。 |
| M1–M4 管理模組 / P1–P4 採購模組 | 管理子 Agent 負責導入、借用、殘值與報廢；採購子 Agent 負責需求確認、庫存查詢、比價與清單產生。 | 專能分工，將管理與採購雙模組打通，杜絕資訊斷裂。 |
| ④ Data Acquisition 資料取得與檢索 | 整合 SQLite 本地資料庫、財物標準分類表、Playwright 動態網頁爬蟲與市場掛牌價。 | 即時獲取官方標準與真實電商 / 二手市場動態數據。 |
| ⑤ Analytics Engine 資料分析引擎 | 執行設備使用率分析、殘值迴歸（log-linear / LightGBM）、商品 Entity Matching 與 TCO 排序。 | 將運算從 LLM 剝離，交由機器學習與統計模型提供確定性結果。 |
| ⑥ Domain Knowledge 法規 RAG 與規則庫 | 法規條文向量資料庫（混合 BM25 + 向量檢索），金額與年限門檻抽離為 YAML 設定檔。 | 確保法規判定零幻覺，法規修訂時無須重新訓練模型。 |
| ⑦ Decision & Guardrail 決策建議與安全防護 | 執行預算、門檻、拆單違規檢核；計算不確定度；高風險操作強制導入 Human-in-the-loop。 | 建立負責任的 AI 防護網，防止違法採購或惡意報廢。 |
| ⑧ Output & Interaction 文件產出與稽核 | 自動生成符合校方規範之 Word 採購 / 報廢草稿、Excel 清冊，並留下完整 Audit Log。 | 大幅節省行政文書時間，確保行政決策具法律追溯性。 |

### 2. 數學模型與演算法深度（Mathematical Formulation & Pseudocode）

提案在技術深度上超越常規 Prompt 組合，具備嚴謹的數學與演算法表示：

- **殘值預估數學公式**
  - 法定帳面價值：採用直線折舊公式 `V_book(t) = C × max(0, 1 - t/L)`，精確對齊主計總處財物分類規範。
  - 市場經濟價值：小樣本時採 `V_mkt(t) = C × (1 - d)^t × q`（d 為貶值率，q 為物況係數）；資料充裕時採用 Gradient Boosting 配合 Bootstrap 產生 80% 預測區間。
- **三大演算法 Pseudocode（具備完整行號）**
  - Algorithm 1 (Main Loop)：實作參數追問迴圈（`missing(params)`）、RBAC 授權檢核、分派子 Agent、法規 RAG 檢索、Guardrail 違規攔截與人工覆核（`human_review`）。
  - Algorithm 2 (Dual-track Valuation)：實作雙軌殘值計算，並依使用狀態與維修成本產生 `KEEP_USING`、`DISPOSE` 或 `TRANSFER_OR_SHARE` 具體處分建議。
  - Algorithm 3 (Procurement Agent)：落實「強制庫存優先檢核（`Inventory.query`）」、網頁比價實體比對、TCO 排序與「拆單請購偵測（`split_order_suspected`）」。

### 3. 實驗設計與交付規範（Evaluation Plan & Final Handover）

- **實驗基準（Baselines）**：設置 B1 人工 Excel 流程、B2 單一 LLM 端到端（無工具）與 B3 純 RAG 問答，評估架構的實質效益。
- **多維度指標（Metrics）**：涵蓋 τ-bench pass^k (k=4) 穩定度、欄位抽取 F1、殘值 MAPE、比價最低價差距、Entity Matching 精確率、法規違規率與 SUS 使用者滿意度。
- **交付規格**：規劃了標準化的專案結構（含 `agent/`、`tools/`、`knowledge/`、`eval/`、`app/`），並嚴格規範以 DESIGN.md 記錄全系統設計，符合「即使原開發者不在，後續接手者亦能無縫驗證與部署」的最高交接標準。

## 五、評審綜合成績試算與優化建議

| 評分項目 | 配分 | 預估得分 | 指標落實評價與亮點 |
| --- | --- | --- | --- |
| Introduction（重要性、動機、證據、創新） | 20% | 19 / 20 | 數據引題有力（監察院貴儀報告、韓國政策、Deloitte 調查），四大痛點與創新論述清晰。 |
| Related Work（AI + Domain + 文獻綜整） | 20% | 19 / 20 | 嚴格維持 1/2 AI + 1/2 Domain 均衡；8 篇高水準文獻綜整表；明確定義 Research Gap。 |
| Deep Research（5 思維 + 3 爭論 + 10 問題） | 20% | 20 / 20 | 三大問題全面達標，推論層次深刻，爭論框架完整（觀點 A/B、Evidence、Consensus、Unresolved）。 |
| Proposed Scheme（AI Agent + 系統架構圖） | 20% | 19 / 20 | 八大模組架構完備，詳細定義管理與採購子 Agent 職能，落實確定性工具與 Guardrail 機制。 |
| Flowchart & Pseudocode（流程圖、演算法與可行性） | 15% | 15 / 15 | 流程圖包含例外分支、追問與人工審核；三大演算法含完整行號；技術選型成熟可行。 |
| GitHub & DESIGN.md（目錄架構與交接規範） | 5% | 5 / 5 | 目錄結構清晰模組化，明確規劃 DESIGN.md 演進藍圖與十週進度時程表。 |
| **總評成績 (Total)** | **100%** | **97 / 100** | **等級：特優 (A+)**。本案為期末專題提案之極佳典範，可直接進入系統實作階段。 |

> **【後續實作階段之具體優化建議】**
>
> 1. **二手市場資料稀疏性應對**：特殊實驗儀器（如示波器、光譜儀）之二手掛牌量可能極低，建議在 rules.yaml 中預先建立「儀器折舊衰減基準矩陣」，當小樣本或無成交紀錄時能優雅降級。
> 2. **Web 爬蟲與採購法共同供應契約對接**：電商網頁存在反爬蟲機制，共同供應契約（政府電子採購網）亦有特定登入或查詢格式，實作時宜設計本地模擬資料或專屬 Mock API，以確保評估實驗之可重現性。
> 3. **人機協同介面（Streamlit）視覺化呈現**：在管理介面上，可強化「帳面價值 vs. 市場價值區間對照圖」與「可移撥設備建議卡片」，提升指導教授與管理人員在審核時之信任感。
