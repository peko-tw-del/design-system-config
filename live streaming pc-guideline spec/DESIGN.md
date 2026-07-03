# 直播助手 PC 端 Design System 規範

產出日期：2026-06-29  
來源：Figma node JSON `13:8209`、Variables 截圖、既有 Claude 報告  
狀態：第一版正式規範草案，可給 dev 參考，也可作為 AI 設計稿檢查規則基礎。2026-07-03 已重新讀取 Figma node `84:346183` 與 `84:349799` 校正。

## 1. 規範結論

這份設計系統已具備可落地的基礎：色彩變數、主要元件、variants、typography、spacing、radius 都能從 Figma node 取得。現在最重要的是把 Figma 原始命名轉成穩定的 semantic token，並把每個 component 的狀態矩陣與檢查規則機器化。

## 2. Source Of Truth

- `design-tokens.normalized.json`：token source of truth，含 primitive / semantic / raw exception。
- `design-components.json`：component anatomy、variants、states、layout summary。
- `design-rules.json`：AI 自動掃描設計稿時使用的規則。
- `PC_SPEC_SYNTHESIZED.md`：Claude 原 pc spec 與 Figma node 84 綜合比對後的 canonical 判定版。
- `FIGMA_NODE_84_AUDIT.md`：Figma node 84 快速稽核摘要。
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

預設 controls 使用 `radius.lg = 8px`；大型面板與 popup 使用 `radius.xl = 12px`；`radius.xl2 = 16px`（2026-07-02 新增，確認出現在 Default/文字 variant，介於 radius.xl 與 radius.2xl 之間）；pill 使用 `radius.pill = 999px`。

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
4. size、padding、gap、radius 不應出現小數像素；超過 0.01px 浮點容差時標記為 `warning`，icon/vector 內部細節除外。
5. 若值不在 token 中，標記為 `warning` 或 `error`。
6. 若是 raw exception，確認是否只出現在允許的 component part。
7. 若互動元件缺少 focus / disabled / hover / active 狀態，標記為 error。

## 9. 2026-07-02 更新記錄

來源：Figma node `64:333111`（popup 頁面內「添加素材」symbol 集合）。

**新增元件**：「添加素材 - 選項卡」，取代舊版「添加素材 BTN」（單一按鈕，259x36，Default/Hover/Clicked 三態）。新版改為 8 種內容 × hover on/off 的卡片式選項（169x55px，圓角 8px），對應 Figma 命名 `选项=X, Hover=on/off`，命名品質比舊版乾淨。舊版規格保留於 `design-components.json` 中並標記 `deprecated-superseded-2026-07-02`，未直接刪除，正式下架前需 Peko 確認舊版是否還有畫面在用。

**發現的問題（已寫入 design-rules.json 為新規則）**：

1. **Hover 背景漸層未綁 token**：`linear-gradient(138.445deg, rgba(117,69,133,0.35), rgba(75,52,141,0.35))` 是字面值，沒有綁 variable。色值跟 `gradient.action.primary`（#754585 / #4B348D）完全一樣，只是角度（138.445° vs 180°）跟透明度（35% vs 100%）不同，判斷是同源色但沒有正式收斂成 token。已在 `design-tokens.normalized.json` 新增草稿 token `gradient.card.hover.subtle`，狀態 `needs-review`，等 Peko 確認角度要不要統一成 180°、要不要正式收錄。
2. **`Brand/hover` 變數無法解析**：這個節點子樹裡有引用到，但值是空的，也不在既有 token 表任何 figmaName 裡，判斷是沒設定或壞掉的 alias。沒有用猜的方式補值，已列為待確認項目。
3. **缺少 active/focus/disabled 狀態**：目前只定義了 hover on/off，依第 8 節規則第 7 條，互動元件缺狀態要標記 error。
4. **舊頁面實例沒跟上主元件更新**：popup 頁面裡仍有多處用未語意化命名的 `Component 310` / `311` / `312` 承載這組卡片內容，還沒換成新版 master symbol 的正式 instance——跟先前記錄過的「發起PK」標籤跨模組亂用屬於同一種系統性問題（主元件改了、頁面實例沒同步）。

