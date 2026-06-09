# 架構概覽

## 整體架構

```text
Project Control Plane
├─ Ticket Settings
├─ Repository Settings
├─ AI Settings
└─ Automation Settings
        ↓
多人互動發想 Room（人 + AI）
        ↓
人工確認定案
        ↓
開票
        ↓
產生 ticket
        ↓
串接 repository / automation / OpenSpec / Git
```

## 核心模組

### 1. Platform Core

- project / tenant 設定
- auth / access control
- secret / key management

### 2. Integration Hubs

- ticket hub
- repository hub
- ai hub

### 3. Collaboration Runtime

- room
- draft
- realtime sync
- AI assist

### 4. Execution Layer

- ticket creation
- OpenSpec validation
- Git / repository flow
- automation execution

## 可替換原則

### Ticket

ticket 必須是平台內部的標準化工作物件，不能直接綁死某一個外部系統。

### Repository

repository 必須是平台內部的標準化儲存庫概念，避免直接綁死 GitHub、GitLab、Bitbucket 其中之一。

### AI

AI 必須是能力層抽象，而不是單一模型整合。

### Editor

Web 編輯器必須可替換，第一版可先採用候選方案，但需以 editor abstraction + adapter 模式規劃。

### Database

資料庫需可替換，資料存取層不可直接與單一資料庫耦合。

### Deployment

部署先以可容器化為原則，不預先綁定特定平台。
