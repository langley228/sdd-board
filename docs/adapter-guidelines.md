# Adapter 設計準則

本文件整理 `sdd-board` 中 ticket、repository、AI、editor 等外部能力接入時的 adapter 規則。

## 核心原則

- 核心 domain 不直接依賴第三方 SDK
- provider-specific 邏輯集中在 adapter 層
- adapter 對外暴露平台自己的能力介面
- 未來新增 provider 時，不應大幅修改既有 domain

## 適用範圍

- ticket provider adapter
- repository provider adapter
- AI provider adapter
- editor adapter

## 1. Adapter 的角色

adapter 的工作不是把第三方 SDK 直接丟給其他模組，而是：

- 接住第三方 API / SDK
- 做資料轉換
- 做錯誤轉換
- 做能力映射
- 回傳平台內部可理解的格式

## 2. 平台內部應依賴什麼

平台內部應依賴：

- `createTicket`
- `listRepositories`
- `generateDraft`
- `provideAutocomplete`
- `renderDiagnostics`

而不是直接依賴：

- 某個 provider 的 request/response shape
- 某個第三方 SDK method name
- 某個 editor library 的專屬事件格式

## 3. 錯誤處理

### 原則

- adapter 需將第三方錯誤轉成平台可理解的錯誤語意
- 不把底層錯誤直接散播到整個系統
- 錯誤分類至少應能區分：
  - auth
  - validation
  - provider failure
  - rate limit
  - unavailable

## 4. 資料轉換

### 原則

- 第三方資料進入平台時先轉成標準化格式
- 平台資料送出第三方前再轉成 provider 格式
- domain 不直接操作第三方 shape

## 5. 能力導向而非品牌導向

### Ticket

平台依賴的是開票、查詢、同步等能力，不是 Jira 或 GitHub 的品牌名稱。

### Repository

平台依賴的是 repository 選擇、綁定、後續流程上下文，不是某一家的 repo model。

### AI

平台依賴的是 chat、generation、autocomplete 等能力，不是單一模型名稱。

### Editor

平台依賴的是 content、diagnostics、suggestions、selection 等能力，不是 Monaco API 本身。

## 6. 可替換性原則

- 新增 provider 時，應優先新增 adapter，而不是修改 domain
- 移除或替換 provider 時，影響應主要落在 adapter 層
- adapter contract 應保持穩定、語意清楚

## 建議目錄

```text
adapters/
├─ ticket/
│  ├─ jira/
│  ├─ github/
│  └─ gitlab/
├─ repository/
│  ├─ github/
│  ├─ gitlab/
│  └─ bitbucket/
├─ ai/
│  ├─ claude/
│  ├─ gemini/
│  └─ copilot/
└─ editor/
   ├─ monaco/
   └─ codemirror/
```

## 一句話總結

> `sdd-board` 的 adapter 應以平台能力為中心，將第三方差異收斂在 adapter 層內，避免外部系統語意滲入核心 domain。
