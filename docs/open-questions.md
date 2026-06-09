# 待定問題

本文件整理 `sdd-board` 目前規劃中**尚未最終定案**、但後續實作前需要持續追蹤的問題。

## 1. Editor abstraction 的細節界面

雖然第一候選是 Monaco，且已確立要保留可替換性，  
但平台內部 editor contract 的最終欄位與事件格式仍待細化。

## 2. 第一版資料庫是否維持 SQLite

目前第一候選是 SQLite，  
但是否在第一版就切到 PostgreSQL，仍需視實作複雜度與協作需求評估。

## 3. Ticket provider 第一階段先落哪一家

雖然規劃上支援 Jira、GitHub、GitLab，  
但第一階段應先以哪一個 provider 作為實作起點，仍待收斂。

## 4. Repository provider 第一階段先落哪一家

GitHub、GitLab、Bitbucket 都已納入整合視野，  
但第一階段應先從哪一個 repository provider 落地，仍待決定。

## 5. AI provider 的能力分工

目前已規劃多 AI 設定與可替換性，  
但 chat、generation、autocomplete、summary 等能力要如何映射到不同 provider，仍待細化。

## 6. Decision review 的畫面與確認粒度

定案必須是明確操作，  
但第一版是單一步驟確認，還是分 proposal/design/tasks 多段確認，仍待收斂。

## 7. Ticket 建立後的 repository 綁定策略

ticket 產生後，是立即要求綁定 repository，  
還是允許稍後再補，仍待定義。

## 8. 自動化觸發粒度

目前已確立自動化在定案後才啟動，  
但第一版哪些步驟採同步、哪些採背景流程，仍待決定。

## 9. OpenSpec 後續流程與平台邊界

平台會接 OpenSpec 後續流程，  
但哪些動作留在 Web 端、哪些保留 CLI bridge 概念，仍待細化。

## 一句話總結

> `sdd-board` 目前已收斂主方向，但在 editor contract、provider 先後順序、AI 分工、定案粒度與後續流程邊界上，仍保留後續決策空間。
