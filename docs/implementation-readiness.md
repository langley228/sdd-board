# 實作前檢查表

本文件用來確認 `sdd-board` 是否已具備進入提案或實作前整理的條件。

## 已具備

- 產品定位已收斂
- 主流程已收斂
- 模組規劃已建立
- 技術第一候選已整理
- coding style / branch / commit 規範已整理
- 安全、測試、adapter 原則已建立
- UI、資料模型、資料流已有概覽文件

## 仍可在實作前再收斂

- 第一個 ticket provider
- 第一個 repository provider
- AI provider 能力映射細節
- editor contract 細節
- SQLite 是否維持為第一版資料庫

## 建議進入下一階段前確認

### 1. 主流程是否固定

應確認：

```text
project
  -> room
  -> decision
  -> ticket
```

是否已作為第一階段不可動搖的主線。

### 2. 第一波 provider 是否收斂

至少確認每一類外部能力的第一落地對象：

- ticket
- repository
- AI

### 3. 文件是否足夠支持實作

至少應已可回答：

- 模組怎麼切
- API 怎麼命名
- editor 怎麼抽象
- provider 怎麼接
- 測試邊界怎麼守

## 一句話總結

> 目前文件已足夠支撐「開始提案」與「進入實作前設計整理」，但在第一波 provider 與少數 contract 細節上仍可再收斂。
