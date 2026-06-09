# 目錄結構藍圖

本文件描述 `sdd-board` 在目前規劃下建議採用的專案目錄結構。

## 規劃方向

- 先以輕量化單體為主
- 保持模組化
- 保留未來拆分與擴充空間

## 建議結構

```text
sdd-board/
├─ app/
├─ components/
├─ modules/
├─ lib/
├─ adapters/
├─ prisma/
├─ public/
├─ docs/
├─ .github/
├─ CLAUDE.md
├─ GEMINI.md
└─ README.md
```

## 各目錄角色

### app/

- Next.js app router 或頁面入口
- 放置 page、layout、route handler 等框架層內容

### components/

- 共用 UI 元件
- 純畫面性質元件
- 不放複雜商業邏輯

### modules/

- 依產品領域拆分模組
- 建議模組：

```text
modules/
├─ project/
├─ room/
├─ ticket/
├─ repository/
├─ ai/
├─ automation/
└─ openspec/
```

每個模組內可再分為：

```text
<module>/
├─ ui/
├─ application/
├─ domain/
├─ infrastructure/
└─ tests/
```

### lib/

- 跨模組共用的工具與基礎能力
- 例如：
  - env
  - logger
  - errors
  - shared types

### adapters/

- 對外系統整合
- 建議依 provider 或類型切分

```text
adapters/
├─ ticket/
│  ├─ jira/
│  ├─ github/
│  └─ gitlab/
├─ repository/
│  ├─ github/
│  ├─ gitlab/
│  └─ bitbucket/
├─ ai/
│  ├─ claude/
│  ├─ gemini/
│  └─ copilot/
└─ editor/
   ├─ monaco/
   └─ codemirror/
```

### prisma/

- schema
- migration
- seed
- 資料存取相關設定

### docs/

- 產品、架構、流程、規範與候選技術文件

### .github/

- Copilot 指引
- workflow
- 其他自動化與協作設定

## 結構原則

- 框架層與業務層分開
- 共用工具與業務模組分開
- provider adapter 與 domain 邏輯分開
- editor、database、AI、ticket、repository 都預留可替換空間

## 第一版建議

第一版可先用單一 Next.js 專案承接所有功能，但仍維持上述模組結構，避免未來擴充時大幅重整。
