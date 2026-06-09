# 資料流概覽

本文件整理 `sdd-board` 目前規劃下的主要資料流，協助後續釐清模組邊界、API 設計與安全界線。

## 核心資料流

```text
Project
  ↓
Room Context
  ↓
Draft
  ↓
Decision
  ↓
Ticket
  ↓
Repository / OpenSpec / Automation
```

## 1. Project → Room

### 說明

- 使用者先進入或選擇 project
- project settings 提供後續 room 的上下文
- room 不應脫離 project 單獨存在

### 主要輸入

- project 基本資料
- ticket settings
- repository settings
- AI settings
- automation settings

## 2. Room → Draft

### 說明

- 在 room 中，人與 AI 共同互動
- 討論結果沉澱為 draft
- draft 屬於定案前內容

### draft 可能承載

- proposal
- design
- spec
- tasks
- 補充摘要

## 3. Draft → Decision

### 說明

- 人工確認後，draft 才轉成 decision
- decision 表示內容已可進入正式流程

### 邊界原則

- 未確認的內容不可直接視為 decision
- decision 不應隱性產生，需有明確操作

## 4. Decision → Ticket

### 說明

- decision 產生後，平台依 project settings 向外部系統開票
- 開票結果回到平台形成 ticket context

### 資料轉換

- platform decision data
  ↓
- ticket adapter
  ↓
- external ticket payload
  ↓
- external ticket response
  ↓
- normalized ticket

## 5. Ticket → Repository / OpenSpec / Automation

### 說明

- ticket 建立後，作為後續正式流程的入口
- 後續可接 repository 綁定、OpenSpec 流程與 automation

### 主要流向

```text
Ticket
  ├─ bind repository
  ├─ prepare repository context
  ├─ continue OpenSpec-related flow
  └─ trigger automation
```

## AI 資料流

### 在發想階段

```text
Project AI Settings
  ↓
Prompt Context Builder
  ↓
AI Provider Adapter
  ↓
AI Response
  ↓
Room / Draft
```

### 原則

- AI 上下文需受 project 設定控制
- AI 回傳內容屬於建議或草稿，不直接視為正式決策

## 外部系統資料流

```text
Platform Domain Model
  ↓
Adapter
  ↓
External System
  ↓
Adapter
  ↓
Normalized Internal Model
```

### 原則

- 所有外部資料進入平台前都應先做標準化
- 所有平台資料送往外部前都應先轉成 provider 需要的格式

## 一句話總結

> `sdd-board` 的主要資料流應從 project 出發，經過 room、draft、decision、ticket，再延伸到 repository、OpenSpec 與 automation，同時由 adapter 層承接所有外部資料轉換。
