# Figma Node 84 Audit

日期：2026-07-03  
Figma file：`jqSHxR0AfK3xDvNbJOl7Li`  
完整 UI kit：`84:346183` / `直播助手PC端_UI kit`  
最新修改區：`84:349799` / `07/01 組件修改項目`

## 結論

`84:346183` 是完整元件庫，`84:349799` 是最新修改區；後續做畫面檢查時，若舊規範中的 `13:*`、`64:*`、`74:*` node id 與這次 Figma 連結不同，應以 `84:*` 的實際讀取結果作為最新版來源。

## 正式 Token

| 分類 | Token | Figma variable | 值 |
|---|---|---|---|
| Text | `color.text.primary` | `Text/WT` | `#FFFFFF` |
| Text | `color.text.secondary` | `Text/Secondary text` | `#B8ACBE` |
| Text | `color.text.secondaryMuted` | `Text/Secondary text 50%` | `#B8ACBE80` |
| Text | `color.text.default` | `Text/Default text` | `#ADAAAA` |
| Text | `color.text.icon` | `Text/icon` | `#E0D8F1` |
| Brand | `color.brand.primary` | `primary/Main` | `#CB8FE9` |
| Background | `color.bg.primary` | `background/Primary` | `#0B080D` |
| Background | `color.bg.browserTop` | `background/ Browser Top` | `#36373C` |
| Surface | `color.surface.panel` | `background/板塊` | `#8A8A8A1A` |
| Surface | `color.surface.panelHover` | `background/板塊 Hover` | `#8A8A8A33` |
| Input | `color.input.background` | `Input/input` | `#312744` |
| Dialog | `color.dialog.background` | `彈窗/Background` | `#1B1721` |
| Border | `color.border.dropdown` | `Border/Drop-down menu` | `#CB8FE980` |
| Shadow | `shadow.overlay` | `彈窗shadow` | `0 0 20px #00000080` |

## Raw Exception / Broken Reference

| 名稱 | 狀態 | 處理 |
|---|---|---|
| `Brand/hover` | broken reference，解析為空 | 不可猜值；請在 Figma 修 variable |
| `Brand/Disable` | broken reference，解析為空 | 不可猜值；請在 Figma 修 variable |
| `Brand/Brand(active)` | broken reference，解析為空 | 不可猜值；請在 Figma 修 variable |
| `板块线框` | malformed，解析為 `","` | 視為 variable 資料壞掉 |
| Browser Top menu border | raw `0.5px white` | 需新增或綁定 border token |
| 品牌紫 35% 漸層 | raw gradient，多角度 | 需決定是否正式收斂成 token |

## 元件摘要

| 元件 | Node | 尺寸 / 狀態 | 檢查重點 |
|---|---|---|---|
| Primary Button - Large | `84:346186` | `350x48`，radius `12`，`Default / Hover / Active / Disabled / loading` | `loading` 已補入 canonical component states；disabled 使用弱化漸層與 muted text |
| button / 主播任務 | `84:349806` | `60x25`，radius `8`，`Disabled1 / Disabled2 / Default / Clicked / Hover` | 規則不得再顯示 `Property 1=未完成/已完成` |
| 主播回覆用 Input | `84:349813` | `278x42`，radius `12`，`Default / Active` | 高度固定 42px；padding 12；border 0.5px |
| 常態資訊 Input | `84:346527` | 文字型 `171x33`；含按鈕型 `350x42` | `Default / Active / Fill / Disabled / Error`；Error border `#DA3C69` |
| Shared_tab | `84:346605` | `80x26`；底線 `24x4` | selected 用 `14/M14` 白字；unselected 用 `14/R14` muted text |
| 下拉选单_性別 | `84:347414` | `171x66`，padding `8`，option `25px` | 已驗證 hover on/off；尚未讀到 active variant |
| 上传的素材 | `84:349819` | `259x36`，padding `8`，gap `8` | selected 使用 raw 品牌紫 35% 漸層；帶聲音素材有音量彈窗 |
| Browser Top | `84:349800` | `1200x40`，Windows/Mac | 背景 `#36373C`；Menu 有「檢查更新」 |

## 後續檢查優先順序

1. 機器檢查仍先讀 `design-rules.json`。
2. 若歷史 node id 與 `84:*` 實際讀取衝突，以本檔與 `DESIGN.md` 第 10.3 節為準。
3. 正式 token 可自動修正；raw exception 只能在指定元件出現。
4. `Brand/*` 空值與 `板块线框` 畸形值一律報 error。
5. 互動元件缺 hover / active / disabled / focus-visible 時仍報 error。
