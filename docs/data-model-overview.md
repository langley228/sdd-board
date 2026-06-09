# 資料模型概覽

本文件整理 `sdd-board` 目前規劃下的核心資料物件與關係。

## 核心物件

```text
Project
├─ ProjectSettings
├─ Room
├─ Draft
├─ Ticket
├─ RepositoryBinding
└─ AutomationState
```

## 1. Project

### 角色

- 所有流程的起點
- 設定與權限的邊界

### 可能欄位

- `id`
- `name`
- `key`
- `description`
- `status`

## 2. ProjectSettings

### 角色

- project 的策略來源
- 控制 ticket、repository、AI、automation 相關行為

### 可能欄位群組

- basic settings
- ticket settings
- repository settings
- AI settings
- automation settings

## 3. Room

### 角色

- 多人互動發想空間
- 保存討論上下文

### 可能欄位

- `id`
- `projectId`
- `title`
- `status`
- `createdBy`

## 4. Draft

### 角色

- 定案前的草稿容器

### 可能欄位

- `id`
- `roomId`
- `proposal`
- `design`
- `spec`
- `tasks`
- `version`

## 5. Decision

### 角色

- 將草稿轉成已確認內容

### 可能欄位

- `id`
- `roomId`
- `confirmedAt`
- `confirmedBy`
- `summary`

## 6. Ticket

### 角色

- 定案後的正式工作入口
- 對應外部 ticket provider

### 可能欄位

- `id`
- `projectId`
- `decisionId`
- `provider`
- `externalTicketId`
- `externalUrl`
- `status`

## 7. RepositoryBinding

### 角色

- 綁定定案後要銜接的 repository

### 可能欄位

- `id`
- `projectId`
- `ticketId`
- `provider`
- `repositoryName`
- `namespace`
- `defaultBranch`

## 8. AutomationState

### 角色

- 紀錄後續 automation 執行狀態

### 可能欄位

- `id`
- `ticketId`
- `type`
- `status`
- `startedAt`
- `finishedAt`

## 關係概念

```text
Project
  ├─ has one ProjectSettings
  ├─ has many Rooms
  ├─ has many Tickets
  └─ has many RepositoryBindings

Room
  ├─ has one active Draft
  └─ may produce one Decision

Decision
  └─ creates one Ticket

Ticket
  ├─ may bind one or more Repositories
  └─ may trigger many AutomationStates
```

## 原則

- project 是最高層邊界
- 定案前後的資料語意必須分開
- 外部系統資料需透過標準化欄位承接
- provider-specific 欄位不應污染核心模型

## 一句話總結

> `sdd-board` 的資料模型應圍繞 project、room、draft、decision、ticket 這條主線建立，再向外延伸 repository 與 automation 狀態。
