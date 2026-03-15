# MAPLAB Kitchen — Master Data Layer

ERP-ready schema for MAPLAB Kitchen's master data management.

## 📁 Structure

```
maplab-master-data/
└── schemas/
    └── maplab_master_data.schema.json
```

## 📊 Tables

| Table | Description | Primary Key |
|-------|-------------|-------------|
| `ITEM_MASTER` | 產品主檔 — 所有品項基礎資料 | `item_id` |
| `PRICE_MASTER` | 價格主檔 — 多價格類型支援 | `price_id` |
| `ASSET_MASTER` | 圖片資產索引 — Google Drive 連結 | `asset_id` |
| `CONTENT_MASTER` | SEO 內容主檔 — 網頁/文章 | `content_id` |
| `MENU_MASTER` | 菜單主檔 — 套餐組合 | `menu_id` |
| `MENU_ITEMS` | 菜單明細表 — 多對多關聯 | `id` |

## 🔗 Relationships

```
ITEM_MASTER ←── PRICE_MASTER (1:N)
ITEM_MASTER ←── ASSET_MASTER (1:N)
ITEM_MASTER ←── CONTENT_MASTER (1:1 or null)
MENU_MASTER ──→ MENU_ITEMS ←── ITEM_MASTER (N:M)
```

## 🏷️ Naming Convention

**item_id**: `{TYPE}-{SUBTYPE}-{SEQ3}`

Example: `DES-MAC-001` (法式馬卡龍)

**Type Prefix**:

| Code | Category |
|------|----------|
| DES | Dessert 甜點類 |
| SAV | Savory 鹹食類 |
| DRK | Drink 飲品類 |
| EQP | Equipment 設備類 |
| PKG | Package 套餐包 |
| SVC | Service 服務類 |

---

## 📋 Changelog

### v0.6 — 2026-03-11 09:00 ⚠️ 防卡死機制建立

**觸發原因**: Claude 在上一 session 處理 Java/Apps Script 工具呼叫時卡死（無回應），導致進度損失。卡死比 Crash 更危險，因為剝奪可觀測性。

**本次工作**:
- 確認 v0.5 狀態：`import2025Order(url)` 由 GPT 完成，靜態測試通過
- README Changelog 格式修正（移除多餘 `- ` 前綴）
- 加入版本時間戳記規範（格式：`vX.X — YYYY-MM-DD HH:MM`）
- 建立防卡死原則（見下方 ⚡ 作業準則）

**待執行（接 v0.5 未完成項）**:
- GPT 修正 v0.5.1（付款狀態判定 + OrderLines 金額邏輯）
- 批次匯入 2025/12 月訂單
- Module B: 2026 訂單匯入
- Module C: 常用品項萃取 → PRICE_MASTER

---

### v0.5 — 2026-03-10 ~16:00

- `import2025Order(url)` function developed by GPT (Apps Script)
- Static test passed: Orders 1, OrderLines 10, OrderCharges 1
- Known issues: payment status detection, OrderLines pricing logic
- Next: v0.5.1 fix then batch import December 2025 orders

### v0.4 — 2026-03-10 14:30

- Claude audited manual tasks: 6/6 complete
- Copilot moved schema.json to /schemas/ (commit 4d0be97)
- Claude-GPT handoff protocol established

### v0.3 — 2026-03-05 17:50

- Dashboard #REF! fixed (QUERY changed to 4 columns)
- 2025 order import plan drafted
- 2025 order format analyzed (Dec 14 sample)

### v0.2 — 2026-03-05 12:45

- Sheets: 6 sheets, 100 items, 6 orders, Apps Script x6
- Items: DST54 + APP20 + MAIN5 + DST15 + BEV6 = 100 total
- Orders: 6 orders, NT$118,450 revenue

### v0.1 — 2026-03-04

- Initial schema, README, repo created

---

## ⚡ 作業準則 — 防卡死機制

**核心原則**: 絕對不要信任任何外部函數會如期執行完畢。

### 強制超時規範

- Apps Script 函數執行上限：30 秒（超時自動中止）
- 每批次匯入上限：10 筆訂單
- 工具呼叫失敗後立即回報，不重試超過 2 次

### Checkpoint 格式（每次工作後必填）

```
Checkpoint [YYYY-MM-DD HH:MM] — vX.X
完成: [項目]
中斷: [卡在哪裡、卡多久]
待執行: [下一步]
已知問題: [問題說明]
接手線索: [給下一棒 AI 的關鍵資訊]
```

### 卡死指標

- 同一工具呼叫等待 > 60 秒 → 強制中止
- 連續 3 次相同操作失敗 → 改策略
- 無法讀取回應 → 截圖確認現況後回報

---

## 🔧 Resources

- **Notion Dashboard**: [Master Data Dashboard](https://www.notion.so/319ab0806d5c81b0815ee6e30136a251)
- **Google Sheets**: [MAPLAB_外燴系統_v0.1](https://docs.google.com/spreadsheets/d/1fn_woqYI_RY9ggGHVidB5SMygAzwe4CL_SOPLhe91Jg/edit)
- **AI Collaboration**: Claude + GPT + GitHub Copilot

---

**Version**: 1.2.0
**Created**: 2026-03-04
**Updated**: 2026-03-11 09:00
**Author**: MAPLAB x Claude x GPT x Copilot


## 📋 Changelog

### v0.8 — 2026-03-15 08:25 | R1完成：11月剩餘5張訂單掃描 + 品項歸檔

**角色**: Claude A1（主執行）  
**本次主線**: 品項歸檔任務 R1 — 11月剩餘5張訂單掃描

**已完成**:
- 讀取 GitHub README 取得技能與作業準則
- 讀取 Notion Dashboard 確認現況（v0.7++ 狀態：Items 223筆，row 223）
- 掃描 2025年11月剩餘5張訂單：11/15 1000/25人、11/16 1000/20人、11/23 800/16人、11/9 700/15人、11/10 700/25人
- Items sheet 新增 22 筆品項（APP086-094, MAIN013, DST111-118, BEV026-029）
- DST110（row 233）補完所有欄位
- 修正多處資料錯誤（category offset、unit tripled、batch_size doubled、extra row）
- 修正 B228（MAIN010 category 被覆蓋）
- Items 總計：255 - 1（標題行）= **254 筆有效品項**

**Checkpoint 2026-03-15 08:25 — v0.8**
完成: R1 11月剩餘5張訂單，22筆新品項（APP086-094, MAIN013, DST111-118, BEV026-029），Items共254筆
待執行: R2 — 10月訂單（10/16科林研發, 10/31 candy, 10/3開幕等~10張）
接手線索: Items sheet 從 row 256 繼續，APP095+, DST119+, BEV030+, MAIN014+
已知問題: E column (default_cost) 新增行皆已確認為0
