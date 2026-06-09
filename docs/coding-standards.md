# 程式規範

本文件記錄目前規劃階段的開發原則，作為後續正式實作時的基線。

## Coding Style

### 1. 可讀性優先

- 優先寫清楚、直白、可維護的程式
- 避免過度聰明或過度壓縮的寫法
- 一個檔案只承擔一個主要責任

### 2. TypeScript 優先

- 啟用 `strict`
- 盡量避免 `any`
- 優先用明確型別描述資料結構與流程邊界
- 對外部輸入與第三方資料要做型別驗證，不直接信任

### 3. 命名規則

- 檔名使用 `kebab-case`
- 變數與函式使用 `camelCase`
- React component、type、interface、class 使用 `PascalCase`
- 常數使用具有語意的命名，避免模糊縮寫

### 4. 函式與模組

- 函式保持短小，盡量單一責任
- 複雜邏輯優先拆成小函式，不堆在同一層
- 共用邏輯放在明確模組中，避免複製貼上
- provider-specific 邏輯不可散落在 domain 與 UI 中

### 5. 註解原則

- 只在需要說明「為什麼」時註解
- 不註解顯而易見的實作細節
- 複雜規則、外部限制、資安邊界可以用簡短註解說明

## 架構原則

### 1. 模組化優先

先以模組化單體為方向，不過早拆成多服務。

### 2. Provider Adapter 模式

外部整合需透過 adapter 層承接，不可讓核心 domain 直接依賴特定 provider。

適用範圍：

- ticket provider
- repository provider
- AI provider
- editor implementation

### 3. Data Access 抽象

資料存取層需獨立封裝，避免直接綁死單一資料庫。

## 命名與分層

- 核心流程以 project 為起點
- room、ticket、repository、AI、automation 應有清楚模組邊界
- 外部系統專屬邏輯應留在 adapter/infrastructure 層

## 檔案與目錄慣例

### 1. 建議模組切分

```text
modules/
├─ project
├─ room
├─ ticket
├─ repository
├─ ai
├─ automation
└─ openspec
```

### 2. 檔案命名建議

- `project-service.ts`
- `create-ticket.ts`
- `monaco-editor-adapter.ts`
- `github-ticket-adapter.ts`

### 3. 匯入順序

建議維持固定順序：

1. framework / external packages
2. internal shared packages
3. local modules
4. type-only imports

同層級 import 應保持穩定排序。

## React / Next.js 寫法

### 1. Component 原則

- component 只處理畫面與互動
- 不在 component 中塞過多 domain 邏輯
- 複雜流程應下沉到 service / use case / module 層

### 2. Hook 原則

- custom hook 用於封裝 UI 狀態與互動邏輯
- 不讓 hook 直接承擔 provider adapter 職責
- hook 名稱以 `use` 開頭，保持語意清楚

### 3. 頁面與 route

- page 負責組裝畫面與資料流
- route handler 負責 API 邊界與輸入輸出處理
- 不在 route handler 中直接堆疊大量商業邏輯

## API 與 Service 寫法

### 1. API 邊界

- request 進入後先做 schema 驗證
- response 保持穩定格式
- 錯誤需明確分類，不返回模糊錯誤

### 2. Service / Use Case

- 一個 use case 處理一個明確動作
- 例如：`confirmDecision`、`createExternalTicket`、`runOpenSpecValidation`
- service 不直接綁死某一個 provider 名稱

### 3. Adapter

- 每個外部系統使用獨立 adapter
- adapter 實作 provider-specific 邏輯
- adapter 對外暴露平台自己的能力介面，而不是第三方 SDK 細節

## Editor / AI / Integration 寫法

### 1. Editor

- UI 層只依賴 editor abstraction
- 不在各處直接調用 Monaco 專屬 API
- diagnostics、suggestions、selection 等能力需先標準化

### 2. AI

- AI 能力以 capability 為單位設計
- 不把業務邏輯直接綁定 Claude、Gemini、Copilot 名稱
- prompt 組裝、provider routing、result mapping 應分開處理

### 3. 外部整合

- ticket、repository、AI provider 都需先經過 adapter 層
- UI 不直接呼叫外部 SDK
- domain 不直接依賴第三方 response shape

## 可替換性要求

### Editor

編輯器邏輯需透過 wrapper / adapter 封裝，不可到處散落 Monaco 專屬 API。

### Database

應避免過度依賴單一資料庫專屬功能。

### AI

AI 能力需以平台能力概念設計，不可把業務邏輯直接綁定單一模型。

## 測試原則

- 單元測試：核心邏輯與映射
- API 測試：server 邊界與流程
- E2E 測試：專案設定、發想、定案、開票等關鍵流程
- 不直接呼叫真實外部 AI、Git、ticket 系統

## 格式化原則

- 使用統一 formatter 與 lint 規則
- 不在同一份變更中混入大量無關格式調整
- 修改既有檔案時，優先遵守檔案現有風格

## Branch 規範

### 1. 命名原則

- branch 名稱應簡短、可讀、可搜尋
- 使用小寫英數與 `-`
- 以變更類型作為前綴

建議格式：

```text
<type>/<short-description>
<type>/<ticket-or-change-id>-<short-description>
```

範例：

```text
feature/project-settings
feature/sc-003-realtime-room
fix/ticket-sync-status
docs/architecture-overview
chore/update-eslint-config
refactor/editor-adapter
test/room-api-flow
```

### 2. type 建議

- `feature/`：新功能
- `fix/`：錯誤修正
- `docs/`：文件調整
- `refactor/`：重構
- `chore/`：工具、設定、依賴更新
- `test/`：測試調整

### 3. branch 原則

- 一個 branch 聚焦一個主題
- 不把多個不相關變更混在同一個 branch
- 若已有 ticket、change id、task id，可放入 branch 名稱提升可追蹤性

## Commit 規範

### 1. 建議格式

建議採用類似 Conventional Commits 的格式：

```text
<type>(<scope>): <summary>
```

範例：

```text
feat(project): add project settings skeleton
fix(ticket): correct ticket provider mapping
docs(readme): clarify product flow
refactor(editor): extract editor adapter interface
test(room): add confirm decision API coverage
chore(deps): update eslint packages
```

### 2. type 建議

- `feat`：新功能
- `fix`：修正
- `docs`：文件
- `refactor`：重構
- `test`：測試
- `chore`：雜項維護

### 3. summary 原則

- 用一句話說清楚這次變更的核心
- 保持簡短，不描述過多背景
- 聚焦結果，不列出所有細節

不建議：

```text
update stuff
fix bug
misc changes
```

建議：

```text
feat(room): add AI-assisted draft actions
fix(repository): handle missing default branch
docs(tech-options): document current stack candidates
```

### 4. commit 邊界

- 一個 commit 盡量只做一件事
- 文件、重構、功能、測試若彼此獨立，應分開 commit
- 避免把純格式化與實際邏輯修改混在同一個 commit

## Git 工作原則

- branch 與 commit 都應可追蹤變更意圖
- branch 命名盡量對齊 feature、ticket、change 或 task
- commit 訊息應讓 reviewer 在不打開 diff 前就能理解本次目的

## 安全原則

- 敏感金鑰不得暴露到前端
- 金鑰需由後端管理
- 外送 AI 前需保留資料遮罩與邊界控管能力
- 部署與執行流程應支援容器化
