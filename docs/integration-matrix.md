# 外部整合矩陣

本文件整理 `sdd-board` 目前規劃下會接入的外部系統類型，以及它們在平台中的角色。

## 整合原則

- 外部系統提供能力，不主導平台核心流程
- 平台內部應先有自己的標準化概念
- provider-specific 差異由 adapter 層承接

## 整合分類

```text
Ticket Systems
Repository Systems
AI Systems
```

## 1. Ticket Systems

### 角色

- 承接定案後的正式工作項目
- 提供 ticket 建立、查詢、狀態同步等能力

### 目前規劃對象

- Jira
- GitHub Issues
- GitLab Issues

### 平台內部應依賴的能力

- `createTicket`
- `getTicket`
- `syncTicketStatus`

### 平台內部不應直接依賴

- Jira 專屬 issue type 欄位名稱
- GitHub issue response shape
- GitLab issue payload 格式

## 2. Repository Systems

### 角色

- 承接定案後的 repository 上下文
- 提供 Git 流程與後續 OpenSpec / automation 銜接能力

### 目前規劃對象

- GitHub Repositories
- GitLab Repositories
- Bitbucket Repositories

### 平台內部應依賴的能力

- `listRepositories`
- `bindRepository`
- `getDefaultBranch`
- `prepareRepositoryContext`

### 平台內部不應直接依賴

- 某一個 provider 的 repo model
- 單一平台的 branch / namespace 命名格式

## 3. AI Systems

### 角色

- 在發想階段提供問答、補全、生成、摘要等能力
- 在後續自動化中提供輔助與推進能力

### 目前規劃對象

- Claude
- Gemini
- Copilot

### 平台內部應依賴的能力

- `chat`
- `generateDraft`
- `autocomplete`
- `summarize`

### 平台內部不應直接依賴

- 單一模型名稱
- 單一 provider SDK 介面
- 第三方 response 形狀

## 整合矩陣

| 類型 | 外部系統 | 在平台中的角色 | 內部抽象方向 |
|---|---|---|---|
| Ticket | Jira | 定案後開票 | ticket capability |
| Ticket | GitHub Issues | 定案後開票 | ticket capability |
| Ticket | GitLab Issues | 定案後開票 | ticket capability |
| Repository | GitHub | repository 上下文 | repository capability |
| Repository | GitLab | repository 上下文 | repository capability |
| Repository | Bitbucket | repository 上下文 | repository capability |
| AI | Claude | 發想 / 生成 / 摘要 | AI capability |
| AI | Gemini | 發想 / 生成 / 補全 | AI capability |
| AI | Copilot | 補全 / 輔助 | AI capability |

## 一句話總結

> `sdd-board` 對外整合的重點不是支援幾家服務，而是先定義 ticket、repository、AI 三類能力，再由 adapter 承接不同 provider。
