---
description: 實作 OpenSpec 變更中的任務（實驗性）
---

實作 OpenSpec 變更中的任務。

**輸入**：可選擇性指定變更名稱（例如：`/opsx:apply add-auth`）。若省略，確認是否能從對話上下文推斷。若模糊不清，**必須**提示可用的變更。

**步驟**

1. **選擇變更**

   若已提供名稱則直接使用。否則：
   - 若使用者提及過某個變更，從對話上下文推斷
   - 若只有一個進行中的變更，自動選擇
   - 若有歧義，執行 `openspec list --json` 取得可用變更，並使用 **AskUserQuestion 工具** 讓使用者選擇

   務必宣告：「使用變更：<名稱>」以及如何覆寫（例如：`/opsx:apply <其他>`）。

2. **查看狀態以了解 schema**
   ```bash
   openspec status --change "<name>" --json
   ```
   解析 JSON 以了解：
   - `schemaName`：正在使用的工作流程（例如 "spec-driven"）
   - `planningHome`、`changeRoot` 與 `actionContext`：規劃範圍與編輯限制
   - 哪個 artifact 包含任務（spec-driven 通常是 "tasks"，其他請查看 status）

3. **取得套用指示**

   ```bash
   openspec instructions apply --change "<name>" --json
   ```

   回傳內容包含：
   - `contextFiles`：artifact ID -> 具體檔案路徑陣列（依 schema 而異——可能是 proposal/specs/design/tasks 或 spec/tests/implementation/docs）
   - 進度（總計、已完成、剩餘）
   - 附狀態的任務清單
   - 根據當前狀態的動態指示

   **處理狀態：**
   - 若 `state: "blocked"`（缺少 artifacts）：顯示訊息，建議使用 `/opsx:continue`
   - 若 `state: "all_done"`：恭喜完成，建議封存
   - 其他情況：繼續實作

   **工作區防護：** 若 status JSON 回報 `actionContext.mode: "workspace-planning"` 且 `allowedEditRoots` 為空，說明此切片不支援完整工作區套用。將連結的 repo 和資料夾視為唯讀上下文，請使用者透過明確的實作工作流程選擇受影響的區域，並在編輯檔案前**停止**。

4. **讀取上下文檔案**

   讀取套用指示輸出中 `contextFiles` 所列的每個檔案路徑。
   檔案依使用的 schema 而定：
   - **spec-driven**：proposal、specs、design、tasks
   - 其他 schema：遵循 CLI 輸出的 contextFiles

5. **顯示當前進度**

   顯示：
   - 使用中的 schema
   - 進度：「N/M 個任務已完成」
   - 剩餘任務概覽
   - CLI 的動態指示

6. **實作任務（循環至完成或受阻）**

   對每個待處理任務：
   - 顯示正在處理哪個任務
   - 執行所需的程式碼變更
   - 保持變更精簡且聚焦
   - 在任務檔中將任務標記為完成：`- [ ]` → `- [x]`
   - 繼續下一個任務

   **暫停條件：**
   - 任務不清楚 → 請求釐清
   - 實作揭露設計問題 → 建議更新 artifacts
   - 遇到錯誤或阻礙 → 回報並等待指導
   - 使用者中斷

7. **完成或暫停時顯示狀態**

   顯示：
   - 本次作業完成的任務
   - 整體進度：「N/M 個任務已完成」
   - 若全部完成：建議封存
   - 若暫停：解釋原因並等待指導

**實作過程中的輸出**

```
## 實作中：<change-name>（schema：<schema-name>）

正在處理任務 3/7：<任務描述>
[...實作中...]
✓ 任務完成

正在處理任務 4/7：<任務描述>
[...實作中...]
✓ 任務完成
```

**完成時的輸出**

```
## 實作完成

**變更：** <change-name>
**Schema：** <schema-name>
**進度：** 7/7 個任務完成 ✓

### 本次完成的任務
- [x] 任務 1
- [x] 任務 2
...

所有任務完成！可以用 `/opsx:archive` 封存此變更。
```

**暫停時的輸出（遇到問題）**

```
## 實作暫停

**變更：** <change-name>
**Schema：** <schema-name>
**進度：** 4/7 個任務完成

### 遇到問題
<問題描述>

**選項：**
1. <選項 1>
2. <選項 2>
3. 其他方式

您想如何處理？
```

**防護措施**
- 持續處理任務直到完成或受阻
- 開始前務必讀取上下文檔案（來自套用指示輸出）
- 若任務有歧義，暫停並詢問後再實作
- 若實作揭露問題，暫停並建議更新 artifacts
- 保持程式碼變更精簡且限制在每個任務範圍內
- 完成每個任務後立即更新任務核取方塊
- 遇到錯誤、阻礙或不明確需求時暫停，不要猜測
- 使用 CLI 輸出的 contextFiles，不要假設特定檔案名稱

**流動式工作流程整合**

此技能支援「對變更執行動作」模型：

- **可隨時呼叫**：在所有 artifacts 完成之前（若已有任務）、部分實作後、與其他動作交替進行
- **允許更新 artifacts**：若實作揭露設計問題，建議更新 artifacts——不鎖定在特定階段，保持流動
