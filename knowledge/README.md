# Knowledge — 知識庫總入口

> KeroroTeam 知識庫 — 3 秒內找到任何資料

## 📁 目錄結構（3層深度）

```
knowledge/
├── README.md                    ← 你在這裡
├── _index/
│   └── 00_master-index.md      ← 全域索引
├── common/                      ← 跨專案共享知識
├── hardware/                   ← 硬體相關
├── software/                   ← 軟體相關
├── firmware/                    ← 韌體相關
├── tools/                      ← 工具相關
└── processes/                  ← 流程方法論
```

## 🎯 目標

- **3 秒內找到任何資料**
- **3 層深度** — L1領域 → L2主題 → L3文件
- **ID + 標籤混合** — ID 提供定位感，標籤提供横切搜尋

## 🏷️ 命名規範

| 類型 | 格式 | 範例 |
|------|------|------|
| L1 資料夾 | `kebab-case` | `firmware`, `common` |
| 文件 | `{ID}_{slug}.md` | `FW-001_fwupd-guide.md` |
| 標籤 | `#lowercase` | `#usb`, `#debug` |

**ID 前綴：**
- `HW-` = hardware
- `SW-` = software
- `FW-` = firmware
- `TL-` = tools
- `PR-` = processes
- `CM-` = common

## 🔍 如何找到資料

1. **依領域** → 進入對應 L1 目錄 → README
2. **依 ID** → 查詢 `_index/00_master-index.md`
3. **依關鍵字** → `Ctrl+Shift+F` 全文搜尋

## 👥 維護責任

| 成員 | 負責領域 |
|------|---------|
| Dororo | 文件驗收 |
| Kururu | 知識分類優化 |
| 全體 | 貢獻知識 |

## 📖 快速連結

- [common/](common/) — 跨專案共享
- [firmware/](firmware/) — 韌體知識
- [hardware/](hardware/) — 硬體知識
- [_index/00_master-index.md](_index/00_master-index.md) — 全域索引

---

> 「知識庫的目的不是分類優雅，而是需要時找得到。」— Kururu
