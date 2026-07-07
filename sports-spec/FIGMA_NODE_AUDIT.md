# 七星體育 Figma Node Audit

日期:2026-07-07  
目的:記錄本次建立 sports-spec 時可追溯的 Figma 來源與已知風險。

## 來源

| 類型 | Node | 用途 |
|---|---|---|
| 色彩 / 文字規範 | `8089:13855` | 新版 color foundation 與 typography 決策來源 |
| 元件 / 完整畫面 | `6863:293089` | 左側元件、右側完整設計稿與常見場景 |
| Claude 規範包 | `/Users/peko/Downloads/files.zip` | draft tokens、rules、components、DESIGN.md |

## 已確認

- 產品為七星體育,此資料包應獨立於 `pc-spec`。
- 文字標準為 PingFang HK。
- 數字標準為 DIN Alternate Bold。
- 體育 live-data 色彩需獨立於系統 status 色彩。
- 現況已有足夠資料建立揪錯貓第一版審查基礎,但不是完整定案版設計系統。

## 主要風險

- Claude draft 中曾把 PingFang HK / SC 混用列為 warning;本次依 owner 決策改為 error。
- 賠率數字目前 13px / 14px 並存,不宜直接判其中一個為錯。
- CTA 漸層目前是硬寫值,應標記為需要 token 化。
- 賽事卡是高頻畫面單元,但目前仍有散裝 frame 與硬寫色問題。
- 部分元件只有 inventory,尚未完成 detailed spec,plugin 第一版不應對它們做過度精準判斷。
