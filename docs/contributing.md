# 貢獻流程

本文件整理 `sdd-board` 在目前規劃階段建議採用的協作與提交流程。

## 核心原則

- 先理解文件與方向，再開始變更
- 一個 branch 聚焦一個主題
- 一次變更聚焦一個清楚目標
- 文件與程式規範變動應同步更新

## 建議流程

```text
閱讀 README 與 docs
  ↓
確認要處理的主題
  ↓
建立 branch
  ↓
進行變更
  ↓
補齊測試 / 文件
  ↓
整理 commit
```

## 開始變更前

建議先閱讀：

- `README.md`
- `docs/product-overview.md`
- `docs/development-flow.md`
- `docs/coding-standards.md`

若要修改架構或整合方向，也應先參考：

- `docs/architecture-overview.md`
- `docs/adapter-guidelines.md`
- `docs/security-policy.md`

## Branch 原則

請遵守 `docs/coding-standards.md` 中的 branch 規範。

重點：

- `feature/`：新功能
- `fix/`：修正
- `docs/`：文件
- `refactor/`：重構
- `chore/`：維護
- `test/`：測試

## Commit 原則

請遵守 `docs/coding-standards.md` 中的 commit 規範。

建議格式：

```text
<type>(<scope>): <summary>
```

## 文件同步原則

以下情況應同步更新相關文件：

- 產品流程變更
- 架構原則變更
- 技術候選變更
- coding style / branch / commit 規範變更

## 測試原則

任何程式變更都應考慮對應測試邊界：

- 核心邏輯：單元測試
- API 邊界：API / 整合測試
- 主流程：E2E 測試

詳見 `docs/testing-policy.md`。

## 一句話總結

> `sdd-board` 的貢獻流程應以「先對齊文件、再聚焦變更、同步維護規範與測試」為原則。
