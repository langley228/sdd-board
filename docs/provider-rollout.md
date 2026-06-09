# Provider 落地順序

本文件整理 `sdd-board` 在外部 provider 接入上的建議落地順序，避免同時攤開所有整合而失焦。

## 核心原則

- 先做抽象，再選第一個 provider 落地
- 一類能力先落一個 provider 即可驗證架構
- 後續擴 provider 應以 adapter 新增為主，而不是重寫主流程

## Ticket Provider

### 規劃範圍

- Jira
- GitHub Issues
- GitLab Issues

### 建議順序

1. 選一個第一落地 provider
2. 驗證 `createTicket` 與基本 mapping
3. 再擴第二個 provider

## Repository Provider

### 規劃範圍

- GitHub
- GitLab
- Bitbucket

### 建議順序

1. 先落一個 repository provider
2. 驗證 repository binding 與 context 準備
3. 再擴其他 provider

## AI Provider

### 規劃範圍

- Claude
- Gemini
- Copilot

### 建議順序

1. 先落一個 chat / generation provider
2. 再補 autocomplete provider
3. 再做多 provider routing

## Editor Provider

### 規劃範圍

- Monaco
- CodeMirror

### 建議順序

1. 先落 Monaco
2. 先守住 abstraction
3. 之後有需求再補第二個 editor adapter

## 一句話總結

> 外部 provider 的落地應採「每類能力先做一個、先驗證抽象，再逐步擴充」的策略。
