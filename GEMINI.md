# GEMINI.md

## 專案背景

- 專案名稱：`sdd-board`
- 專案定位：將 OpenSpec 從 CLI 延伸到 Web 的多專案協作平台

## 核心流程

```text
建立/選擇專案
  -> 多人互動發想（人 + AI）
  -> 人工確認定案
  -> 開票
  -> 產生 ticket
  -> 串接 repository / automation
  -> 進入 OpenSpec / Git / 外部系統後續流程
```

## 重要原則

- 專案（project）是所有流程的起點
- 發想與定案先在平台內完成，之後才開票
- AI 在發想階段即參與，不是後補功能
- 定案後，系統依專案設定向外部系統建立 ticket
- ticket、repository、AI provider、editor、database 都必須保留可替換性
- 架構先偏輕量，但要保留清楚的擴充邊界

## 目前第一候選技術

- 前端 / 全端框架：Next.js
- 資料存取層：Prisma
- 第一版資料庫候選：SQLite
- 即時互動：Socket.io
- AI 整合：Vercel AI SDK
- Web 編輯器：Monaco（需以 abstraction / adapter 封裝）
- 認證方案：Auth.js
- 測試方案：Jest + Supertest + Playwright
- 部署原則：可容器化

以上為目前第一候選，不代表不可更動的最終定案。

## 撰寫程式時的規則

- 遵守 `docs/coding-standards.md`
- 優先寫可讀、明確、可維護的 TypeScript
- 盡量避免 `any`
- provider-specific 邏輯必須留在 adapter / infrastructure 層
- 不要把第三方 SDK 細節散落在 domain 與 UI
- 模組切分保持清楚：`project`、`room`、`ticket`、`repository`、`ai`、`automation`、`openspec`
- Editor 必須視為可替換元件，不可把業務邏輯直接綁在 Monaco API 上
- Database 必須視為可替換元件，不要過度依賴單一資料庫特性，除非必要

## Git 規範

- 遵守 `docs/coding-standards.md` 中的 branch 與 commit 規範
- branch 與 commit 都應聚焦單一主題
- commit 格式建議：

```text
<type>(<scope>): <summary>
```

## 文件維護

- 產品方向、流程、架構原則變更時，同步更新 `README.md` 與 `docs/`
- 程式規範變更時，同步更新 `docs/coding-standards.md`
- 技術候選調整時，同步更新 `docs/tech-options.md`
