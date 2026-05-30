# @Doc (AI-Native Semantic Document Notation)

> 專為人類、AI 與編譯器重新構思的「雙線並行」語義節點式 Markdown。

@Doc 是一種 AI 原生的結構化標記語言，旨在通過極度克制的語法槽位，在保持 Markdown 流暢度的同時，為大語言模型（LLM）與前端渲染器提供確定性、低成本、串流安全的雙向溝通橋樑。

---

## 核心設計理念與痛點防禦

1. **確定性語法與符號唯一性 (Deterministic Grammar)**
固定語法槽位，且**方括號 `[]` 擁有全域絕對唯一的語義：內容槽位（Content Slot）**。徹底杜絕字元逃逸地獄（Escaping Hell）。
2. **高階語義與結構權重隔離 (Ontology Separation)**
拒絕 JSX 式的「組件叢林」混亂。文檔骨架由不可分割的 `Core Nodes` 守護，高階組件由 `Semantic Nodes` 封裝，語義角色則作為內部局部槽位（Slots）。
3. **AI 生成 Token 壓縮 (Token Efficiency)**
全面移除冗餘的語法殼（如 Tailwind 的任意值方括號），讓 AI 在流式生成（Streaming）時預測概率最優化，大幅節省大模型的生成成本與解析混亂。
4. **雙線並行轉譯 (Dual-Track Compilation)**
AST 優先（AST-First）。Parser 只負責做純粹的語義與數據提取，將視覺轉譯權交由後端適配器，同時完美相容 Tailwind JIT 與標準 Inline Style。

---

## 節點架構分類 (Node Taxonomy)

@Doc 的核心節點分為兩大陣營，各自遵循嚴格的解析邊界：

### 1. 結構原件 (Core Nodes)

永遠只負責文檔的基礎骨架與數字層級，是不可再分割的原子。

* `布局 / 文本`：`@h1`, `@h2`, `@h3`, `@p`, `@quote`, `@code`
* `數據 / 媒體`：`@list`, `@img`, `@link`

### 2. 語義容器與配置 (Semantic Nodes)

提供高階上下文語義，內部分化為兩種行為模式：

* **行內語義 (Inline Semantic)**：如 `@lang(ja)[日本語]`，轉譯為具備精準語境的行內標籤。
* **區塊元數據 (Block Metadata)**：如 `@seo{...}`，不渲染任何視覺 HTML，直接將配置注入 Host 應用或編譯目標。

---

## 核心語法

```@Doc
@node(修飾符){樣式/元數據}[內容](動作)

```

| 槽位 (Slot) | 語義規則 | 實踐規範 |
| --- | --- | --- |
| **`@node`** | 語義節點類型 | 必須為官方標準庫或 Schema 宣告之合規節點。 |
| **`()`** | 行為 / 修飾符 | 定義節點的變體（如 `primary`, `featured`, 或語言代碼 `zh-tw`）。 |
| **`{}`** | 樣式 / 元數據 | 1. **區塊元數據**：內建標準 JSON / JSON5 / YAML 子集（如 `@seo`）。<br>2. **視覺樣式**：擁抱微型 Tailwind，**嚴禁使用方括號 `-[...]`** 數值一律改用連字號直接串接（如 `w-300px`）。 |
| **`[]`** | 內容槽位 | 唯一的內文邊界。若節點單獨佔據一行且上下有空行，解譯為 Block；前後有文字，則強制解譯為 Inline。 |
| **`()` 尾綴** | 動作 / 引用 | 行為觸發器（如 `submit`, `install`）或遠端數據參照。 |

---

## 語法範例

### 基礎與高階語義混排文檔

```@Doc
@seo {
  "title": "@Doc 2026 Spec",
  "description": "AI-native semantic document runtime"
}

@h1[@Doc 專案規範]

這是普通段落，其中包含行內語義節點：這是 @lang(ja)[日本語] 的展現。

@card(featured){w-300px bg-f8f9fa text-sm}[
  @title[AI 原生語言]
  @text[
    具有確定性語法的結構化 Markdown，專為雙向 AST 設計。
  ]
  @btn(primary)[立即開始](install)
]

@table[
  @cols[id,name,price]

  @data[
    [1,早餐,60]
    [2,午餐,80]
    [3,晚餐,90]
  ]
]

```

> **注意：** `@title` 與 `@text` 在此處並非 Root 層級的獨立節點，而是 `@card` 容器內部合法的**局部語義槽位（Slots）**，成功規避了語義層級塌陷風險。

---

## 規範化 AST 優先映射 (Canonical AST)

每個語法結構都能無損、清晰地映射到規範的抽象語法樹。在解析 `{style}` 時，Parser 會自動將樣式分類為靜態類名（Static）與自定義動態屬性（Dynamic）。

**範例輸入：**

```@Doc
@btn(primary){text-lg w-120px bg-fff}[確認](submit)

```

**↓ Parser 輸出之規範 AST ↓**

```json
{
  "type": "button",
  "modifier": ["primary"],
  "attributes": {
    "styles": {
      "static": ["text-lg"],
      "dynamic": [
        { "prop": "w", "value": "120px" },
        { "prop": "bg", "value": "fff" }
      ]
    }
  },
  "content": ["確認"],
  "action": "submit"
}

```

---

## 雙線並行轉譯管道 (Dual-Track Compilation)

基於上述規範 AST，後端渲染器（Renderers）可啟動物理分流轉譯：

### 路線 A：Tailwind JIT 適配器（高效 Web 端）

後端適配器遍歷 `styles` 物件，自動幫動態值「補殼還原」，完美觸發 Tailwind CSS 的 JIT 編譯：

* `static` 保持不變 ➔ `text-lg`
* `dynamic` 自動還原括號 ➔ `w-120px` 轉譯為 `w-[120px]`；`bg-fff` 偵測為顏色字串自動補井號 ➔ `bg-[#fff]`
* **最終 HTML 輸出：**
```html
<button class="text-lg w-[120px] bg-[#fff] ...">確認</button>

```



### 路線 B：Universal Inline Style 適配器（跨平台/無封裝環境）

當遇到老舊前端、Email 渲染、或原生 Native App 視圖時，動態值直接翻譯成標準的 CSS 行內鍵值對：

* `dynamic` 轉譯 ➔ `width: 120px; background-color: #fff;`
* **最終 HTML 輸出：**
```html
<button class="text-lg" style="width: 120px; background-color: #fff;">確認</button>

```



---

## 目標與現狀

* **相容性**：保持對標準 Markdown 基礎結構的雙向無損轉換。
* **防禦力**：全面凍結 `!@define` 巨集系統的動態編程特性，第一階段僅支援靜態 JSON Schema 映射，防止演變為危險的 Programmable Runtime。

## 授權

MIT
