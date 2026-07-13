# 直播助手 PC Design System 綜合判定版

日期：2026-07-03  
用途：給設計師、工程師與後續畫面檢查工具共同使用的 canonical 規範摘要。  
比對來源：

- Claude 原始 pc spec：`DESIGN.md`、`design-tokens.normalized.json`、`design-components.json`、`design-rules.json`
- 最新 Figma 連結：`84:346183` / `直播助手PC端_UI kit`
- 最新修改區：`84:349799` / `07/01 組件修改項目`
- 本次補正：`FIGMA_NODE_84_AUDIT.md`、`design-rules.json`、`design-components.json`

## 判定原則

1. Claude 原 pc spec 是 baseline，保留其完整 token 分層、component catalog 與機器規則結構。
2. Figma node `84:*` 是目前可追溯的最新設計來源；若與舊 `13:*`、`64:*`、`74:*` 紀錄衝突，以 `84:*` 為準。
3. 只有能從 Figma variable 解析出值的項目才能列為正式 token。
4. 空值、畸形值與字面漸層不得被自動猜成 token，只能標記為 raw exception 或 broken reference。
5. 最終交付必須同時更新人看的文件與機器看的規則，兩者不能分離。

## 綜合比對結果

| 項目 | Claude pc spec | Figma node 84 | 最終判定 |
|---|---|---|---|
| Source of truth | 以 `13:8209` 與截圖/報告為主 | 最新連結實際為 `84:346183` / `84:349799` | 保留 Claude 結構，新增 `84:*` 為最新版來源優先級 |
| Token 分層 | primitive / semantic / raw exception 架構完整 | 變數讀值可驗證主要色彩與字體 | 沿用 Claude 架構 |
| Browser Top 背景 | 已補 `background/Browser Top` | `background/ Browser Top = #36373C` | 正式 token：`color.bg.browserTop` |
| Brand state variables | 已標 `Brand/*` 空值 | `Brand/hover`、`Brand/Disable`、`Brand/Brand(active)` 仍為空 | broken reference，不可猜值 |
| `板块线框` | 已標畸形值 | node 84 仍解析為 `","` | broken reference，需設計端修 variable |
| Primary Button Large | Claude 舊版 4 態 | node `84:346186` 有 `Default / Hover / Active / Disabled / loading` | 補 `loading` 為正式 state，規則同步新增 |
| button / 主播任務 | 已改成 `type`，但部分規則還殘留舊文案 | node `84:349806` 確認 5 態 | 規則改成 `type=Disabled1 / Disabled2 / Default / Clicked / Hover` |
| 主播回覆用 Input | 已修 42px | node `84:349813` 確認 `278x42` | 42px 為 canonical |
| 常態資訊 Input | 已有多狀態規格 | node `84:346527` 確認 `171x33` / `350x42` 與 Error 狀態 | Claude 規格可用，Error 色需保留 |
| Dropdown | Claude 後續已更正為列表型 dropdown + 日曆拆分 | node 84 仍只驗證到 hover on/off，未讀到 active variant | 不可宣稱 active 已完成；active 仍待補證據 |
| 上传的素材 | 已標 selected raw gradient 與音量彈窗 | node `84:349819` 確認 | 保留 raw exception，音量彈窗納入檢查背景 |
| Raw 紫色漸層 | Claude 已發現多角度 | node 84 再次確認多處存在 | 建議收斂為正式 token 前，檢查器只能當 raw exception |

## Canonical Tokens

以下可直接給工程使用；命名以 semantic 為主，Figma 原名保留作對照。

