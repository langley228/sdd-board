---
description: 提案新變更——一步建立並生成所有 artifacts
---

提案新變更——一步建立變更並生成所有 artifacts。

我將建立包含以下 artifacts 的變更：
- proposal.md（做什麼與為什麼）
- design.md（如何做）
- tasks.md（實作步驟）

準備好實作時，執行 /opsx:apply

---

**輸入**：`/opsx:propose` 後的參數是變更名稱（kebab-case），或使用者想要建構內容的描述。

**步驟**

1. **若未提供輸入，詢問要建構什麼**

   使用 **AskUserQuestion 工具**（開放式，無預設選項）詢問：
   > 「您想要做什麼變更？描述您想要建構或修復的內容。」

   從他們的描述導出 kebab-case 名稱（例如：「add user authentication」→ `add-user-auth`）。

   **重要**：在了解使用者要建構什麼之前，不要繼續執行。

2. **建立變更目錄**
   ```bash
   openspec new change "<name>"
   ```
   這在 CLI 解析的規劃首頁建立含 `.openspec.yaml` 的腳手架變更。

3. **取得 artifact 建構順序**
   ```bash
   openspec status --change "<name>" --json
   ```
   解析 JSON 取得：
   - `applyRequires`：實作前所需的 artifact ID 陣列（例如：`["tasks"]`）
   - `artifacts`：附狀態和依賴關係的所有 artifact 清單
   - `planningHome`、`changeRoot`、`artifactPaths` 與 `actionContext`：路徑與範圍上下文。使用這些而不是假設 repo 本地路徑。

4. **依序建立 artifacts 直到準備好套用**

   使用 **TodoWrite 工具** 追蹤 artifacts 的進度。

   依依賴順序循環處理 artifacts（先處理沒有待處理依賴的 artifacts）：

   a. **對每個狀態為 `ready`（依賴已滿足）的 artifact**：
      - 取得指示：
        ```bash
        openspec instructions <artifact-id> --change "<name>" --json
        ```
      - 指示 JSON 包含：
        - `context`：專案背景（對你的限制——不要包含在輸出中）
        - `rules`：artifact 特定規則（對你的限制——不要包含在輸出中）
        - `template`：輸出檔案的結構
        - `instruction`：此 artifact 類型的 schema 特定指導
        - `resolvedOutputPath`：解析後的輸出路徑或模式
        - `dependencies`：可讀取作為上下文的已完成 artifacts
      - 讀取任何已完成的依賴檔案作為上下文
      - 使用 `template` 作為結構建立 artifact 檔案，並寫入 `resolvedOutputPath`
      - 將 `context` 和 `rules` 作為限制套用——但不要複製到檔案中
      - 顯示簡短進度：「已建立 <artifact-id>」

   b. **繼續直到所有 `applyRequires` artifacts 完成**
      - 建立每個 artifact 後，重新執行 `openspec status --change "<name>" --json`
      - 確認 `applyRequires` 中的每個 artifact ID 在 artifacts 陣列中都有 `status: "done"`
      - 當所有 `applyRequires` artifacts 完成時停止

   c. **若某個 artifact 需要使用者輸入**（上下文不清楚）：
      - 使用 **AskUserQuestion 工具** 釐清
      - 然後繼續建立

5. **顯示最終狀態**
   ```bash
   openspec status --change "<name>"
   ```

**輸出**

完成所有 artifacts 後，摘要：
- 變更名稱和位置
- 建立的 artifacts 清單附簡短描述
- 準備好的內容：「所有 artifacts 已建立！可以開始實作了。」
- 提示：「執行 `/opsx:apply` 或請我實作來開始處理任務。」

**Artifact 建立指南**

- 遵循 `openspec instructions` 中每個 artifact 類型的 `instruction` 欄位
- schema 定義每個 artifact 應包含的內容——遵循它
- 建立新 artifact 前讀取依賴 artifacts 作為上下文
- 使用 `template` 作為輸出檔案的結構——填寫其各節
- **重要**：`context` 和 `rules` 是對**你**的限制，不是檔案內容
  - 不要將 `<context>`、`<rules>`、`<project_context>` 區塊複製到 artifact 中
  - 這些指導你要寫什麼，但絕不應出現在輸出中

**防護措施**
- 建立實作所需的**所有** artifacts（由 schema 的 `apply.requires` 定義）
- 建立新 artifact 前務必讀取依賴 artifacts
- 若上下文嚴重不清楚，詢問使用者——但傾向於做合理決定以保持動力
- 若已存在同名變更，詢問使用者是否要繼續它或建立新的
- 寫入後確認每個 artifact 檔案存在，再繼續下一個
