# 直播助手 PC 端 Design System 規範

產出日期：2026-06-29  
來源：Figma node JSON `13:8209`、Variables 截圖、既有 Claude 報告  
狀態：第一版正式規範草案，可給 dev 參考，也可作為 AI 設計稿檢查規則基礎。

## 1. 規範結論

這份設計系統已具備可落地的基礎：色彩變數、主要元件、variants、typography、spacing、radius 都能從 Figma node 取得。現在最重要的是把 Figma 原始命名轉成穩定的 semantic token，並把每個 component 的狀態矩陣與檢查規則機器化。

## 2. Source Of Truth

- `design-tokens.normalized.json`：token source of truth，含 primitive / semantic / raw exception。
- `design-components.json`：component anatomy、variants、states、layout summary。
- `design-rules.json`：AI 自動掃描設計稿時使用的規則。
- `visual-guideline.html`：給團隊看的視覺化文件。

## 3. Token 分層

### Primitive Token

Primitive token 保存原始色值、字體、尺寸，例如 `color.primitive.purple.500 = #CB8FE9`。它不應直接被一般畫面使用，除非是在建立 semantic token。

### Semantic Token

Semantic token 是工程與 AI 檢查主要使用的層級，例如：

| Semantic token | Figma variable | 用途 |
|---|---|---|
| `color.text.primary` | Text/WT | 深色背景上的主要文字 |
| `color.text.secondary` | Text/Secondary text | 次要文字 |
| `color.surface.panel` | background/板塊 | 玻璃面板背景 |
| `color.border.subtle` | Border/Card | 卡片與面板細邊 |
| `color.action.brand.subtle` | Button/Button01 | 品牌弱底色操作 |
| `color.input.background` | Input/input | 輸入框背景 |

### Component Token

Component token 應只用在特定元件，例如 `component.liveInfoCard.width = 277px`、`component.liveInfoCard.cover.size = 80px`。這些不應升級成全域 spacing token。

## 4. 色彩規則

透明色必須拆成 base color 與 opacity。  
例如 `background/板塊` 不只記成 `rgba(138,138,138,0.10)`，而是：

```json
{ "base": "#8A8A8A", "opacity": 0.1, "css": "rgba(138, 138, 138, 0.10)" }
```

這樣 AI 才能判斷 Figma 的 `#8A8A8A 10%` 和 CSS 的 `rgba(138,138,138,.1)` 是同一個 token。

## 5. Typography 規則

預設中文使用 PingFang HK，簡中使用 PingFang SC，英文數字使用 Inter。

| Token | 用途 |
|---|---|
| `typography.body.md` | 預設內容文字 |
| `typography.body.sm` | 次要標籤、小型列表內容 |
| `typography.caption` | meta 資訊 |
| `typography.micro` | 僅限非關鍵 badge，需 accessibility review |

## 6. Spacing 與 Radius 規則

全域 spacing 建議使用 `2, 4, 6, 8, 12, 16, 20`。  
`1, 3, 7, 9, 10` 目前標記為 raw/component exception，不應任意重用。

預設 controls 使用 `radius.lg = 8px`；大型面板與 popup 使用 `radius.xl = 12px`；pill 使用 `radius.pill = 999px`。

## 7. Component 規範

本次從 Figma node 解析出 36 組 Component Set。每個元件都已在 `design-components.json` 中包含：

- node id
- variant properties
- states
- variants
- base size
- radius
- spacing
- common typography
- token mapping
- status

重要元件包含：

- Primary Button - Large
- Secondary Button - Large
- Primary Button - Medium
- 常態資訊 Input
- 主播回覆用 Input
- 直播信息
- 直播画面管理
- 直播類型 Button
- Switch / Radio
- Icon system
- 上传的素材

## 8. AI 檢查規則

AI 檢查時應使用 `design-rules.json`，不要只看 HTML。檢查順序：

1. 先判斷 node 是否屬於已登錄 component。
2. 比對 component variant/state 是否存在。
3. 比對 size、padding、gap、radius、typography、fill、stroke。
4. 若值不在 token 中，標記為 `warning` 或 `error`。
5. 若是 raw exception，確認是否只出現在允許的 component part。
6. 若互動元件缺少 focus / disabled / hover / active 狀態，標記為 error。

## 9. 目前待補

- 需要用含 `file_variables:read` scope 的 Figma token 補抓 variables JSON。
- `Button01 / Button02 / Button03 / Button04` 需要設計 owner 確認正式用途。
- `Property 1` 這類 variant 命名需要改成語意命名，例如 `state`、`type`、`selected`。
- Focus-visible 規則尚未完整出現在 Figma，需要補入元件規範。
- 8px micro text 需要 accessibility review。
