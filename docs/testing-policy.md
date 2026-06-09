# 測試原則

本文件整理 `sdd-board` 在目前規劃下建議採用的測試邊界與原則。

## 測試目標

- 確保核心流程可用
- 確保 API 邊界穩定
- 確保外部整合可被隔離
- 確保未來重構時不破壞主要使用情境

## 測試層次

```text
單元測試
  ↓
API / 整合測試
  ↓
E2E 測試
```

## 1. 單元測試

### 驗證重點

- domain logic
- mapping logic
- automation rule
- validation result transform
- provider-independent service logic

### 原則

- 一次只測單一責任
- 不依賴真實外部服務
- 優先覆蓋可重複、可純函式化的邏輯

## 2. API / 整合測試

### 驗證重點

- project settings API
- room flow API
- confirm decision API
- ticket creation API
- repository binding API
- AI request boundary

### 原則

- 驗證 request / response shape
- 驗證輸入驗證與錯誤處理
- 驗證 server 邊界，而不是只測內部函式

## 3. E2E 測試

### 驗證重點

- 建立/選擇專案
- 進入 room
- 人與 AI 互動發想
- 定案
- 開票
- ticket 後續流程入口可用

### 原則

- 只覆蓋最重要的使用者主流程
- 不追求把所有細節都堆到 E2E
- 優先保障產品主軸不壞掉

## 外部系統測試邊界

以下外部系統在測試中應以 mock、stub 或測試替身隔離：

- AI provider
- ticket provider
- repository provider
- Git 流程
- OpenSpec CLI

## 明確限制

- 不直接呼叫真實 AI API
- 不直接呼叫真實 Jira / GitHub / GitLab / Bitbucket
- 不直接操作正式儲存庫
- 不在測試中依賴不可控的網路狀態

## 建議工具

- `Jest`：單元測試
- `Supertest`：API / server 測試
- `Playwright`：E2E 測試

## 測試資料原則

- 測試資料應最小化
- 盡量使用可讀、可追蹤的假資料
- provider 測試資料應避免綁定真實憑證與真實專案資訊

## 一句話總結

> `sdd-board` 的測試策略應以「主流程優先、外部整合隔離、邊界清楚」為核心原則。