## 10. 2026-07-02（下午）批次更新 — 來源 node 74:333669「07/01 組件修改項目」

這批不是單一元件，是五個各自獨立的更新，其中「上传的素材」跟稍早記錄的「添加素材 - 選項卡」**不是同一個東西、也不是重複**——前者是「已上傳素材列表項目」，後者是「選擇要加入哪種素材的選項卡」，兩個是同一個素材流程裡前後不同階段的元件。

**新增元件（design-components.json 新增 3 組，共 40 組）：**
- **Browser Top**：應用程式視窗頂列（Windows / Mac 兩樣式），新增 `background/Browser Top` (#36373C) 色彩 token；帳號選單新增「檢查更新」選項。
- **Menu / 双击编辑弹窗**：「上传的素材」列表項目雙擊後的音量調整彈窗，內含麥克風bar與百分比數值。
- **下拉选单 (Dropdown Family)**：8 組下拉選單（登入區碼／視頻源／情感／職業／性別／直播地點／來源／日期），先前完全沒收錄，這次補上，新增 Hover 狀態。

**更新既有元件：**
- **button / 主播任務**：variant 屬性從無語意的「Property 1」改成「type」，狀態從已完成/未完成兩態擴充為 Disabled1/Disabled2/Default/Hover/Clicked 五態——這正好回應了本文件最初就記錄的待補事項「Property 1 這類 variant 命名需要改成語意命名」。
- **主播回覆用 Input**：高度 41px → 42px，跟一般 Input 對齊。
- **上传的素材**：選中列新增字面漸層背景，未收斂成 token。

**發現的系統性問題（比單一元件層級更重要）**：

`Brand/hover`、`Brand/Disable`、`Brand/Brand(active)` 三個變數，在 button/主播任務 跟 上传的素材 兩個不相關的元件裡都被引用，但透過 `get_variable_defs` 讀出來全部是空值。加上另外發現的 `板块线框` 變數本身回傳畸形字串 `","`——這已經不是單一元件的個案，是 Figma 檔案裡「Brand/*」這一整組狀態色變數本身壞掉或未發布的跡象，建議 Peko 直接在 Figma 裡稽核這組變數，而不是逐一元件修。已把這條寫成新的 global rule `token.variable.no-broken-reference`。

另外，三個不相關元件（添加素材選項卡 hover、button/主播任務 disabled、上传的素材選中列）各自獨立寫出了「品牌紫漸層在 35% opacity」但用了三種不同角度（138.445°／131.384°／159.263°），代表這是一個真實存在、被多處重複發明的視覺模式，值得正式收斂成一顆 token 並統一角度，而不是繼续讓各元件各寫各的。

## 10.1 更正（同日）

上面第 10 節裡「下拉选单 Dropdown Family」的描述有兩個錯誤，Peko 指出後已修正：

1. **「新增 Hover 狀態」是錯的**。Hover=on/off 這組狀態，在這次「07/01 組件修改項目」出現之前，這份對話一開始掃描 node 64:333497 時就已經存在。是我自己先前沒把它寫進 design-components.json，不是 Figma 新增的功能。07/01 這批到底改了什麼，目前沒有確切證據（annotation 提到「有 active 和 Hover」，但我只驗證到 hover，沒驗證到 active 差異在哪），需要 Peko 補充。
2. **「下拉选单_日期」不是普通下拉選單，是日曆**。內部圖層命名就是「日历」，anatomy 是月份切換＋星期標頭＋日期網格，跟其他 7 個列表型下拉選單完全不同結構，已拆成獨立的「日曆」元件條目。月份標籤用了 Noto Sans SC，是目前 token 表三種字體家族（PingFang HK/SC、Inter）之外的新字體，需要確認要不要正式收錄。陰影效果則跟既有 `shadow.overlay` token 完全吻合，不需要新增。

design-components.json 已同步修正（41 組），錯誤紀錄保留在上方並用此節說明更正過程，沒有直接覆蓋掉。

## 10.2 更正（同日，第二次）

Peko 重新整理後發現「主播回覆用 Input」的修正建議還是顯示 41px。查了之後確認：**我只更新了 `design-components.json`（人看的規格文件），沒有同步更新 `design-rules.json` 裡實際驅動 AI 檢查建議的 componentRules 條目**，兩份資料當時不同步，導致檢查工具吐出來的還是舊值。已修正：

- `13:8927.13:8928.size`、`13:8927.13:8938.size` 兩條規則的 `expectedValue.height` 由 41px 改為 42px。
- 順手一併修正 `button / 主播任務` 的規則 variant 標籤，從舊的「Property 1=未完成/已完成」改成新的「type=Disabled1/Disabled2」，避免規則跟實際圖層命名對不上。
- 全文搜尋過一次確認沒有其他殘留的舊數值（唯一其他出現 "41" 的地方是「141px」寬度，屬於字串誤判，不是真正的殘留錯誤）。

這次的教訓：以後同一批更新要同時檢查 `design-components.json`（規格說明）跟 `design-rules.json`（機器檢查規則）兩份，不能只改一邊。

**追加**：光修 Input 那兩條還不夠，我對這次動過的 9 個元件、以及全部 41 組元件做了一次系統性覆蓋率檢查，發現這批新增的 4 組元件（Browser Top、Menu/双击编辑弹窗、下拉选单家族、日曆）**完全沒有對應的 componentRules**，等於規格寫了但機器檢查完全不會管。已補上 9 條規則（含日曆月份標籤用了未收錄字體 Noto Sans SC 這條，標記 error）。現在 componentRules 總數 274 條、加 global rules 共 283 條，`design-components.json` 全部 41 組元件都至少有一條規則覆蓋，size 數值逐一比對過，沒有再發現落差。README、HTML、Figma 頁面的規則總數也一併同步成 283。

**這個流程以後會固定做**：更新任一元件規格後，一定會 grep `design-rules.json` 裡對應 component 名稱，逐條核對 expectedValue 是否跟新規格一致、以及是否完全沒有規則覆蓋，而不是只驗證 JSON 格式跟看 `design-components.json` 本身有沒有寫對。

## 10.3 2026-07-03 重新讀取 Figma node 84 校正

來源：

- 完整 UI kit：Figma node `84:346183`，名稱 `直播助手PC端_UI kit`
- 最新修改區：Figma node `84:349799`，名稱 `07/01 組件修改項目`

這次讀到的 `84:*` 節點與既有規範內容高度一致，但它是目前連結中實際可追溯的最新版來源；因此後續檢查應優先把 `84:*` 當作最新 Figma 來源，舊的 `13:*` / `64:*` / `74:*` 紀錄保留為歷史對照。

**已確認的正式 token：**

| Token | Figma variable | 值 | 用途 |
|---|---|---|---|
| `color.text.primary` | `Text/WT` | `#FFFFFF` | 深色背景主要文字 |
| `color.text.secondary` | `Text/Secondary text` | `#B8ACBE` | 次要文字 |
| `color.text.secondaryMuted` | `Text/Secondary text 50%` | `#B8ACBE80` | disabled / placeholder 弱文字 |
| `color.text.default` | `Text/Default text` | `#ADAAAA` | Browser Top 標題、一般預設文字 |
| `color.text.icon` | `Text/icon` | `#E0D8F1` | icon / 輔助符號 |
| `color.brand.primary` | `primary/Main` | `#CB8FE9` | 品牌主色、focus border、版本號 |
| `color.bg.primary` | `background/Primary` | `#0B080D` | App / menu 深底 |
| `color.bg.browserTop` | `background/ Browser Top` | `#36373C` | Windows / Mac Browser Top 背景 |
| `color.surface.panel` | `background/板塊` | `#8A8A8A1A` | 面板弱背景 |
| `color.surface.panelHover` | `background/板塊 Hover` | `#8A8A8A33` | 面板 hover |
| `color.input.background` | `Input/input` | `#312744` | 輸入框與選單選項背景 |
| `color.dialog.background` | `彈窗/Background` | `#1B1721` | Dropdown / popup 背景 |
| `color.border.dropdown` | `Border/Drop-down menu` | `#CB8FE980` | Dropdown 外框 |
| `shadow.overlay` | `彈窗shadow` | `0 0 20px #00000080` | popup / overlay |

**仍屬 raw exception 或 broken reference：**

- `Brand/hover`、`Brand/Disable`、`Brand/Brand(active)`：在 node `84:346183` 與 `84:349799` 仍解析為空值，不能當正式 token 使用。
- `板块线框`：仍解析為畸形字串 `","`，應視為 Figma variable 資料壞掉。
- 品牌紫漸層的 raw 角度仍不一致：Primary Button 使用 90deg；主播任務 disabled 使用 131.384deg；上传的素材 selected 使用 159.263deg；添加素材 hover 使用 138.445deg。這些色值同源於 `#754585 → #4B348D`，但目前不是同一顆正式 token。
- Browser Top 帳號 Menu 邊框是 `0.5px white`，還沒綁正式 border token。

**元件校正：**

- `Primary Button - Large`：node `84:346186`，尺寸 `350x48`，radius `12`，狀態 `Default / Hover / Active / Disabled / loading`。Claude 舊版少列 `loading`，已於 2026-07-03 補入 `design-components.json` 與 `design-rules.json`。
- `button / 主播任務`：node `84:349806` 確認五態 `Disabled1 / Disabled2 / Default / Clicked / Hover`，尺寸 `60x25`，radius `8`，padding `12x4`，label 用 `12/M12`。機器檢查規則不可再顯示舊的 `Property 1=未完成/已完成`。
- `主播回覆用 Input`：node `84:349813` 確認尺寸 `278x42`，radius `12`，padding `12`，border `0.5px background/scroll bar`，狀態 `Default / Active`。
- `常態資訊 Input`：node `84:346527`，一般文字型為 `171x33`、radius `8`；含按鈕型為 `350x42`；狀態 `Default / Active / Fill / Disabled / Error`；Error border 使用 `輔助色/PK150-直播间弹窗选取框 #DA3C69`。
- `Shared_tab`：node `84:346605`，尺寸 `80x26`，selected 使用 `14/M14` 白字與 `24x4` 品牌漸層底線；unselected 使用 `14/R14` 與 `Text/Secondary text 50%`。
- `下拉选单_性別`：node `84:347414`，容器 `171x66`，背景 `彈窗/Background`，border `Border/Drop-down menu`，radius `8`，padding `8`，選項高 `25`、radius `6/7`。這次仍只驗證到 hover on/off；annotation 提到 active，但目前讀到的 node 沒有提供 active variant。
- `上传的素材`：node `84:349819`，尺寸 `259x36`，padding `8`，gap `8`，selected 背景使用 raw 品牌紫 35% 漸層；帶聲音素材會出現 `169x36` 音量彈窗，slider track `4px`、knob `12px`、value label `10px Inter Semi Bold`。
- `Browser Top`：node `84:349800`，Windows / Mac 皆為 `1200x40`，背景 `#36373C`，logo `25x25`，版本號 `12/R12` 品牌主色，Menu 內容含 `我的资料 / 设备管理 / 檢查更新 / 退出登录`。

**後續作為畫面檢查背景時的優先順序：**

1. 先讀 `design-rules.json` 做機器判定。
2. 若 node id 或狀態與 `13:*` 歷史規則衝突，以本節 `84:*` 校正紀錄為準。
3. 值有 token 時用正式 token；沒有 token 但本節列為 raw exception 時只允許出現在指定元件。
4. `Brand/*` 空值與 `板块线框` 畸形值一律報 error，不做近似替換。
5. 互動元件若缺 hover / active / disabled / focus-visible，仍按第 8 節規則標記。

## 11. 目前待補

- 需要用含 `file_variables:read` scope 的 Figma token 補抓完整 variables JSON，特別是 `Brand/*` 這組壞掉的狀態變數。
- `Button01 / Button02 / Button03 / Button04` 需要設計 owner 確認正式用途。
- `Property 1` 這類 variant 命名需要改成語意命名，例如 `state`、`type`、`selected`；`button / 主播任務` 已完成，其他元件仍需逐一收斂。
- Focus-visible 規則尚未完整出現在 Figma，需要補入元件規範。
- 8px micro text 需要 accessibility review。
