# 七星體育 間距與圓角規範 v1.0

> 新開規範,不覆蓋既有 `design-tokens.normalized.json`。  
> 來源:Figma `ZOHnVejsbiI7BbU8YIsm0j` 的 PC 參考節點 `8042:10247`、體育元件庫/設計稿 section `6863:293089`、Claude 候選節點 `8182:12795`。  
> 判定原則:優先採體育元件庫與設計稿實測;PC 端助手只作參考,不可直接沿用。

## 與 PC 端助手不同

PC 參考尺度是 spacing `2 / 4 / 8 / 12 / 16 / 24 / 32 / 48`, radius `0 / 4 / 8 / 12 / 16 / full`。

體育實測後不能直接沿用 PC 尺度:

- `6px` 和 `10px` 在體育元件與畫面中高頻出現,應納入正式 spacing。
- `20px` 在體育的 Seek、底部菜单與大區塊距離中有明確用途。
- `32px / 48px` 在體育本次掃描中沒有足夠 UI 使用證據,不納入。
- `34px` 只作為 iOS Home Indicator safe-bottom,不得作一般 spacing。

## Spacing

| Token | 值 | 用途 | 狀態 |
|---|---:|---|---|
| spacing/2 | 2px | 緊湊圖文間距,BetOption / Team Info / Score | approved |
| spacing/4 | 4px | 元件內小間距,MarketTabs / Title group | approved |
| spacing/6 | 6px | 體育專屬內容群組間距與小型 padding | approved |
| spacing/8 | 8px | 區塊內間距、卡片內距、通知卡片 | approved |
| spacing/10 | 10px | Keypad / Checkbox / SlideBetButton / btn_bet | approved |
| spacing/12 | 12px | 主容器間距與 375 寬畫面 gutter | approved |
| spacing/16 | 16px | 卡片與段落間距、Header 左右 padding | approved |
| spacing/20 | 20px | 大區塊間距、資訊區到盤口區 | approved |
| spacing/24 | 24px | popup 與頁面級留白 | approved |
| spacing/safe-bottom | 34px | iOS Home Indicator 安全區,僅限 padding-bottom | approved scoped |

## Radius

| Token | 值 | 用途 | 狀態 |
|---|---:|---|---|
| radius/none | 0px | 無圓角 | approved |
| radius/xs | 4px | 賠率框、MarketGroup、賽事卡、注單卡片 | approved |
| radius/sm | 8px | 按鈕狀態、漂字、較強互動容器 | approved |
| radius/md | 12px | Tabs、tab_btn、Focus 態 | approved |
| radius/lg | 16px | Popup 彈窗 | approved |
| radius/xl | 20px | 底部菜单上緣、Seek track | approved |
| radius/full | 999px | 藥丸與圓形: Indicator、SlideBetButton、國旗裁切、switch | approved |

## 偏離值遷移建議

以下值不建 token。plugin 在體育模式需以 warning 報告,並附建議值。

| 值 | 類型 | 觀察 | 建議 |
|---:|---|---|---|
| 1px | spacing | btn_bet 賠率數字組全變體 | 改 `spacing/2` |
| 3px | spacing | 注单玩法(play) 與赛事状态家族垂直堆疊 | 改 `spacing/4` |
| 5px | spacing | 注单卡內部群組 | 改 `spacing/6` |
| 9px | spacing | 非 SPACE_BETWEEN 的 Section Header / top 手調值 | 改 `spacing/8` |
| 11px | spacing | 赛事状态全部时段變體 | 改 `spacing/12` |
| 15px | spacing | Checkbox 家族手調值 | 改 `spacing/16` |
| 2px | radius | 赛事状态單一節點 | 改 `radius/xs` |
| 40px | radius | SlideBetButton Disabled | 改 `radius/full` |
| 100 / 500 / 437.5px | radius | 藥丸/圓形多種寫法 | 改 `radius/full` |

## 待設計確認

| 值 | 類型 | 觀察 | 暫定處理 |
|---:|---|---|---|
| 6px | radius | BetAmountInput Empty/Filled/Error 三態皆為 6px | 先報 warning;等設計決定改 `radius/xs` 或新增 input 專用規範 |
| 2.25 / 2.75px | padding | amount 文字垂直置中 | 不建 token |
| 0.465 / 0.5 / 6.373 / 10.484 / 13.5px | radius/spacing | vector、圖片、icon 縮放產物 | 排除 |

## Plugin 檢查規則

- 體育模式只允許 `spacing-radius.tokens.json` 內 approved spacing/radius。
- `primaryAxisAlignItems=SPACE_BETWEEN` 的 `itemSpacing` 是 Figma 計算值,不檢查。
- `spacing/safe-bottom 34px` 只允許 `paddingBottom`,其他屬性出現 34px 應報 warning。
- `Component Set` 外框的 `cornerRadius=5` 是 Figma 展示外框,不檢查。
- Vector / Boolean / Line 的小數 radius 不檢查。
- `100 / 500 / 437.5 / 40px` 這類 full round 寫法需報低風險 warning,建議綁 `radius/full`。
- 其他未列入遷移表的 `radius >= min(width,height)/2` 視覺 full round 可排除。
