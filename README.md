# OS-6
OS-6
**1. 作業目標 (Project Objectives)
本實驗旨在於 Raspberry Pi Pico (RP2040) 環境下，利用 FreeRTOS Queue (隊列) 機制實作多工任務間的通訊。

任務同步： 透過 Queue 傳遞訊息，控制 8 顆外部 LED 與 1 顆板載 LED 的閃爍狀態。

優先級管理： 驗證不同任務優先級 (TASK_PRIORITY) 對系統排程的影響。

穩定性測試： 觀察在多任務併發下，Queue 是否能正確解耦 (Decouple) 產生者與消費者任務。

2. 硬體需求 (Hardware Requirements)
開發板： Raspberry Pi Pico (RP2040)。

擴充板： 包含 8 顆外部 LED 的延伸測試板。

連線： GPIO 0 至 GPIO 7 (外部 LED)，GPIO 25 (板載 LED)。

3. 軟體環境與工具 (Software SOP)
請遵循以下標準作業程序 (SOP) 進行開發：

解壓縮： 將 RPIPicoFreeRTOSCourse8.zip 解壓縮至工作目錄。

開啟專案： 使用 VSCode 開啟資料夾 AssignmentQueue。

匯入專案： 確保 Pico SDK 環境正確，匯入 AssignmentQueue 專案。

編譯： 點擊 VSCode 下方的 Compile 或 Compile Project 按鈕生成 .uf2 檔案。

燒錄： * 按下 Pico 上的 BOOT 按鈕。

同時觸碰或按下 RST (Reset) 按鈕一次。

將編譯好的 uf2 檔案拖入名為 RPI-RP2 的磁碟機。

驗證： 觀察藍色 LED 與其餘 8 顆 LED 是否按照 Queue 傳遞的指令開始正常閃爍。

4. 關鍵技術解析 (Technical Implementation)
A. FreeRTOS Queue 機制
在本專案中，Queue 扮演了核心通訊角色：

Producer (產生者)： 發送閃爍指令或狀態至 Queue。

Consumer (消費者)： 監聽 Queue，當接收到訊息時立即驅動對應的 GPIO。

B. 任務優先級配置 (Task Priorities)
參考程式碼範例，任務優先級設定如下：

BlinkAgent (High): TASK_PRIORITY + 2 — 確保控制邏輯具備最高響應性。

Worker 2 (Medium): TASK_PRIORITY + 1 — 處理次要負載。

Worker 1 (Low): TASK_PRIORITY — 基本工作任務。

5. 實驗觀察與結論 (Observations)
系統表現： 當 Queue 正常運作時，LED 閃爍節奏應保持一致，不會因為任務負載增加而產生明顯延遲。

記憶體監控： 透過 runTimeStats() 可觀察到各任務的堆疊高水位線 (hw)，確保沒有溢出風險。

Queue 優勢： 相較於全域變數，使用 Queue 能有效避免競態條件 (Race Condition)，提高系統在即時作業系統 (RTOS) 下的可靠性。**
