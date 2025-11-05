**產品名稱：** AI 旅遊規劃器

**團隊成員：**
*   組長：林煒翔
*   組員：龍顥文、許鼎祥
*   指導教授：陳煥

**1. 簡介**

*   **問題陳述：** 當前的旅遊諮詢服務面臨資訊高度碎片化的問題。使用者需要花費大量時間在多個來源（如線上旅行社 OTA、航空公司、社群媒體）上手動搜尋、比較和整合數據，才能獲得全面的比價和最佳折扣。
*   **解決方案：** 開發一個 AI Agent 系統，能夠融合結構化（API 數據）和非結構化（社群媒體折扣）的數據，實現時間節省與綜合優惠識別。
*   **專案目標：** 建立可擴展的 AI Agent 系統，負責數據的解析、決策和執行，為使用者提供自動化的旅遊規劃體驗。
*   **預期效益：** 縮短客戶回應時間、提高客戶滿意度、降低運營成本。

**2. 相關研究**

*   **Amadeus API：** 一個提供航班即時資料服務的平台，提供 Flights Search、Pricing、Schedule 等 API 端點。
*   **Skyscanner API + Flight-Spy：**  Skyscanner API 提供比價與價格通知，Flight-Spy 是一個開源專案，示範如何利用 Skyscanner API 追蹤機票價格，並在價格低於門檻時發出通知。
*   **線上 APP：**
    *   iMean AI Flight Planner：一款以人工智慧驅動、專注於機票搜尋與旅遊規劃的APP。
    *   Mindtrip AI 旅遊助理：以 AI 為核心、支援行程共創與即時建議的旅遊助理平台。

**3. 產品設計**

*   **系統架構：** 採用三層式架構：
    *   **呈現層 (Presentation Layer)：** 負責用戶交互，通過 WebHook 將用戶輸入傳輸到系統核心。
    *   **協調層 (Orchestration Layer)：** 以 n8n 工作流為核心，包含 LangChain Agent 作為推理引擎，負責任務解析、決策和流程控制。
    *   **數據獲取層 (Data Acquisition Layer)：** 包含所有數據源，包括實時 API 調用和定製爬蟲。
*   **流程入口：**
    *   WebHook：會即時接收來自用戶介面的請求，並將這些請求以標準化的格式傳遞給後端的 n8n 工作流程，確保資料傳輸的即時性與可靠性。
*   **核心協調層：**
    *   n8n 接收 WebHook 數據後啟動 LangChain Agent 節點，使用 LLM 驅動，負責意圖解析、狀態管理和模組決策。
*   **子節點層：**
    *   API：負責接收查詢、發送 API 請求、檢查 API 狀態碼、處理 API 資料和傳遞資料。
    *   爬蟲模組：負責初始化爬蟲、發送 HTTP 請求、檢查 HTTP 狀態碼、解析網頁內容、提取目標資料、清洗轉換資料和儲存資料。
*   **輸出層：**
    *   資料整合模組：負責接收來自爬蟲模組和 API 介面的資料、清洗資料、資料轉換、資料合併、資料去重和傳遞資料。
*   **多代理 AI 旅遊規劃器**
    * 各個AI Agent任務分工：
        * Flight Analyst: 從 SerpAPI 取得航班資訊,依票價、時長、轉機與艙等進行排序與建議
        * Hotel Analyst Agent：比較多家飯店的價格、評價、距離與設施,挑選最合適選項。
        * Travel Planner Agent：將航班與飯店結果整合成完整行程,包含每日活動與建議時程。
    * 任務流動順序與輸出整合：使用者輸入到 AI 生成最終行程的完整執行鏈。

**4. 技術細節**

*   **LLM Core：** Google Gemini 2.0
*   **Agent Framework：** CrewAI
*   **Data Source：** SerpAPI (Google Flights / Hotels)
*   **Backend：** FastAPI + Pydantic
*   **Frontend：** Streamlit

**5. 未來方向**

*   可改進為 Parallel Flow 以加速資料蒐集階段。
*   可整合官方 API (如 Amadeus / Skyscanner) 提升資料準確性。