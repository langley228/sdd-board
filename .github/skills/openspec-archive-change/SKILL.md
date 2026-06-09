---
name: openspec-archive-change
description: 封存實驗性工作流程中已完成的變更。當使用者想要在實作完成後完成並封存變更時使用。
license: MIT
compatibility: 需要 openspec CLI。
metadata:
  author: openspec
  version: "1.0"
  generatedBy: "1.4.1"
---

封存實驗性工作流程中已完成的變更。

**輸入**：可選擇性指定變更名稱。若省略，請嘗試從對話上下文推斷。若模糊不清，必須提示使用者選擇可用的變更。

**步驟**

1. **若未提供變更名稱，提示使用者選擇**

   執行 `openspec list --json` 取得可用變更。使用 **AskUserQuestion 工具** 讓使用者選擇。

   只顯示進行中的變更（未已封存的）。
   若可取得，包含每個變更所用的 schema。

   **重要**：不要猜測或自動選擇變更，務必讓使用者選擇。

2. **確認 artifact 完成狀態**

   執行 `openspec status --change "<name>" --json` 確認 artifact 完成情況。

   解析 JSON 以了解：
   - `schemaName`：正在使用的工作流程
   - `planningHome`、`changeRoot`、`artifactPaths` 與 `actionContext`：路徑與範圍上下文
   - `artifacts`：附狀態（`done` 或其他）的 artifact 清單

   若 status 回報 `actionContext.mode: "workspace-planning"`，說明此切片不支援工作區封存並**停止**。不要將工作區變更移入 repo 本地封存或編輯連結的 repo。

   **若有任何 artifact 未 `done`：**
   - 顯示列出未完成 artifacts 的警告
   - 使用 **AskUserQuestion 工具** 確認使用者是否要繼續
   - 若使用者確認則繼續

3. **確認任務完成狀態**

   讀取任務檔（通常是 `tasks.md`）確認是否有未完成任務。

   計算標記 `- [ ]`（未完成）與 `- [x]`（已完成）的任務數量。

   **若發現未完成任務：**
   - 顯示未完成任務數量的警告
   - 使用 **AskUserQuestion 工具** 確認使用者是否要繼續
   - 若使用者確認則繼續

   **若不存在任務檔：** 不顯示任務相關警告，直接繼續。

4. **評估 delta spec 同步狀態**

   使用 status JSON 中的 `artifactPaths.specs.existingOutputPaths` 確認是否有 delta specs。若不存在，不顯示同步提示直接繼續。

   **若存在 delta specs：**
   - 將每個 delta spec 與 `openspec/specs/<capability>/spec.md` 的對應主規格比較
   - 確定將套用的變更（新增、修改、移除、重新命名）
   - 提示前先顯示合併摘要

   **提示選項：**
   - 若需要變更：「立即同步（建議）」、「不同步直接封存」
   - 若已同步：「立即封存」、「仍然同步」、「取消」

   若使用者選擇同步，使用 Task 工具（subagent_type: "general-purpose"，prompt: "Use Skill tool to invoke openspec-sync-specs for change '<name>'. Delta spec analysis: <include the analyzed delta spec summary>"）。無論使用者的選擇，繼續進行封存。

5. **執行封存**

   若 `planningHome.changesDir` 下不存在 `archive` 目錄則建立：
   ```bash
   mkdir -p "<planningHome.changesDir>/archive"
   ```

   使用當前日期生成目標名稱：`YYYY-MM-DD-<change-name>`

   **確認目標是否已存在：**
   - 若已存在：以錯誤失敗，建議重新命名現有封存或使用不同日期
   - 若不存在：將 `changeRoot` 移至封存目錄

   ```bash
   mv "<changeRoot>" "<planningHome.changesDir>/archive/YYYY-MM-DD-<name>"
   ```

6. **顯示摘要**

   顯示封存完成摘要，包含：
   - 變更名稱
   - 使用的 schema
   - 封存位置
   - 是否已同步規格（若適用）
   - 任何警告的備註（未完成的 artifacts/任務）

**成功時的輸出**

```
## 封存完成

**變更：** <change-name>
**Schema：** <schema-name>
**封存至：** 由 `planningHome.changesDir`/YYYY-MM-DD-<name>/ 導出的封存路徑
**規格：** ✓ 已同步至主規格（或「無 delta specs」或「已跳過同步」）

所有 artifacts 完成。所有任務完成。
```

**防護措施**
- 若未提供名稱，務必提示使用者選擇變更
- 使用 artifact 圖（openspec status --json）確認完成情況
- 不因警告阻止封存——只需通知並確認
- 移動至封存時保留 .openspec.yaml（它會隨目錄一起移動）
- 清楚顯示發生了什麼
- 若要求同步，使用 openspec-sync-specs 方式（agent 驅動）
- 若存在 delta specs，務必先執行同步評估並在提示前顯示合併摘要
