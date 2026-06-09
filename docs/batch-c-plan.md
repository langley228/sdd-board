# Batch C 規劃

本文件用來收斂 `sdd-board` 第三批實作主線，也就是在核心主線與體驗支撐完成後，逐步補上平台擴充能力。

## 對應 Change

- `sc-005-ticket-provider-adapter`
- `sc-008-repository-binding`
- `sc-009-repository-provider-adapter`
- `sc-010-automation-runtime`
- `sc-011-openspec-flow-bridge`

## 目標

Batch C 的目標是讓平台從「有主線的產品」成長為「可逐步接上外部工程流程的平台」。

## Batch C 不做什麼

- 不追求一次把所有 provider 做滿
- 不追求完整企業級流程引擎
- 不追求所有 OpenSpec 後續能力一次到位

## 1. sc-005-ticket-provider-adapter

### 目標

把 ticket provider 差異收斂到 adapter 層。

### 第一版最低範圍

- ticket provider contract
- 第一落地 provider adapter
- create ticket mapping
- 基本錯誤分類

### 成功條件

- Ticket 流程不再直接綁死單一 provider
- 後續新增第二個 provider 時主要改動集中在 adapter 層

## 2. sc-008-repository-binding

### 目標

在 ticket 之後引入 repository 上下文。

### 第一版最低範圍

- ticket 與 repository 的綁定關係
- repository settings 最小欄位
- repository context 顯示與保存

### 成功條件

- ticket 後可接續進入 repository 流程
- project 能提供基本 repository 設定

## 3. sc-009-repository-provider-adapter

### 目標

把 repository provider 差異收斂到 adapter 層。

### 第一版最低範圍

- repository provider contract
- 第一落地 repository provider adapter
- repository 查詢與 context 準備能力

### 成功條件

- 平台內部不直接依賴某家 repository API 形狀
- 後續可擴 GitHub / GitLab / Bitbucket

## 4. sc-010-automation-runtime

### 目標

建立定案後的自動化推進層。

### 第一版最低範圍

- automation settings 最小欄位
- automation state
- ticket 後的少量自動化觸發

### 第一版原則

- 只處理定案後流程
- 不回頭侵入 room / decision 階段

### 成功條件

- 自動化有狀態可追蹤
- 自動化可開關、可配置

## 5. sc-011-openspec-flow-bridge

### 目標

將 ticket / repository 後續串到 OpenSpec 流程。

### 第一版最低範圍

- 平台與 OpenSpec 的 bridge 邊界
- 後續 OpenSpec 流程入口
- 基本上下文承接

### 成功條件

- 平台主線與 OpenSpec 後續流程能接上
- 不直接把 Web 平台與 OpenSpec 規則引擎耦死

## Batch C 的依賴順序

```text
sc-005
sc-008 -> sc-009
sc-004 + sc-006 + sc-008 -> sc-010
sc-008 + sc-010 -> sc-011
```

## Batch C 待進一步收斂的點

- 第一個 ticket provider
- 第一個 repository provider
- repository 綁定是在開票後立即進行，還是允許延後
- automation 哪些步驟第一版要做成同步
- OpenSpec bridge 第一版只做入口還是做更多流程承接

## 一句話總結

> Batch C 的目標，是在不破壞主線清晰度的前提下，讓 `sdd-board` 逐步接上 repository、automation 與 OpenSpec 的後續平台能力。
