# @Doc (原為 M@rkdown)

> 專為人類與 AI 重新構思的語義節點式 Markdown。

@Doc 是一種 AI 原生的結構化標記語言，旨在實現：

* **人類可讀性**
* **確定性解析**（Deterministic parsing）
* **AST 優先架構**（AST-first architecture）
* **Token 效率**（節省生成成本）
* **串流安全渲染**（Streaming-safe rendering）
* **語義化文件結構**

與傳統 Markdown 不同，@Doc 引入了使用 `@nodes` 的固定**語義槽位（Semantic Slots）**。

---

## 範例

### 基礎 Markdown

```markdown
# Hello World
這是普通的 Markdown。

```

### @Doc (M@rkdown)

```@Doc
@card(featured){w:full}[
  @title[AI 原生語言]
  @text[
    具有確定性語法的結構化 Markdown。
  ]
  @btn(primary)[立即開始](install)
]

```

---

## 核心語法

`@node(修飾符){樣式}[內容](動作)`

| 槽位 (Slot) | 意義 |
| --- | --- |
| **@node** | 語義節點類型 |
| **()** | 行為 / 修飾符 (Modifier) |
| **{}** | 樣式 / 元數據 (Metadata) |
| **[]** | 內容 |
| **() 尾綴** | 動作 / 引用 (Action / Reference) |

---

## 設計理念

### 1. 確定性語法 (Deterministic Grammar)

固定的語法槽位使解析過程穩定且可預測。

### 2. 人類可讀的壓縮

保持語法精簡，同時不失可讀性。

### 3. AI 原生結構

專為以下需求設計：

* LLM 生成
* 語義解析
* AST 映射
* 串流渲染
* 結構化編輯

### 4. AST 優先 (AST-First)

每個語法結構都能清晰地映射到規範的 AST（抽象語法樹）。

**範例：**
`@btn(primary){#000}[確認](submit)`

**↓ 轉換為 ↓**

```json
{
  "type": "button",
  "modifier": ["primary"],
  "style": {
    "color": "#000"
  },
  "content": "確認",
  "action": "submit"
}

```

---

## 為什麼需要 @Doc？

傳統 Markdown 的設計初衷是：
**人類 ➔ HTML**

@Doc 的設計目標則是：
**人類 ↔ AI ↔ AST ↔ 渲染器**

---

## 目標

* 保持 Markdown 兼容性
* 語義化文件結構
* 確定性解析
* AI 友善的 Token 化（Tokenization）
* 無損 AST 轉換
* 支援增量解析
* 串流安全渲染

---

## 現狀

目前處於早期實驗性設計階段

重點關注領域：

* 語法設計
* AST 規範
* 解析器架構
* Token 優化
* 串流解析
* AI 原生文件工作流

---

## 授權

# MIT
