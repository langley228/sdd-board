# Change 規劃

本文件整理 `sdd-board` 目前規劃的 change map，作為後續 proposal、design 與實作拆分的基線。

## Change 編號總覽

| Change | 名稱 | 目的 |
|---|---|---|
| **sc-001-project-foundation** | 建立專案與最小設定基礎 | 建立/選擇 project，作為所有流程起點 |
| **sc-002-ideation-room** | 多人互動發想房間 | 建立 room、draft 與人 + AI 協作空間 |
| **sc-003-decision-gate** | 定案確認機制 | 將 draft 明確轉成 decision |
| **sc-004-ticket-creation** | 定案後直接開票 | 依 project 設定建立 ticket |
| **sc-005-ticket-provider-adapter** | ticket provider 抽象 | 建立 Jira / GitHub / GitLab 的 ticket adapter 基礎 |
| **sc-006-ai-provider-hub** | AI provider 能力層 | 建立多 AI 設定與 AI capability 抽象 |
| **sc-007-editor-abstraction** | Web 編輯器抽象層 | 以 Monaco 為第一版候選，保留可替換性 |
| **sc-008-repository-binding** | repository 綁定與上下文 | 在 ticket 後串接 repository 上下文 |
| **sc-009-repository-provider-adapter** | repository provider 抽象 | 建立 GitHub / GitLab / Bitbucket repository adapter 基礎 |
| **sc-010-automation-runtime** | 定案後自動化流程 | 建立 ticket 後的 automation state 與流程推進 |
| **sc-011-openspec-flow-bridge** | OpenSpec 後續流程橋接 | 將 ticket / repository 後續串到 OpenSpec 流程 |
| **sc-012-platform-hardening** | 平台穩定化與治理 | 安全、測試、權限、容器化與整體補強 |

## 依賴關係

```text
主線骨架
sc-001 -> sc-002 -> sc-003 -> sc-004

體驗支撐
sc-001 -> sc-006
sc-002 -> sc-007

平台擴充
sc-004 -> sc-005
sc-004 -> sc-008 -> sc-009
sc-004 + sc-006 + sc-008 -> sc-010
sc-008 + sc-010 -> sc-011

穩定化
sc-012 橫跨全域
```

## 各 Change Scope

### sc-001-project-foundation

**目的**：建立平台起點。  
**範圍**：

- project 建立 / 選擇
- 最小 project settings
- 基本 project 資料模型
- project 入口頁面

**依賴**：無

---

### sc-002-ideation-room

**目的**：建立發想空間。  
**範圍**：

- room 基本模型
- draft 基本模型
- 多人互動發想基本流
- 人與 AI 共存的 room 上下文

**依賴**：sc-001

---

### sc-003-decision-gate

**目的**：建立定案邊界。  
**範圍**：

- draft → decision 的明確轉換
- decision review / confirm 流程
- 定案狀態與顯性操作

**依賴**：sc-002

---

### sc-004-ticket-creation

**目的**：定案後正式開票。  
**範圍**：

- decision 後 create ticket
- ticket context 回寫
- ticket workbench 初版

**依賴**：sc-003

---

### sc-005-ticket-provider-adapter

**目的**：抽象 ticket provider。  
**範圍**：

- ticket provider contract
- Jira / GitHub / GitLab ticket adapter 基礎
- ticket mapping 與錯誤邊界

**依賴**：sc-004

---

### sc-006-ai-provider-hub

**目的**：抽象 AI 能力層。  
**範圍**：

- project AI settings
- AI capability routing
- chat / generation / autocomplete 能力模型

**依賴**：sc-001、sc-002

---

### sc-007-editor-abstraction

**目的**：建立可替換編輯器層。  
**範圍**：

- Monaco 接入
- editor contract
- diagnostics / suggestions / selection 抽象

**依賴**：sc-002

---

### sc-008-repository-binding

**目的**：把 ticket 接到 repo。  
**範圍**：

- ticket 後 repository 綁定
- repository context
- repository 基礎設定流程

**依賴**：sc-004

---

### sc-009-repository-provider-adapter

**目的**：抽象 repository provider。  
**範圍**：

- repository provider contract
- GitHub / GitLab / Bitbucket repository adapter 基礎
- repository 能力標準化

**依賴**：sc-008

---

### sc-010-automation-runtime

**目的**：建立定案後流程推進層。  
**範圍**：

- automation settings
- automation state
- ticket 後流程觸發

**依賴**：sc-004、sc-006、sc-008

---

### sc-011-openspec-flow-bridge

**目的**：將平台流程接到 OpenSpec。  
**範圍**：

- ticket / repository 後續串接 OpenSpec 流程
- 平台與 OpenSpec 的 bridge 邊界

**依賴**：sc-008、sc-010

---

### sc-012-platform-hardening

**目的**：穩定化與治理。  
**範圍**：

- 安全補強
- 權限補強
- 測試補強
- 容器化與整體穩定化

**依賴**：全域橫跨，建議後期持續進行

## 批次建議

### Batch A：主線骨架

- sc-001
- sc-002
- sc-003
- sc-004

### Batch B：體驗支撐

- sc-006
- sc-007

### Batch C：平台擴充

- sc-005
- sc-008
- sc-009
- sc-010
- sc-011

### Batch D：穩定化

- sc-012

## 目前建議優先順序

第一階段應先守住：

```text
Create Project
  -> Project Settings（最小化）
  -> Room（人 + AI）
  -> Decision
  -> Create Ticket
```

也就是先完成 Batch A，再補 Batch B。

## 一句話總結

> `sdd-board` 的 change 規劃應先以 `project -> room -> decision -> ticket` 為主線，再逐步擴充 AI、editor、repository、automation 與 OpenSpec 能力。
