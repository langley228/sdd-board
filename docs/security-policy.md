# 安全原則

本文件整理 `sdd-board` 在目前規劃下的安全邊界與基本原則。

## 核心原則

- 敏感資訊只在後端處理
- 外部系統憑證不得暴露到前端
- AI 外送資料需保留遮罩與邊界控管
- repository 與外部系統操作需可追蹤
- 預設採最小權限原則

## 1. 機敏資料管理

### 適用範圍

- AI provider token
- repository 憑證
- ticket provider 憑證
- 其他外部系統 API key

### 原則

- 所有機敏資訊皆由後端保存
- 不將敏感憑證回傳給前端
- 憑證需有獨立欄位與保護機制
- project 間不得共用未隔離的機敏資料

## 2. AI 資料邊界

### 原則

- 送往 AI 前需明確控制上下文範圍
- 需保留資料遮罩（masking）能力
- 需避免把不必要的敏感資料送出
- AI 回傳結果不得直接視為可信輸入

### 建議控管點

- prompt context builder
- masking policy
- project-level AI policy
- provider routing boundary

## 3. Project 隔離

### 原則

- 每個 project 都有自己的 ticket、repository、AI、automation 設定
- project 間的設定、憑證、上下文不得混用
- room、draft、ticket context 需受 project 邊界保護

## 4. Git / Repository 安全

### 原則

- Git 憑證不得直接暴露在前端
- repository 操作需經過後端授權與控制
- 須避免讓流程直接污染正式儲存庫
- 後續 Git / OpenSpec 執行需保留隔離執行空間概念

## 5. 權限與可見性

### 原則

- 使用者只能看到其有權限的 project 與內容
- project 設定頁、敏感設定頁與執行動作需受權限控制
- 定案、開票、後續自動化等關鍵動作應能限制操作者

## 6. 測試安全原則

- 測試不得使用真實正式憑證
- 測試不得直接呼叫真實外部服務
- 測試假資料需與真實專案、真實帳號分離

## 一句話總結

> `sdd-board` 的安全設計應以「project 隔離、後端保管機敏資訊、AI 外送邊界可控、外部操作可追蹤」為核心。
