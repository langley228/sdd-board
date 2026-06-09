# 畫面藍圖

本文件整理 `sdd-board` 目前規劃下的主要畫面與其角色。

## 主要畫面

```text
1. Project List / Dashboard
2. Project Settings
3. Ideation Room
4. Decision Review
5. Ticket Workbench
```

## 1. Project List / Dashboard

### 目的

- 顯示使用者可存取的 project
- 作為所有流程的入口

### 建議內容

- project 卡片或列表
- project 狀態
- 快速進入 room / settings / ticket workbench

## 2. Project Settings

### 目的

設定 project 的 ticket、repository、AI 與 automation 規則。

### 建議分頁

- Basic
- Ticket
- Repository
- AI
- Automation

## 3. Ideation Room

### 目的

承接多人互動發想與 AI 協作。

### 畫面概念

```text
┌──────────────────────────────────────┐
│ 左：對話 / 訊息流                     │
│ 右：草稿區（proposal/design/tasks）   │
│ 下：AI 動作 / 建議 / 摘要             │
└──────────────────────────────────────┘
```

### 關鍵能力

- 人與 AI 共同討論
- 草稿編輯
- AI 補全 / 摘要 / 建議
- 定案前的工作區

## 4. Decision Review

### 目的

在定案前集中檢查內容是否足以進入正式流程。

### 建議內容

- proposal 摘要
- design 摘要
- tasks 摘要
- 待確認項目
- 確認定案按鈕

## 5. Ticket Workbench

### 目的

承接定案後的正式流程入口。

### 建議內容

- 已建立的 ticket 資訊
- 關聯 repository
- 後續 OpenSpec / Git / automation 狀態
- 相關操作入口

## UI 原則

- project 是入口，不是附屬頁面
- ideation room 是產品核心畫面
- 定案必須是明確操作，不可隱性發生
- ticket workbench 是定案後的正式工作台

## 一句話總結

> `sdd-board` 的 UI 應圍繞「project → room → decision → ticket」這條主流程展開。
