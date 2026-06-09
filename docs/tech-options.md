# 技術候選

本文件記錄目前規劃階段的第一候選技術，僅代表當前偏好，不等於最終定案。

## 第一候選

| 區塊 | 第一候選 |
|---|---|
| 前端 / 全端框架 | Next.js |
| 資料存取層 | Prisma |
| 第一版資料庫 | SQLite |
| 即時互動 | Socket.io |
| AI 整合 | Vercel AI SDK |
| Web 編輯器 | Monaco |
| 認證方案 | Auth.js |
| 測試方案 | Jest + Supertest + Playwright |
| 部署原則 | 可容器化 |

## 技術原則

### 1. 先輕量化

第一版優先選擇能快速建立產品主流程的技術。

### 2. 保留可替換性

以下能力在設計上都應保留替換空間：

- database
- editor
- ticket provider
- repository provider
- AI provider

### 3. 抽象先於綁定

即使第一版有偏好技術，也不應讓核心 domain 直接耦合到特定 provider 或平台。

## 備註

- SQLite 為第一版候選，不代表長期資料庫定案
- Monaco 為第一版候選，但必須以可替換方式封裝
- AI 整合可先由 Vercel AI SDK 起步，後續仍可補自家 AI abstraction
