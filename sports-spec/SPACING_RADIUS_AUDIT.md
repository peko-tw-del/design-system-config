# 七星體育 間距與圓角掃描摘要

掃描日期:2026-07-08  
Figma: `ZOHnVejsbiI7BbU8YIsm0j`

## 掃描節點

- PC 端助手參考:`8042:10247`
- 體育元件庫/設計稿:`6863:293089`
- Claude 候選結果:`8182:12795`

## 主要證據

- PC 參考 spacing 為 `2/4/8/12/16/24/32/48`, radius 為 `0/4/8/12/16/full`。
- 體育元件庫 component section 中,auto-layout spacing 高頻為 `2/4/8/10/12`,另有 `6/16/24`;padding 高頻為 `2/4/6/8/10/12/16`;radius 高頻為 `4/12/100/500`,另有 BetAmountInput `6`。
- 體育畫面中,首頁與投注/注單流程反覆出現 `2/4/6/8/10/12/16/24`;`30/185/271` 出現在 SPACE_BETWEEN 或排版推開值,不應列入 token。
- `32/48` 未看到足夠體育 UI 使用證據,不納入體育 spacing。

## 排除規則來源

- `SPACE_BETWEEN` 的 `itemSpacing` 會出現 `34/127/185/200/207/271` 等計算值,不能當作 token。
- Component Set 外框 `cornerRadius=5` 是 Figma 預設展示樣式,不是 UI 值。
- vector、圖片裁切、icon 縮放會產生小數值,例如 `0.465/0.5/6.373/10.484`,不作 token。

## 已定案遷移建議

- spacing `1 -> 2`, `3 -> 4`, `5 -> 6`, `9 -> 8`, `11 -> 12`, `15 -> 16`。
- radius `2 -> 4`, `40/100/437.5/500 -> 999`。
- 上述值不建 token,plugin 需以 warning 報告並附建議 token。

## 需要討論

- `radius/input 6px` 是否納入。BetAmountInput 三態都使用 6px,但整體體育元件最高頻圓角仍是 4px。
