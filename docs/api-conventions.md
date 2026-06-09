# API 設計約定

本文件整理 `sdd-board` 在目前規劃下建議採用的 API 設計原則。

## 核心原則

- API 是平台邊界，不是把內部實作直接暴露出去
- API 應優先表達業務語意，而不是第三方 provider 細節
- request / response 應保持穩定、可預測
- 輸入必須驗證，錯誤必須可理解

## API 分類

```text
Project APIs
Room APIs
Decision APIs
Ticket APIs
Repository APIs
Automation APIs
```

## 命名原則

- 以資源與動作語意命名
- 避免直接暴露第三方品牌命名

例如：

- `/api/projects`
- `/api/projects/:projectId/rooms`
- `/api/rooms/:roomId/decision`
- `/api/decisions/:decisionId/tickets`
- `/api/tickets/:ticketId/repositories`

## Request / Response 原則

### Request

- 進入 API 後先做 schema 驗證
- 不直接信任前端輸入
- 對外部 provider 相關欄位需先轉成平台語意

### Response

- 優先回傳平台自己的標準格式
- 避免直接回傳第三方原始 payload
- 回傳內容應包含足夠上下文，但不洩漏敏感資訊

## 錯誤處理原則

API 錯誤需可區分至少以下類型：

- validation error
- auth / permission error
- not found
- provider error
- unexpected error

避免：

- 模糊錯誤訊息
- 把第三方原始錯誤直接回傳給前端

## 與 provider 的邊界

API 不應直接設計成：

- `/api/jira/...`
- `/api/github/...`
- `/api/gitlab/...`

除非該 API 的目的就是管理 provider 設定本身。

一般流程 API 應以平台主線命名：

- project
- room
- decision
- ticket
- repository
- automation

## 安全原則

- API 不回傳敏感金鑰
- API 不暴露不必要的 provider 細節
- 關鍵操作需受權限控制

## 一句話總結

> `sdd-board` 的 API 應以平台業務語意為中心，將第三方系統細節封裝在 adapter 與 server 內部，而不是直接暴露到 API 介面上。
