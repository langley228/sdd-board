# 領域邊界

本文件用來定義 `sdd-board` 各核心領域的責任邊界，避免後續實作時互相滲透。

## 核心領域

```text
Project
Room
Draft / Decision
Ticket
Repository
AI
Automation
```

## 1. Project

### 責任

- 作為流程起點
- 保存 settings
- 作為權限與策略邊界

### 不負責

- 不直接承擔 room 互動細節
- 不直接承擔 provider-specific 實作

## 2. Room

### 責任

- 承接多人互動發想
- 維持討論上下文
- 與 draft 協作

### 不負責

- 不直接開票
- 不直接決定 repository provider 細節

## 3. Draft / Decision

### Draft 責任

- 保存定案前內容
- 承接 AI 與人類共同整理出的草稿

### Decision 責任

- 表示已確認內容
- 作為後續開票的正式輸入

### 邊界

- Draft 不等於 Decision
- Decision 必須由明確操作產生

## 4. Ticket

### 責任

- 作為定案後的正式工作入口
- 承接外部工作系統的標準化表示

### 不負責

- 不直接承擔 room 討論內容
- 不直接承擔 repository provider 差異

## 5. Repository

### 責任

- 提供後續實作與流程上下文
- 表示 repository 綁定與相關設定

### 不負責

- 不主導定案
- 不主導發想流程

## 6. AI

### 責任

- 提供 chat、generation、autocomplete、summary 等能力

### 不負責

- 不自動成為正式決策來源
- 不直接繞過 project settings 進行操作

## 7. Automation

### 責任

- 在定案後推進後續流程
- 承接 ticket 之後的動作

### 不負責

- 不取代發想
- 不取代人工定案

## 一句話總結

> `sdd-board` 的邊界核心是：Project 提供策略，Room 承接發想，Draft/Decision 處理語意轉換，Ticket 承接正式工作，Repository / AI / Automation 提供後續能力。
