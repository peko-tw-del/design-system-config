# 七星體育 設計規範 v0.2 (draft)

> 依據元件庫交付模板(00 Cover / 01 Foundations / 02 Components / 03 Patterns / 04 Screens / 05 Dev Notes)結構編寫。
> 資料來源:Figma `ZOHnVejsbiI7BbU8YIsm0j` 六個元件 section(component / icon / Navigation Bar / 赛事状态 / 页面状态 / Thumb)結構盤點 + 8 個關鍵元件/卡片 CSS 實值抽樣,右側設計稿(首頁、投注介面、注單、通知中心)作為使用場景對照。
> 標記「未驗證」者一律需回 Figma 確認後才能定案,不得直接沿用。

---

## 01 Foundations

### 1.1 色彩架構

採兩層架構。primitive 層存原始色值,semantic 層定義語意角色,元件只允許引用 semantic 層變數,禁止直接寫 hex。同色值不同語意者各自獨立命名,合併只發生在 alias 層。

#### 語意角色總表

| 角色 | Token | 值 | 用途 | 限制 |
|---|---|---|---|---|
| 品牌 | color/brand/primary | #45A6FF | tab 指示、可點擊強調 | |
| 系統錯誤 | color/status/error | #FF6954 | 表單錯誤、失敗提示 | 禁用於賠率與盈虧 |
| 系統成功 | color/status/success | #62D643 | 操作完成提示 | 禁用於賠率與盈虧 |
| 賠率漲 | color/odds/up | #FF6954 | 賠率上漲(紅漲) | live-data,獨立族 |
| 賠率跌 | color/odds/down | #62D643 | 賠率下跌(綠跌) | live-data,獨立族 |
| 賠率預設 | color/odds/none | #45A6FF | 賠率無變動 | live-data,獨立族 |
| 賠率鎖盤 | color/odds/locked | 未驗證 | 盤口鎖定 icon | live-data,需取值 |
| 盈虧贏 | color/result/win | #FF6954 | 結算贏(紅贏) | live-data,獨立族 |
| 盈虧輸 | color/result/loss | #62D643 | 結算輸(綠輸) | live-data,獨立族 |
| 盈虧平 | color/result/draw | #45A6FF | 平/走盤 | live-data,獨立族 |
| 主文字 | color/text/primary | #FFFFFF | 主要文字 | |
| 次文字 | color/text/secondary | #61819E | 次要文字、未選 tab | |
| 提示文字 | color/text/hint | rgba(97,129,158,.45) | 佔位提示 | |
| 半透明輔助 | color/text/inverse-50 | rgba(255,255,255,.5) | 深底輔助文字 | |
| 輸入底 | color/input/bg | rgba(0,0,0,.15) | 輸入框底 | |
| 輸入框線 | color/input/border | #61819E | 輸入框預設邊框 | 與 text/secondary 同值但獨立 |
| 輸入錯誤 | color/input/border-error | #FF6954 | 錯誤邊框+訊息 | alias 至 status/error |
| 分隔線 | color/border/divider | rgba(97,129,158,.45) | 分隔線 | 與 text/hint 同值但獨立 |
| 卡片底 | color/surface/card | #212E39 | 賽事卡、清單卡 | 現況寫死 |
| 選項底 | color/surface/option | rgba(255,255,255,.03) | 盤口選項 | 現況寫死 |
| 膠囊底 | color/surface/badge | rgba(0,0,0,.2) | 計數膠囊 | 現況寫死 |
| 弱化文字 | color/text/tertiary | #506D87 | 計數(+56) | 現況寫死 |
| 選項標籤 | color/text/option-label | #7497B7 | 主/客/和、讓球數 | 現況寫死 |
| CTA 漸層起 | color/cta/primary-gradient-start | rgba(41,115,184,.9) | 投注按鈕 | 現況寫死,必須綁定 |
| CTA 漸層迄 | color/cta/primary-gradient-end | #2B73B5 | 投注按鈕 | 現況寫死,必須綁定 |
| 按鈕禁用 | color/cta/disabled-bg | rgba(47,64,78,.8) | 禁用底 | |

#### 中式紅綠雙軌鐵律

賠率方向色(紅漲綠跌)與系統回饋色(紅錯綠對)語意方向相反。odds、result 兩族與 status 族即使色值相同,永遠獨立命名、獨立管理、掛 live-data tag。任何「看起來一樣就合併」的提案一律駁回,理由見 05 Dev Notes 問題清單第 1、2 條。

### 1.2 字體

| 用途 | 字體 | 規格 |
|---|---|---|
| 介面文字 | PingFang HK | 中文、英文介面文案與一般標籤文字;Regular / Semibold 兩檔 |
| 所有數字 | DIN Alternate Bold | 賠率、比分、金額、限額、時間、百分比、計數、統計一律使用,禁止數字用 PingFang |

