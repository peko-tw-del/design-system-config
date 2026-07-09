# 七星體育 設計規範 v0.4.0 (foundation refresh)

> 依據元件庫交付模板(00 Cover / 01 Foundations / 02 Components / 03 Patterns / 04 Screens / 05 Dev Notes)結構編寫。
> 資料來源:Figma `ZOHnVejsbiI7BbU8YIsm0j` node `8359:14643` foundation。色彩以 18 個 Figma Variables 為準,並同步 Typography / Spacing / Radius / Shadow foundation。

顏色白名單只以 `8359:14643` foundation / Figma Variables 為準。元件庫與右側設計稿都只是被檢查對象;若它們出現未登記色或透明度,揪錯貓需報告。體育模式所有 fill、stroke、圖層 opacity 必須是 100% 實色,不得用 rgba 或 30%/50% opacity 調色。Overlay、scrim、backdrop、iOS 原生鍵盤與 Home Indicator 這類素材不納入 token 規範檢查。
> 標記「未驗證」者一律需回 Figma 確認後才能定案,不得直接沿用。

---

## 01 Foundations

### 1.1 色彩架構

採兩層架構。primitive 層對齊新版 Figma Variables(Main / Secondary / Text / Background),semantic 層定義產品語意角色。新版色彩規範採「一個顏色一條,同色不重複列」,所以機器檢查以 18 個 Variables 實值作為白名單;產品語意仍在 semantic 層拆分,避免 status / odds / result 在文案與修正建議中混稱。

新版註記:紫色 `#7C4DFF` 與粉色 `#E8557D` 已隨定案移除。Basic_Color 表格存在紅旗待核項,本版以 Figma Variables API 實值為準。

#### 語意角色總表

| 角色 | Token | 值 | 用途 | 限制 |
|---|---|---|---|---|
| 主色 | color/brand/primary | #45A6FF | Main/B80;tab 指示、可點擊強調、賠率無變動 | |
| 投注按鈕 | color/cta/primary | #2D8CE5 | Main/B60=投注按钮 | 舊 CTA 漸層不再是 Basic_Color 變數 |
| 主色按壓 | color/brand/pressed / color/cta/pressed | #3C8BD4 | Main/B50=Pressed | |
| 系統錯誤 | color/status/error | #FF6954 | Secondary/Error;輸入錯誤、失敗提示 | 同色也承載 odds/result,需靠語意層分流 |
| 系統成功 | color/status/success | #62D643 | Secondary/Success;投注成功提示 | 同色也承載 odds/result,需靠語意層分流 |
| 比分高亮 | color/status/score | #FFB83D | Secondary/score;比分、錢包餘額、預計派彩金額 | |
| 賠率漲 | color/odds/up | #FF6954 | 賠率上漲(紅漲) | live-data |
| 賠率跌 | color/odds/down | #62D643 | 賠率下跌(綠跌) | live-data |
| 賠率預設 | color/odds/none | #45A6FF | 賠率無變動 | live-data |
| 賠率選中值 | color/odds/selectedValue | #17212A | 底賠率選中後的深色數值 | live-data,場景待核 |
| 賠率鎖盤 | color/odds/locked | 未驗證 | 盤口鎖定 icon | live-data,需取值 |
| 盈虧贏 | color/result/win | #FF6954 | 結算贏(紅贏) | live-data |
| 盈虧輸 | color/result/loss | #62D643 | 結算輸(綠輸) | live-data |
| 盈虧平 | color/result/draw | #45A6FF | 平/走盤 | live-data |
| 一般文字 | color/text/primary | #61819E | Text/T60;文字主色 | 表格文字欄曾誤列,以 Variables 實值為準 |
| 白字 | color/text/inverse | #FFFFFF | Text/WT;選中 tab、隊伍名、標題 | |
| 標籤文字 | color/text/label | #7497B7 | Text/T50=Label;盤口選項標籤 | 表格文字欄曾誤列,以 Variables 實值為準 |
| 膠囊/圖示文字 | color/text/badge | #506D87 | Text/T70=Badge;盘口数、Icon | |
| 陰影/遮罩黑 | color/surface/black | #000000 | Background/Black;陰影、遮罩 | overlay/scrim/backdrop 透明素材排除掃描 |
| 輔助文字 | color/text/assist | #96C5F2 | Text/T30=辅助字;投注按鈕輔助字 | |
| 輸入/彈窗底 | color/input/bg / color/surface/inputPopup | #1E2935 | Background/B80=Input,弹窗Bg | |
| 輸入框線 | color/input/border | #384C5E | Background/B20;提示詞/分隔/控制弱色 | 場景待核 |
| 輸入錯誤 | color/input/borderError | #FF6954 | 錯誤邊框+訊息 | alias 至 status/error |
| 分隔線 | color/border/divider | #384C5E | Background/B20 | |
| 最深底/膠囊底 | color/surface/deepest | #17212A | Background/B100;最深底、盘口数膠囊底 | 盘口数 B90 vs B20 歸屬待核 |
| 卡片/AppBar 底 | color/surface/appBarCard | #212E39 | Background/B70=Card | |
| 內卡/scrollbar | color/surface/innerCard | #28343F | Background/B60 | |
| 禁用下注底 | color/cta/disabledBg | #2A3A47 | Background/B50=Disable下注按鈕 | |
| 控制弱底 | color/surface/control | #384C5E | Background/B20;投注金額選項、disable、選中、提示詞 | 場景待核 |

