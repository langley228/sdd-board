# 階段性路線圖

本文件整理 `sdd-board` 目前規劃下的分階段推進方向，作為後續提案與實作拆分的參考。

## 路線圖原則

- 先建立清楚主流程
- 先讓核心體驗可用，再補強整合與自動化
- 先輕量落地，再逐步擴大能力範圍

## Phase 0：文件與方向整理

### 目標

- 固定產品定位
- 固定主流程
- 建立開發文件與規範

### 目前已涵蓋

- README
- docs/
- coding style
- branch / commit 規範
- 技術第一候選

## Phase 1：Project 與 Room 基礎

### 目標

- 建立 project 基礎結構
- 建立 project settings 基本頁面
- 建立 room 與 draft 的基本互動

### 核心成果

- project 建立 / 選擇
- room 畫面雛形
- draft 基本儲存
- 基本 AI 互動入口

## Phase 2：定案與開票流程

### 目標

- 建立 decision review
- 建立定案後開票流程
- 建立 ticket workbench 基本上下文

### 核心成果

- confirm decision
- create ticket
- ticket provider 抽象第一版
- ticket 與 project / room 關聯

## Phase 3：Repository 與後續流程

### 目標

- 引入 repository 設定與綁定
- 建立後續 OpenSpec / Git 流程上下文

### 核心成果

- repository settings
- repository binding
- repository adapter 第一版

## Phase 4：Automation 擴充

### 目標

- 建立定案後流程推進能力
- 讓 AI 與後續流程自動化更完整

### 核心成果

- automation settings
- automation state tracking
- AI 後續摘要 / 補齊
- 外部流程同步

## Phase 5：穩定化與擴充

### 目標

- 補齊測試
- 優化安全邊界
- 評估 DB、部署、分層與可擴充性調整

### 核心成果

- 更完整測試覆蓋
- adapter 補強
- 安全與權限強化
- 視需要調整資料庫或執行架構

## 一句話總結

> `sdd-board` 的推進順序應是：先把 project、room、decision、ticket 這條主線做出來，再往 repository、automation 與平台擴充能力延伸。
