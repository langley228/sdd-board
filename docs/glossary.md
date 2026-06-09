# 名詞定義

本文件整理 `sdd-board` 目前規劃中常用的核心名詞，避免後續討論與設計時語意混淆。

## Project

在 `sdd-board` 中，project 是所有流程的起點，也是：

- 設定邊界
- 權限邊界
- 外部系統設定容器
- AI 與 automation 策略來源

## Room

room 是多人互動發想的空間，用來承接：

- 人與 AI 討論
- 草稿整理
- 發想內容收斂

room 不是 ticket，也不是 repository。

## Draft

draft 是定案前的草稿內容容器，可包含：

- proposal
- design
- spec
- tasks

draft 代表尚未正式進入外部流程的內容。

## Decision

decision 是人工確認後的正式決策結果。  
它表示發想內容已從「討論中」進入「可執行」狀態。

## Ticket

ticket 是定案後由平台向外部工作系統建立的正式工作項目。

ticket 的來源可能是：

- Jira
- GitHub Issues
- GitLab Issues

在平台內，ticket 應以標準化工作物件看待，而不是直接等同某一家 provider 的資料格式。

## Repository

repository 是定案後要串接的儲存庫上下文。  
其來源可能是：

- GitHub
- GitLab
- Bitbucket

在平台內，repository 應視為標準化能力，而不是特定平台名稱。

## AI Provider

AI provider 指外部模型服務提供者，例如：

- Claude
- Gemini
- Copilot

在平台內應優先看待其能力，例如 chat、generation、autocomplete，而不是單一品牌名稱。

## Automation

automation 指定案後，根據 project settings 自動推進後續流程的能力，例如：

- 自動開票後續同步
- repository 準備
- OpenSpec / Git 後續流程
- AI 摘要與補齊

automation 不是發想本身，也不取代人工定案。

## Adapter

adapter 是平台接入外部系統時的轉換層。  
其目的是：

- 吸收 provider-specific 差異
- 做資料與錯誤轉換
- 對平台內部提供穩定能力介面

## Editor Abstraction

editor abstraction 指平台內部對 Web 編輯器的統一介面。  
其目的是讓第一版可先使用 Monaco，但未來仍保留替換編輯器的空間。

## 一句話總結

> `sdd-board` 的核心語意應建立在 project、room、draft、decision、ticket 這條主線上，其他外部系統則透過標準化能力與 adapter 接入。
