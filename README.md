# sdd-board

sdd-board 是一個把 OpenSpec 從 CLI 擴展到 Web 的多專案協作平台。

核心流程：

```text
建立/選擇專案
   ↓
多人互動發想（人 + AI）
   ↓
人工確認定案
   ↓
開票
   ↓
產生 ticket
   ↓
串接 repository / automation
   ↓
OpenSpec / Git / 外部系統後續流程
```

## 目前方向

- 以專案為流程起點
- 發想階段支援多人協作與 AI 串接
- 定案後直接向外部系統開票產生 ticket
- 後續再銜接 repository、OpenSpec、Git 與 automation
- 外部 ticket、repository、AI provider 都採抽象化設計

## 文件

- [產品概覽](docs/product-overview.md)
- [架構概覽](docs/architecture-overview.md)
- [開發流程](docs/development-flow.md)
- [模組規劃](docs/module-plan.md)
- [目錄結構藍圖](docs/project-structure.md)
- [專案設定模型](docs/project-settings.md)
- [技術候選](docs/tech-options.md)
- [畫面藍圖](docs/ui-blueprint.md)
- [資料模型概覽](docs/data-model-overview.md)
- [外部整合矩陣](docs/integration-matrix.md)
- [自動化流程概覽](docs/automation-flow.md)
- [名詞定義](docs/glossary.md)
- [階段性路線圖](docs/roadmap.md)
- [貢獻流程](docs/contributing.md)
- [API 設計約定](docs/api-conventions.md)
- [程式規範](docs/coding-standards.md)
- [測試原則](docs/testing-policy.md)
- [安全原則](docs/security-policy.md)
- [Adapter 設計準則](docs/adapter-guidelines.md)

## 開發約定

- 程式撰寫風格、命名、分層、adapter 原則請參考 [程式規範](docs/coding-standards.md)
- branch 命名與 commit 訊息格式已納入 [程式規範](docs/coding-standards.md)
- 目前技術選型仍以第一候選為主，保留後續替換與調整空間

## 目前狀態

目前專案仍處於規劃與文件整理階段。README 與 `docs/` 內容主要用來固定產品方向、架構原則與技術候選，並不代表所有技術選型已最終定案。