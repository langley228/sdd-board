---
description: 將 delta specs 從變更同步至主規格
---

將變更中的 delta specs 同步至主規格。

這是一個 **agent 驅動**的操作——你將讀取 delta specs 並直接編輯主規格來套用變更。這允許智慧合併（例如，新增場景而不複製整個需求）。

**輸入**：可在 `/opsx:sync` 後選擇性指定變更名稱（例如：`/opsx:sync add-auth`）。若省略，確認是否能從對話上下文推斷。若模糊不清，**必須**提示可用的變更。

**步驟**

1. **若未提供變更名稱，提示使用者選擇**

   執行 `openspec list --json` 取得可用變更。使用 **AskUserQuestion 工具** 讓使用者選擇。

   顯示有 delta specs 的變更（在 `specs/` 目錄下）。

   **重要**：不要猜測或自動選擇變更，務必讓使用者選擇。

2. **解析變更上下文**

   執行：
   ```bash
   openspec status --change "<name>" --json
   ```

   若 status 回報 `actionContext.mode: "workspace-planning"`，說明此切片不支援工作區規格同步並**停止**。不要退回使用 repo 本地路徑或編輯連結的 repo。

3. **尋找 delta specs**

   使用 status JSON 中的 `artifactPaths.specs.existingOutputPaths` 作為 delta spec 檔案清單。

   每個 delta spec 檔案包含以下節：
   - `## ADDED Requirements` - 要新增的新需求
   - `## MODIFIED Requirements` - 對現有需求的變更
   - `## REMOVED Requirements` - 要移除的需求
   - `## RENAMED Requirements` - 要重新命名的需求（FROM:/TO: 格式）

   若未找到 delta specs，通知使用者並停止。

4. **對每個 delta spec，將變更套用至主規格**

   對 CLI 回傳的每個 repo 本地能力 delta spec 路徑：

   a. **讀取 delta spec** 以了解預期的變更

   b. **讀取主規格**（位於 `openspec/specs/<capability>/spec.md`，可能尚不存在）

   c. **智慧地套用變更**：

      **ADDED 需求：**
      - 若主規格中不存在需求 → 新增它
      - 若需求已存在 → 更新它以匹配（視為隱式 MODIFIED）

      **MODIFIED 需求：**
      - 在主規格中找到需求
      - 套用變更，可以是：
        - 新增新場景（不需要複製現有場景）
        - 修改現有場景
        - 變更需求描述
      - 保留 delta 中未提及的場景/內容

      **REMOVED 需求：**
      - 從主規格中移除整個需求區塊

      **RENAMED 需求：**
      - 找到 FROM 需求，重新命名為 TO

   d. **若能力尚不存在則建立新主規格**：
      - 建立 `openspec/specs/<capability>/spec.md`
      - 新增 Purpose 節（可以簡短，標記為 TBD）
      - 新增含 ADDED 需求的 Requirements 節

5. **顯示摘要**

   套用所有變更後，摘要：
   - 哪些能力已更新
   - 做了哪些變更（需求新增/修改/移除/重新命名）

**Delta Spec 格式參考**

```markdown
## ADDED Requirements

### Requirement: New Feature
The system SHALL do something new.

#### Scenario: Basic case
- **WHEN** user does X
- **THEN** system does Y

## MODIFIED Requirements

### Requirement: Existing Feature
#### Scenario: New scenario to add
- **WHEN** user does A
- **THEN** system does B

## REMOVED Requirements

### Requirement: Deprecated Feature

## RENAMED Requirements

- FROM: `### Requirement: Old Name`
- TO: `### Requirement: New Name`
```

**關鍵原則：智慧合併**

與程式化合併不同，你可以套用**部分更新**：
- 要新增場景，只需在 MODIFIED 下包含該場景——不需要複製現有場景
- delta 代表*意圖*，不是全面替換
- 使用你的判斷來合理地合併變更

**成功時的輸出**

```
## 規格已同步：<change-name>

已更新主規格：

**<capability-1>**：
- 新增需求：「New Feature」
- 修改需求：「Existing Feature」（新增了 1 個場景）

**<capability-2>**：
- 建立新規格檔案
- 新增需求：「Another Feature」

主規格已更新。變更仍在進行中——實作完成後再封存。
```

**防護措施**
- 做出變更前讀取 delta 和主規格
- 保留 delta 中未提及的現有內容
- 若有不清楚的地方，請求釐清
- 進行時顯示你要變更的內容
- 操作應是冪等的——執行兩次應得到相同結果
