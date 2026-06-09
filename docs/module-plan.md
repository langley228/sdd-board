# 模組規劃

本文件整理 `sdd-board` 目前規劃中的核心模組，作為後續實作與目錄拆分的依據。

## 模組分層

```text
Platform Core
  ↓
Integration Hubs
  ↓
Collaboration Runtime
  ↓
Execution Layer
  ↓
Application UI
```

## 1. Platform Core

平台核心模組負責專案邊界、權限、安全與設定。

### project

- 管理 project 基本資料
- 管理專案設定入口
- 作為所有流程的起點與邊界

### auth

- 使用者登入驗證
- session / identity 管理
- 與 project 權限對應

### access-control

- project 成員權限
- 操作可見性與可執行性控制

### secrets

- 管理外部系統憑證
- 儲存 AI、repository、ticket provider 的敏感資訊
- 保持金鑰只在後端管理

## 2. Integration Hubs

此層負責把外部系統抽象成平台內的標準化能力。

### ticket

- 對外串接 Jira、GitHub、GitLab 等工作系統
- 將外部 ticket 轉成平台內可理解的工作物件
- 承接定案後的開票流程

### repository

- 對外串接 GitHub、GitLab、Bitbucket 等儲存庫
- 提供 repository 設定、綁定與後續 Git 流程上下文

### ai

- 對外串接 Claude、Gemini、Copilot 等 AI provider
- 將 AI 能力整理成平台內的能力層
- 支援多 AI 設定與後續自動化能力

## 3. Collaboration Runtime

此層承接發想、草稿、同步與互動體驗。

### room

- 多人互動發想空間
- 支援人與 AI 共同參與
- 維持討論、草稿與決策的上下文

### draft

- proposal / design / spec / tasks 草稿管理
- 保存發想階段內容
- 作為定案前的工作區

### realtime

- 即時同步事件
- room 內成員互動更新
- 支援多人協作與後續狀態廣播

### editor

- Web 編輯器封裝
- 第一版候選為 Monaco
- 需保留 editor abstraction 與 adapter 能力

## 4. Execution Layer

此層承接定案後的正式流程。

### decision

- 人工確認定案
- 將發想內容轉成正式輸入

### ticket-creation

- 依 project 設定向外部系統開票
- 產生 ticket 並回寫平台上下文

### openspec

- 後續 OpenSpec 流程與驗證
- 承接規格文件與相關操作

### git-flow

- repository 綁定後的 Git 流程
- 與後續 OpenSpec / automation 串接

### automation

- 定案後的自動化流程
- 包含後續 AI、自動驗證、外部同步等能力

## 5. Application UI

此層是使用者直接操作的工作台。

### project-settings-ui

- 專案設定頁
- 設定 ticket、repository、AI、automation

### ideation-room-ui

- room 對話與草稿畫面
- AI 協作入口

### ticket-workbench-ui

- 開票結果與後續流程工作台
- 顯示 ticket、repository、automation、OpenSpec 狀態

## 模組原則

- 一個模組聚焦一個主要責任
- provider-specific 邏輯不得散落在各模組內部
- 外部系統需透過 adapter 接入
- UI、domain、integration、execution 應有清楚邊界
