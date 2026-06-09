# Batch B 規劃

本文件用來收斂 `sdd-board` 第二批實作主線，也就是在 Batch A 跑通後，補上體驗支撐層：

```text
AI Provider Hub
  +
Editor Abstraction
```

## 對應 Change

- `sc-006-ai-provider-hub`
- `sc-007-editor-abstraction`

## 目標

Batch B 的目標不是擴大產品主流程，而是補強 Batch A 的核心體驗：

1. 讓 AI 能力不只是臨時串接，而是成為平台內的標準化能力層
2. 讓 Web 編輯器不只是可用，而是可替換、可承接後續 AI 與 diagnostics

## Batch B 不做什麼

- 不急著做完整多 provider routing 策略
- 不急著支援所有 AI 能力類型
- 不急著實作第二套 editor
- 不急著做進階 editor 協作機制

## 1. sc-006-ai-provider-hub

### 目標

把 AI 串接從單次實作提升為平台能力層。

### 第一版最低範圍

- project AI settings 基本欄位
- 一組標準化 AI capability
- 一個第一落地 AI provider
- room 中可用的 AI chat / generation 能力

### 第一版建議 capability

- `chat`
- `generateDraft`
- `summarize`

### 成功條件

- AI 可透過 project 設定決定是否啟用
- AI 能力以平台語意對外暴露
- room 不直接依賴單一 provider SDK 細節

## 2. sc-007-editor-abstraction

### 目標

建立可替換的 Web 編輯器層。

### 第一版最低範圍

- Monaco 接入
- editor contract 第一版
- content / diagnostics / suggestions / selection 抽象
- room 草稿區可由 editor abstraction 驅動

### 第一版原則

- 先不做第二個 editor adapter
- 先確保平台內部不直接耦合 Monaco API

### 成功條件

- room / draft 能透過 editor contract 工作
- AI suggestion 與 validation diagnostics 有標準化承接點
- 未來更換 editor 時有清楚替換邊界

## Batch B 與 Batch A 的關係

```text
Batch A 先跑通主線
  ↓
Batch B 補強主線體驗
```

也就是：

- Batch A 決定產品有沒有靈魂
- Batch B 決定產品好不好用、後面好不好擴

## Batch B 待進一步收斂的點

- 第一個 AI provider 先選哪個
- AI capability 與 provider 的第一版映射
- editor contract 的最小欄位清單
- diagnostics 與 suggestions 的標準格式

## 一句話總結

> Batch B 的目標，是讓 `sdd-board` 的主線不只可跑，還具備可延展的 AI 與 editor 工作台基礎。