#### 中式紅綠雙軌鐵律

賠率方向色(紅漲綠跌)與系統回饋色(紅錯綠對)語意方向相反。新版 Variables 面板為一色一列,因此 `Secondary/Error` 同時列出輸入錯誤、賠率上升、盈虧贏;但在規範層仍必須分成 status / odds / result 三個語意入口。plugin 可接受同一色值,但修正建議必須說清楚情境。

### 1.2 字體

| 用途 | 字體 | 規格 |
|---|---|---|
| 介面文字 / 小標 / 日期 tab | PingFang HK | 中文、英文介面文案、玩法名稱、盤口小標、日期篩選 tab;例如「主」「-1」「+1」「大4/4.5」「06/22」「0623」 |
| 純數字資料 | DIN Alternate Bold | 賠率、比分、金額、限額、比賽時間、百分比、計數、統計;例如「1.51」「46'00”」「2」「0」 |

字級尺度:10(錯誤訊息)/ 12(標籤)/ 13(卡內賠率、比賽時間、注單狀態,檔內有 13/R13-iOS26 樣式)/ 14(賠率元件、輸入、tab)/ 16(金額、按鈕)。兩個待裁決:15px(可贏標籤)一處待併入;賠率數字同時存在 13 與 14 兩種字級(賽事卡 vs OddsValue 元件),必須擇一。玩法/盤口小標如「主」「-1」「大4/4.5」「小4/4.5」整串視為文案,使用 PingFang HK;日期篩選 tab 如「06/22」「0623」也使用 PingFang HK。其他純數字資料如賠率、比分、金額、比賽時間「46'00”」使用 DIN Alternate Bold。現況同一張賽事卡內 PingFang HK 與 SC 混用;依 2026-07-07 owner 決策,SC 視為需修正,統一為 PingFang HK。

行高:賠率 20px,按鈕文字 22px。

### 1.3 圓角

| Token | 值 | 用途 |
|---|---|---|
| radius/input | 6px | 輸入框 |
| radius/tab | 12px | tab 容器 |
| radius/pill | 500px | 膠囊按鈕 |
| radius/indicator | 100px | tab 底線指示條 |
| radius/card | 4px | 賽事卡、盤口選項 |
| radius/badge | 100px | 計數膠囊 |

SlideBetButton Disabled 態現況為 40px,需遷移並綁定 `radius/full`(規則 C-001 / spacing-radius migration)。

### 1.4 間距與尺寸

輸入框 327x36,padding 12/10。按鈕高 42,padding 左8 右16 上下10。tab 高 40。滑塊 28。tab 指示條 22x3。icon 尺度 12 / 16 / 20 / 24。

---

## 02 Components(01-04 元件庫已更新,規格詳見 design-components.json)

| 章節 | 元件 | 關鍵規格 | Plugin |
|---|---|---|---|
| 01 共用基礎 Common | button | 134x40,r4,Main/B80;文字 PingFang HK Semibold 14,#17212A | inventory + detailed |
| 01 共用基礎 Common | Button/Confirm | 351x50,r8;Default Main/B80,Pressed Main/B50,Disabled Background/B20/Text/T60 | size/radius/padding |
| 02 導航 Navigation | Navigation Bar | 375w;標題型 96,搜尋/篩選型 133,無標題 62;底 Background/B70 | size/padding |
| 02 導航 Navigation | Navigation Bar / Search | 319x36,radius/full,Background/B100,padding 12,itemSpacing 10 | size/padding + full warning |
| 02 導航 Navigation | Navigation Bar / BetOrder | 375x36,Background/B80,padding LR12,itemSpacing 4;active 白 / inactive Text/T60 | size/padding |
| 03 投注 Betting | SlideBetButton | 351x42,pill,padding 8/16/10;Disabled Background/B50 | size/padding + full warning |
| 03 投注 Betting | BetAmountInput | 327x36,r6,padding 12/10;Empty/Filled 邊 #61819E,Error #FF6954 | size/padding + radius 6 warning |
| 03 投注 Betting | Checkbox | 16x16,r4;Checked Main/B80,Unchecked Text/T60 stroke | size/radius/padding |
| 03 投注 Betting | btn_bet_S / btn_bet_L / BetOption | S 58x30,L 58x46.5,BetOption 166.5x42,r4;focus Main/B80,default B60/B70 | size/radius/padding |
| 03 投注 Betting | OddsValue | DIN 14;Up Secondary/Error,Down Secondary/Success,None Main/B80 | size/padding |
| 04 注单 Bet Orders | 注单状态 | 89x18;active Main/B80,pending/void Text/T60;PingFang HK 13 | size/padding |
| 04 注单 Bet Orders | 已结算注单总计状态 / 已结算注单状态 | 贏 Secondary/Error,輸 Secondary/Success,平 Main/B80;金額 DIN | semantic color |
| 04 注单 Bet Orders | 注单总计 / 注单 | 總計 351x60,r4,B70,padding 8/12;列表 351x159 | size/radius/padding |