| Token | Figma variable | 值 | 使用範圍 |
|---|---|---|---|
| `color.text.primary` | `Text/WT` | `#FFFFFF` | 深色背景主要文字 |
| `color.text.secondary` | `Text/Secondary text` | `#B8ACBE` | 次要文字 |
| `color.text.secondaryMuted` | `Text/Secondary text 50%` | `#B8ACBE80` | disabled / placeholder |
| `color.text.default` | `Text/Default text` | `#ADAAAA` | 一般預設文字、Browser Top title |
| `color.text.icon` | `Text/icon` | `#E0D8F1` | icon 與輔助符號 |
| `color.brand.primary` | `primary/Main` | `#CB8FE9` | 品牌主色、版本號、focus |
| `color.bg.primary` | `background/Primary` | `#0B080D` | App 主背景、menu 深底 |
| `color.bg.browserTop` | `background/ Browser Top` | `#36373C` | Browser Top |
| `color.surface.panel` | `background/板塊` | `#8A8A8A1A` | 面板弱背景 |
| `color.surface.panelHover` | `background/板塊 Hover` | `#8A8A8A33` | 面板 hover |
| `color.input.background` | `Input/input` | `#312744` | input、dropdown option |
| `color.dialog.background` | `彈窗/Background` | `#1B1721` | dropdown / popup |
| `color.border.dropdown` | `Border/Drop-down menu` | `#CB8FE980` | dropdown border |
| `shadow.overlay` | `彈窗shadow` | `0 0 20px #00000080` | popup / overlay |

## Raw Exception 與不可用 Token

| 名稱 | 分類 | 判定 |
|---|---|---|
| `Brand/hover` | broken reference | 空值，不可使用 |
| `Brand/Disable` | broken reference | 空值，不可使用 |
| `Brand/Brand(active)` | broken reference | 空值，不可使用 |
| `板块线框` | malformed variable | 值為 `","`，不可使用 |
| `linear-gradient(... #754585 → #4B348D ... 35%)` | raw exception | 多元件共用但角度不一，需設計 owner 決定是否收斂 |
| Browser Top menu `0.5px white border` | raw exception | 應補正式 border token |

## Canonical Component States

| Component | Canonical states / variants | Canonical size |
|---|---|---|
| Primary Button - Large | `Type=Default / Hover / Active / Disabled / loading` | `350x48`, radius `12` |
| Secondary Button - Large | `Type=Default / Hover / Active / Disabled` | `350x48`, radius `12` |
| Primary Button - Medium | `default / Hover / Clicked / Disabled / 结束直播` | `88x31` |
| button / 主播任務 | `type=Disabled1 / Disabled2 / Default / Clicked / Hover` | `60x25`, radius `8` |
| 常態資訊 Input | `State=Default / Active / Fill / Disabled / Error` + `類型` | text `171x33`; text+button `350x42` |
| 主播回覆用 Input | `State=Default / Active` | `278x42`, radius `12` |
| Shared_tab | `點擊=on / off` | `80x26`; indicator `24x4` |
| Dropdown Family | `Hover=on / off` | by family; gender `171x66` |
| 日曆 | Calendar, not ordinary dropdown | `300x249` |
| 上传的素材 | `選取=on / off` + source type | `259x36` |
| Browser Top | Windows/Mac + logged in/unlogged | `1200x40` |

## 檢查器使用規則

1. 機器規則來源是 `design-rules.json`。
2. 規格說明來源是 `PC_SPEC_SYNTHESIZED.md` 與 `DESIGN.md`。
3. node id 衝突時，以 `84:*` 觀察結果覆蓋舊歷史紀錄。
4. component 規則必須同時檢查 size、radius、state 命名與 token 使用。
5. 對 `Brand/*` 空值與 `板块线框` 畸形值報 error。
6. 對 raw gradient 報 warning，除非它出現在已登記允許的 component part。
7. 若互動元件缺 focus-visible 規格，仍報 error；目前 Figma 尚未完整補齊。

## 仍需設計 Owner 決策

- `Brand/hover`、`Brand/Disable`、`Brand/Brand(active)` 是否重建為正式 variables。
- 品牌紫 35% 漸層是否統一角度並升級成正式 token。
- Browser Top menu border 是否新增 `color.border.menu` 或併入現有 border token。
- Dropdown annotation 提到 active，但 node 84 只讀到 hover；需要補 active variant 或確認 annotation 是描述需求。
- Focus-visible 樣式需要正式畫入 Figma，否則工程只能依規則暫行補 `color.brand.primary` / `gradient.input.focus.border`。
