# 第一階段待決策清單

本文件整理 `sdd-board` 在進入第一階段實作前，仍建議先收斂的幾個關鍵決策。

## 核心原則

- 先做真正會影響 Batch A 的決策
- 不急著把所有未來問題一次定完
- 優先決定會直接影響 `project -> room -> decision -> ticket` 主線的項目

## 1. 第一個 Ticket Provider

### 為什麼重要

`sc-004-ticket-creation` 與 `sc-005-ticket-provider-adapter` 都會受它影響。

### 待決事項

- 第一階段先用 Jira、GitHub Issues，還是 GitLab Issues
- 第一版是否只做 create，不做完整 sync

## 2. 第一個 AI Provider

### 為什麼重要

`sc-002-ideation-room` 與 `sc-006-ai-provider-hub` 的最小形態都會受它影響。

### 待決事項

- 第一階段先用哪一個 AI provider
- 第一版先做 chat 還是同時做 generation

## 3. Decision Review 粒度

### 為什麼重要

`sc-003-decision-gate` 的 UX 與資料模型會直接受影響。

### 待決事項

- 第一版是否只做單一步驟確認
- decision summary 需不需要分 proposal / design / tasks 區塊

## 4. 最小 Project Settings 欄位

### 為什麼重要

`sc-001-project-foundation` 需要知道第一版的 settings 到哪裡為止。

### 待決事項

- 只保留 projectName / projectKey / default providers
- 還是額外先放一部分 repository / automation 欄位

## 5. Room 中 AI 的最小互動型態

### 為什麼重要

會影響 room 畫面設計、AI provider 能力模型與 editor suggestion 介面。

### 待決事項

- 先做 chat 側欄
- 先做 draft assist
- 或兩者都做最小版本

## 6. Ticket Result 畫面的最小範圍

### 為什麼重要

Batch A 結尾不一定要直接進入完整 workbench，但要有明確結果頁。

### 待決事項

- 只顯示 ticket 建立成功結果
- 還是同時帶出後續 repository 入口

## 建議決策順序

```text
1. 最小 Project Settings
2. 第一個 AI Provider
3. Room 中 AI 的最小互動型態
4. Decision Review 粒度
5. 第一個 Ticket Provider
6. Ticket Result 畫面的最小範圍
```

## 一句話總結

> 第一階段不需要把所有未來問題都定完，但至少要先把 Project、Room、Decision、Ticket 這條主線上真正會影響實作的決策拍板。
