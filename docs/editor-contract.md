# Editor Contract

本文件用於描述 `sdd-board` 內部對 Web 編輯器的抽象能力，確保第一版使用 Monaco 時仍保留未來替換空間。

## 目標

- 第一版可採用 Monaco
- 平台邏輯不直接耦合 Monaco API
- 保留未來切換 CodeMirror 或其他 editor 的空間

## 核心能力

平台內部應依賴下列能力，而不是依賴特定編輯器 API：

- `loadContent`
- `updateContent`
- `setReadonly`
- `setDiagnostics`
- `showSuggestions`
- `getSelection`
- `onContentChange`

## 概念資料結構

### Content

- 當前編輯內容
- 可能對應 proposal、design、spec、tasks

### Diagnostics

- error
- warning
- info

### Suggestions

- AI autocomplete
- AI rewrite suggestion
- editor assistance

### Selection

- 使用者當前選取範圍
- 作為 AI 補全或針對性操作的上下文

## 設計原則

- 平台內部永遠先處理標準化資料結構
- editor adapter 負責將標準化資料轉成實際 editor API
- AI suggestion 與 validation diagnostics 均不應直接綁死 Monaco 專屬格式

## 第一版原則

- 可先實作 `monaco-editor-adapter`
- 同時保留 `codemirror-editor-adapter` 的擴充位置

## 一句話總結

> Editor 在 `sdd-board` 中是可替換的工作台元件，平台依賴的是編輯能力，不是 Monaco 這個品牌本身。