字級尺度:10(錯誤訊息)/ 12(標籤)/ 13(卡內賠率、時間、注單狀態,檔內有 13/R13-iOS26 樣式)/ 14(賠率元件、輸入、tab)/ 16(金額、按鈕)。兩個待裁決:15px(可贏標籤)一處待併入;賠率數字同時存在 13 與 14 兩種字級(賽事卡 vs OddsValue 元件),必須擇一。現況同一張賽事卡內 PingFang HK 與 SC 混用;依 2026-07-07 owner 決策,SC 視為需修正,統一為 PingFang HK。

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

SlideBetButton Disabled 態現況為 40px,違反 pill 規則,需統一為 500px(規則 C-001)。

### 1.4 間距與尺寸

輸入框 327x36,padding 12/10。按鈕高 42,padding 左8 右16 上下10。tab 高 40。滑塊 28。tab 指示條 22x3。icon 尺度 12 / 16 / 20 / 24;國旗現況 17px 為孤立尺寸,建議併入 16(待裁決)。

---

## 02 Components(已抽樣 5 個,規格詳見 design-components.json)

| 元件 | 狀態 | 關鍵規格 | 觸發條件 |
|---|---|---|---|
| OddsValue | None / Up / Down / locked | DIN 14px;Up 紅 Down 綠 None 藍 | 即時賠率變動方向 |
| 已结算注单总计状态 | 贏 / 輸 / 平 | 標籤 PingFang 12 灰藍;金額 DIN 16 | 注單結算結果 |
| BetAmountInput | Empty / Filled / Error | 6px 圓角,錯誤紅框+上方 10px 訊息 | 低於 2.00 或高於 30,000 觸發 Error |
| SlideBetButton | Default / Dragging / Success / Disabled | 42px 膠囊,漸層藍;禁用深灰 | 拖曳中 / 投注成功 / 未選注或封盤 |
| tab_btn | default / Focus | 選中白字+3px 藍指示條 | 分類切換 |
| 賽事卡 | 未開賽~點球大戰 8 態 | 4px 卡,#212E39 底,盤口欄 58px | 賽況即時切換 |
| 注单状态 | 投注成功/处理中/已结算/走盘 | PingFang 13px,藍/灰藍二分 | 注單生命週期 |

其餘 40+ 元件僅完成結構盤點(清單見 design-components.json inventory),規格待逐一補齊。**特別注意:賽事卡雖有 8 種狀態,現況是普通 frame 不是 component,規則 G-008 要求元件化後才能進 02 章定案。**

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
2. **語意反轉命名**:「贏」綁在名為 error 的變數上、「輸」綁在 success 上。中式紅贏綠輸慣例硬套西式 error=紅 命名的結果,新人接手必誤用。
3. **中英雙套平行變數**:status/* 與 辅助色/* 同值雙包,分別被不同元件引用,已是實際存在的壞重複。
4. **投注 CTA 漸層整組寫死**:rgba(41,115,184,.9) → #2B73B5 未綁任何變數,主要行動按鈕是全檔案最沒保護的顏色。
5. **SlideBetButton 圓角不一致**:Disabled 40px,其餘狀態 500px。
6. **Variant 屬性名至少 9 種並存**:State、Property 1、play、赛况、数值、已讀、分类、Tab State、Search State,含大量 Property 1 佔位名。
7. **元件命名四種格式混用**:Icon/Pin、icon 16px、Icon_12px、icon / 搜索。
8. **同名元件集重複**:tab_btn 兩組(variant 值還一組 Focus 一組 focus)、amount 兩組。
9. **繁簡混用**:同一 Navigation Bar 元件集內「標題＋返回」(繁)與「筛选」(簡)並存。
10. **語意錯置的元件分類**:「国旗」集混入兵工厂、曼城、巴黎圣日尔曼、布蘭特福德等俱樂部隊徽;「icon / 搜索」實際內容為聯賽 icon。
11. **賽事卡整卡零變數且非元件**:首頁最高頻元素,8 種狀態全是散裝 frame,#212E39 / #45A6FF / #7497B7 / #506D87 / rgba(255,255,255,.03) 全部寫死,內部圖層全為 Frame 159788xxxx 佔位名。
12. **藍色也有雙包**:#45A6FF 同時存在 brand/primary 與 main/b80 兩個變數,注單狀態綁後者、tab 綁前者。
13. **賠率字級雙標**:OddsValue 元件 14px,賽事卡內賠率 13px,同一種資料兩種字級。
14. **字體家族混用**:同一張賽事卡內 PingFang HK 與 PingFang SC 並存;已決議統一 PingFang HK,SC 視為違規。
15. **非整數行高**:盤口欄標題行高 16.5px。

---

## 覆蓋範圍聲明

本版規範的色彩、字體、圓角、間距值,來自 8 個元件/卡片的 CSS 實值讀取(OddsValue、已结算注单总计状态、BetAmountInput、SlideBetButton、tab_btn、賽事卡未開賽態、注单状态、页面状态 Empty),已驗證。賽事卡僅讀未開賽一態,其餘七態未驗證。其餘元件僅結構盤點,規格欄位為空,不得視為已定案。odds/locked 色值、btn_bet 系列的 focus 色、赛况系列的狀態色等均未讀取,禁止引用本文件宣稱其值。
