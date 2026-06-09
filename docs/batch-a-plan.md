# Batch A 規劃

本文件用來收斂 `sdd-board` 第一批實作主線，也就是：

```text
Create Project
  -> Project Settings（最小化）
  -> Room（人 + AI）
  -> Decision
  -> Create Ticket
```

## 對應 Change

- `sc-001-project-foundation`
- `sc-002-ideation-room`
- `sc-003-decision-gate`
- `sc-004-ticket-creation`

## 目標

Batch A 的目標不是把整個平台做滿，而是先把產品最核心的主線跑通：

1. 使用者可以建立或選擇 project
2. 在 project 內進入 room
3. 在 room 中進行人與 AI 的互動發想
4. 將 draft 明確確認為 decision
5. 定案後直接開票，產生 ticket

## Batch A 不做什麼

- 不追求一次支援所有 ticket provider
- 不追求 repository 流程完整落地
- 不追求 OpenSpec 後續流程全部接通
- 不追求完整 automation runtime
- 不追求複雜權限矩陣

## 1. sc-001-project-foundation

### 目標

建立整個平台的流程起點。

### 第一版最低範圍

- 建立 project
- 列出 project
- 選擇 project
- 最小 project settings

### 最小 project settings 建議

- `projectName`
- `projectKey`
- `defaultTicketProvider`
- `defaultAiProvider`
- `description`（可選）

### 成功條件

- 可以進入某一個 project 的工作空間
- project 能提供後續 room / decision / ticket 的上下文

## 2. sc-002-ideation-room

### 目標

建立人與 AI 共同參與的發想空間。

### 第一版最低範圍

- 建立 room
- 建立 draft
- room 內對話區
- room 內草稿區
- AI 互動入口

### Draft 第一版建議欄位

- `proposal`
- `design`
- `tasks`

### 成功條件

- 使用者可在 room 中輸入內容
- 可與 AI 互動
- 討論結果可沉澱成 draft

## 3. sc-003-decision-gate

### 目標

讓「發想中」與「已確認」的內容有清楚分界。

### 第一版最低範圍

- decision review 區塊
- 顯性 confirm action
- decision 狀態建立

### 第一版建議做法

- 先採單一步驟確認
- 暫不拆 proposal / design / tasks 多段審批

### 成功條件

- draft 不會自動變成 decision
- 定案是使用者明確執行的動作

## 4. sc-004-ticket-creation

### 目標

在 decision 之後，讓系統能直接開票。

### 第一版最低範圍

- decision 後呼叫 ticket create flow
- 建立 ticket context
- 顯示 ticket 建立結果

### 第一版原則

- 先只落一個 ticket provider
- 先做 create ticket，不急著做完整 sync

### 成功條件

- 定案後能建立 ticket
- ticket 資訊能回到平台內
- 使用者能看到 ticket 成果

## Batch A 建議資料主線

```text
Project
  -> Room
  -> Draft
  -> Decision
  -> Ticket
```

這條主線應優先保持清楚，避免過早混入 repository、automation、OpenSpec 等後續概念。

## Batch A 建議畫面主線

```text
Project List / Create Project
  -> Project Workspace
  -> Ideation Room
  -> Decision Review
  -> Ticket Result / Ticket Workbench 初版
```

## Batch A 待進一步收斂的點

- 第一個 ticket provider 先選哪個
- 第一個 AI provider 先選哪個
- Room 內 AI 互動的最小形態
- Decision review 顯示的摘要粒度

## 一句話總結

> Batch A 的目的，是先把 `project -> room -> decision -> ticket` 做成一條真能跑通的最小主線，讓 `sdd-board` 的產品靈魂先出現。
