# 七星體育 Design System 綜合判定版

日期:2026-07-07  
用途:給設計師、工程師與揪錯貓共同使用的體育版 canonical 規範摘要。

## 比對來源

- Claude 產出的 draft spec:`DESIGN.md`、`design-tokens.normalized.json`、`design-components.json`、`design-rules.json`
- Figma 色彩與文字規範:`ZOHnVejsbiI7BbU8YIsm0j / 8089:13855`
- Figma 元件與完整畫面:`ZOHnVejsbiI7BbU8YIsm0j / 6863:293089`
- Owner 決策:2026-07-07 確認「文字 PingFang HK、數字 DIN」

## 判定原則

1. `design-tokens.normalized.json` 是 plugin 的 token 來源。
2. `design-rules.json` 是揪錯貓的機器檢查來源。
3. `DESIGN.md` 是給人看的規範說明,不可作為唯一機器來源。
4. 已有 owner 決策者列為 error;尚未定案但有風險者列為 warning;只盤點現況者列為 info 或 draft。
5. 同色值不同語意必須拆 token,尤其體育產品的賠率 / 盈虧色彩不得與系統狀態色混用。

## 已升級為正式規則

| 項目 | 最終判定 | 揪錯貓層級 |
|---|---|---|
| 介面文案字體 | PingFang HK | error |
| 數字字體 | DIN Alternate Bold | error |
| HK / SC 混用 | 不允許,SC 視為需修正 | error |
| 未登記顏色 | 不允許直接使用 hex / rgba | error |
| live-data 色彩 | odds/result/status 分族管理 | error |
| 小數點尺寸與行高 | 先標記;容器尺寸需修正,文字行高待裁決 | warning |
| 高頻卡片散裝 frame | 賽事卡、注單卡應元件化 | error |

## 待 owner 裁決

- 賠率字級到底定為 13px 或 14px。
- `15px` 可贏標籤是否保留為正式 token。
- CTA 漸層 `rgba(41,115,184,.9) -> #2B73B5` 是否建立正式 gradient token。
- 賽事卡的 8 種賽況是否全部整理成 component variants。
- `odds/locked` 鎖盤 icon 色值需回 Figma 補值。

## 給揪錯貓的建議分層

- `error`:字體、數字字體、未登記色、語意錯置 token、variant 命名佔位、重複元件名稱。
- `warning`:賠率字級衝突、15px 孤立字級、非整數行高、尚未正式建變數的漸層。
- `info`:目前僅盤點但未讀完整 CSS 的元件 inventory。

## 下一步

1. 把 `sports-spec` 接入揪錯貓規範切換。
2. 先讓 plugin 讀取 `design-rules.json` 的 global / naming 規則。
3. component 規則先跑已 detailed 的元件:OddsValue、BetAmountInput、SlideBetButton、tab_btn、已结算注单总计状态、賽事卡抽樣。
4. 用七星體育同一份 Figma 檔反掃一次,確認誤報與漏報。
