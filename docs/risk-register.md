# 風險清單

本文件整理 `sdd-board` 目前規劃階段可預見的主要風險，用於後續設計與實作時持續追蹤。

## 1. 範圍膨脹

### 說明

專案同時涵蓋：

- Web 工作台
- 多專案設定
- AI 協作
- ticket provider 整合
- repository provider 整合
- OpenSpec 後續流程
- automation

若沒有清楚階段切分，很容易過早膨脹。

### 因應方向

- 先守住 `project -> room -> decision -> ticket` 主線
- 把 repository、automation、provider 擴充分階段推進

## 2. Provider 差異擴散

### 說明

Jira、GitHub、GitLab、Bitbucket、Claude、Gemini、Copilot 的差異若直接滲入 domain，會使核心模型快速混亂。

### 因應方向

- 嚴格要求 adapter 層吸收差異
- 核心系統只依賴標準化能力

## 3. Editor 綁死風險

### 說明

若第一版以 Monaco 實作時沒有先建立 abstraction，之後將很難替換編輯器。

### 因應方向

- 先定義 editor contract
- 讓 AI suggestion、diagnostics、selection 等能力走平台介面

## 4. Database 遷移成本

### 說明

第一版若採 SQLite，但資料存取層過度耦合，後續改用其他資料庫成本會大幅上升。

### 因應方向

- 維持 data access abstraction
- 避免過早依賴單一資料庫特性

## 5. AI 邊界失控

### 說明

若 AI 上下文、遮罩與 provider routing 沒有先定好，容易造成資料外送範圍失控或流程語意混亂。

### 因應方向

- project-level AI settings
- prompt context boundary
- masking policy

## 6. 定案邏輯模糊

### 說明

若 room、draft、decision 之間的語意沒切清楚，系統會很快混淆「討論中」與「已確認」的內容。

### 因應方向

- decision 必須是顯性動作
- 定案前後資料模型分開

## 7. 自動化介入過早

### 說明

若在發想或定案前就讓自動化大量介入，可能破壞產品主軸並讓外部系統充滿噪音。

### 因應方向

- 自動化只定位在定案後
- 所有自動化都受 project settings 控制

## 8. 文件與實際方向脫節

### 說明

若後續討論與實作前進，但 README 與 docs 未同步更新，團隊很快會失去共同語意。

### 因應方向

- 將 README 與 docs 視為持續維護資產
- 重要方向變更時同步更新文件

## 一句話總結

> `sdd-board` 目前最大的風險不是單一技術選型，而是範圍膨脹、provider 差異擴散、定案語意模糊與可替換原則失守。