其餘元件仍保留 inventory。**特別注意:賽事卡雖有 8 種狀態,現況是普通 frame 不是 component,規則 G-008 要求元件化後才能進正式 component rules。**

### 元件庫目前仍需設計修正

- `Property 1` 仍出現在 Button/Confirm、Navigation Bar / BetOrder、btn_bet、注单等元件,plugin 會報 variant 佔位命名。
- SlideBetButton Dragging 仍有 PingFang SC 與 `#FDFEFF`,plugin 會由字體與色彩規則報告。
- Button/Confirm / Navigation Search / RightActions 這類 pill 寫法需改綁 `radius/full`。
- BetAmountInput radius 6px 仍待設計決定是否改 `radius/xs` 或新增 input 專用規範。

### 互動狀態完整性要求

可互動元件至少定義 default / focus(selected) / disabled 三態。現況多數元件缺 disabled,遇停盤封盤場景會臨時湊色(規則 C-006)。

---

## 03 Patterns(待建)

投注流程(選注 → 金額輸入 → 滑動確認 → 成功回饋)、注單篩選、賽況即時更新三個核心 pattern 待元件規格補齊後編寫。

---

## 04 Screens(待建)

依 token 系統既有進度,賠率表、投注單、注單紀錄、錢包餘額畫面已有 token 對應,結果頁與剩餘畫面待補。

---

## 05 Dev Notes:現況問題清單

依嚴重度排序,修正前規範不得定案:

1. **同一紅色 #FF6954 扛三種語意**:status/error(輸入錯誤)、status/error(賠率上漲)、辅助色/error(贏錢金額)。三個變數無 alias 關係,改任何一個都可能連動其他兩處。綠色 #62D643 同病。
2. **Variables 一色一列後,語意需靠 alias 補清楚**:新版 `Secondary/Error` 同時承載輸入錯誤、賠率上升、盈虧贏;plugin 不應再把「贏綁 error」視為單純錯誤,而是要求元件規格寫成 `color/result/win -> Secondary/Error`。
3. **舊中英雙套平行變數需收斂**:status/*、辅助色/*、main/b80、brand/primary 這類同值雙包需收斂到新版 Variables,本地只保留 semantic alias。
4. **投注 CTA 已改以 Variables 管理主色**:新版 Basic_Color 定義 Main/B60 `#2D8CE5` 為投注按鈕、Main/B50 `#3C8BD4` 為 Pressed;舊 `rgba(41,115,184,.9) → #2B73B5` 不再是合規 Basic_Color。CTA 疊層/邊框仍列紅旗待核。
5. **SlideBetButton 圓角不一致**:Disabled 40px,其餘狀態 500px。
6. **Variant 屬性名至少 9 種並存**:State、Property 1、play、赛况、数值、已讀、分类、Tab State、Search State,含大量 Property 1 佔位名。
7. **元件命名四種格式混用**:Icon/Pin、icon 16px、Icon_12px、icon / 搜索。
8. **同名元件集重複**:tab_btn 兩組(variant 值還一組 Focus 一組 focus)、amount 兩組。
9. **繁簡混用**:同一 Navigation Bar 元件集內「標題＋返回」(繁)與「筛选」(簡)並存。
10. **賽事卡整卡需改綁新版 Variables 且仍非元件**:首頁最高頻元素,8 種狀態全是散裝 frame,需收斂到 Background/B70、Background/B100、Background/B20、Main/B80、Text/T50、Text/T70 等新版色彩,內部圖層全為 Frame 159788xxxx 佔位名。
11. **舊藍色雙包已收斂到 Main/B80**:#45A6FF 以 Figma Variables `Main/B80` 為來源;本地 semantic 可 alias 成 brand/primary、odds/none、result/draw,但不得再新增無關雙包。
12. **賠率字級雙標**:OddsValue 元件 14px,賽事卡內賠率 13px,同一種資料兩種字級。
13. **字體家族混用**:同一張賽事卡內 PingFang HK 與 PingFang SC 並存;已決議統一 PingFang HK,SC 視為違規。
14. **非整數行高**:盤口欄標題行高 16.5px。

---

## 覆蓋範圍聲明

本版色彩白名單來自新版 foundation (`8359:14643`) 與 Figma Variables API / foundation metadata 讀取的 18 個 color variables。文字、間距、圓角與陰影已同步 foundation:spacing 4/6/8/10/12/16/20 + safe-bottom 34;radius 4/8/16/full;shadow/nav 與 shadow/bottom-bar。元件規格仍有部分歷史抽樣資料,不得覆蓋 foundation 尺度。
