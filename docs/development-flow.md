# 開發流程

## 主流程

```text
建立/選擇專案
   ↓
多人互動發想（人 + AI）
   ↓
人工確認定案
   ↓
開票
   ↓
產生 ticket
   ↓
串接 repository / automation
   ↓
OpenSpec / Git / 外部系統後續流程
```

## 階段說明

### 1. 建立/選擇專案

專案是流程的起點。每個專案會先定義：

- ticket 系統設定
- repository 設定
- AI 設定
- automation 規則

### 2. 多人互動發想

在 room 中由人與 AI 一起討論、整理、補盲點、生成草稿。

這個階段聚焦於：

- 想法收斂
- proposal / design / tasks 草稿
- AI 問答、補全、摘要

### 3. 人工確認定案

在平台內將發想內容轉成可執行決策。此時應確認：

- proposal 是否成立
- design 是否接受
- tasks 是否可執行

### 4. 開票

定案後直接向外部系統開票。

```text
定案內容
   ↓
依專案設定選擇 ticket provider
   ↓
Jira / GitHub / GitLab 開票
   ↓
產生 ticket
```

### 5. 後續流程

ticket 產生後，才進入：

- repository 串接
- OpenSpec 驗證
- Git 流程
- automation

## 流程原則

- 發想先於開票
- 定案先於 repository 實作
- ticket 是正式工作流程的入口
- AI 在發想階段即參與，不是後補功能
