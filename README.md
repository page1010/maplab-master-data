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
ITEM_MASTER ←── PRICE_MASTER   (1:N)
ITEM_MASTER ←── ASSET_MASTER   (1:N)
ITEM_MASTER ←── CONTENT_MASTER (1:1 or null)
MENU_MASTER ──→ MENU_ITEMS ←── ITEM_MASTER (N:M)
```

## 🏷️ Naming Convention

**item_id**: `{TYPE}-{SUBTYPE}-{SEQ3}`
- Example: `DES-MAC-001` (法式馬卡龍)

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

**Version**: 1.0.0  
**Created**: 2026-03-04  
**Author**: MAPLAB x Claude
