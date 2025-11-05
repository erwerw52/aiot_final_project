# 🤖 Ai 旅遊規劃器 (AI Travel Planner)

## 🌟 專案簡介

**Ai 旅遊規劃器** 是一個以 **多代理人 (Multi-Agent) AI 系統** 為核心的旅遊規劃解決方案。我們的目標是透過整合高度碎片化的旅遊資訊，包括結構化的 API 數據和非結構化的社群媒體優惠，為使用者提供 **即時、全面的比價和一站式行程自動化規劃體驗**。

專案的核心價值在於實現**時間節省**與**綜合優惠識別**。

## 💡 專案動機：告別研究焦慮

當前旅遊諮詢服務面臨的核心挑戰是資訊的**高度碎片化**：

*   **資料分散且難以比價：** 旅遊決策所需的關鍵數據分散在多個異質來源，包括大型線上旅行社 (OTA) 平台（如 Booking.com、Agoda、Trip.com）和專業航空公司的即時報價系統。
*   **隱藏的優惠：** 許多高價值的利基型折扣數據存在於傳統搜尋引擎無法擷取的社群媒體（例如 Facebook 社團清艙組合）中。
*   **時間成本高昂：** 用戶為了獲取全面的比價和最佳折扣，不得不投入**大量的時間**進行手動搜尋、比較與數據整合，導致超過 **10 小時**的研究焦慮。

## ✨ 核心目標與價值

本專案旨在建立一個可擴展的 AI Agent 系統，將旅遊規劃從「被動查詢」轉變為「主動規劃」。

| 效益指標 (Metric) | 改善前 (Traditional) | 改善後 (AI Agent System) | 戰略意義 (Strategic Significance) |
| :---------------- | :------------------- | :----------------------- | :------------------------------ |
| 客戶回應時間      | >30 分鐘             | **平均 < 5 秒**          | 實現全天候即時響應，大幅提升服務體驗。 |
| 客戶滿意度        | 75%                  | **95%**                  | 建立品牌忠誠度，擴大市場競爭力。 |
| 運營成本          | 高 (需大量人力)      | **顯著降低**             | 自動化處理重複性諮詢。        |

## 系統架構與技術棧

本系統採用 **三層式架構** 設計，以確保職責分離和高模塊化。

### 核心技術棧 (Technology Stack)

| 組件         | 技術/工具                        | 描述                                     |
| :----------- | :------------------------------- | :--------------------------------------- |
| **LLM Core** | Google Gemini 2.0                | 處理語意理解與最終行程生成。             |
| **Agent FW** | CrewAI                           | 協調多個任務導向的 AI 代理人。           |
| **Orchestration** | n8n (Workflow Engine)            | 核心協調平台，實現複雜工作流編排。       |
| **Backend**  | FastAPI + Pydantic               | 任務中樞，定義資料模型與執行序列流程。   |
| **Data Source** | SerpAPI (Google Flights/Hotels) | 提供即時、結構化的航班與酒店查詢結果。   |
| **Frontend** | Streamlit + WebHook              | 用於使用者互動與結果展示，並接收輸入請求。 |
| **Data Access** | Amadeus / Skyscanner API + 定制爬蟲 | 用於航班與酒店的即時資料服務。           |

### 多 Agent 協作機制 (CrewAI)

系統的核心流程由三個專門化的 AI Agent 協作完成：

| Agent 名稱             | 任務 (Task) | 資料來源 (Source) | 輸出 (Output) |
| :--------------------- | :---------- | :---------------- | :------------ |
| **Flight Analyst Agent** | 航班搜尋與推薦 | SerpAPI (Flights) | 推薦航班 JSON 摘要 |
| **Hotel Analyst Agent** | 酒店比價與推薦 | SerpAPI (Hotels)  | 推薦酒店 JSON 摘要 |
| **Travel Planner Agent** | 行程整合與生成 | Flight JSON + Hotel JSON | 完整行程表 Markdown |

**任務流動順序 (Sequential Flow)：**

1.  使用者輸入旅遊需求（經 Streamlit + WebHook 傳至 FastAPI）。
2.  FastAPI 呼叫 CrewAI Workflow。
3.  **Flight Agent** 啟動 $\to$ 呼叫 SerpAPI $\to$ 輸出航班 JSON。
4.  **Hotel Agent** 啟動 $\to$ 呼叫 SerpAPI $\to$ 輸出酒店 JSON。
5.  **Travel Planner Agent** 啟動 $\to$ 整合 JSON $\to$ Gemini LLM 分析 $\to$ 生成每日行程 Markdown。
6.  FastAPI 收集最終結果 $\to$ Streamlit 呈現給使用者。

## 🛠️ 潛在優化方向

*   **Parallel Flow：** 未來可改進為 Parallel Flow，以加速資料蒐集階段（例如同時讓 Flight Agent 和 Hotel Agent 運行）。
*   **官方 API 整合：** 整合更多官方 API (如 Amadeus / Skyscanner) 以提升資料的即時性和準確性。

## 👥 團隊成員

| 職稱 | 姓名 (中文) |
| :--- | :---------- |
| **組長** | 林煒翔
| **組員** | 龍顥文
| **組員** | 許鼎祥
| **指導教授** | 陳煥 教授
