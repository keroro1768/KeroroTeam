# T001 研究報告：KeroroTeam 知識庫階層式查找系統

**研究成員：** Kururu  
**日期：** 2026-03-29  
**目標：** 建立 3 秒內可找到任意資料的階層式知識庫

---

## 1. 現況分析

### 目前結構
```
knowledge/
├── M487/      ← 空資料夾
├── FWUPD/     ← 空資料夾
├── USB/       ← 空資料夾
└── tools/     ← 空資料夾
```

### 問題
- 無統一的命名規範
- 缺乏跨專案共享機制
- 搜尋完全依賴人工記憶
- 無索引機制

---

## 2. 知識庫架構設計

### 2.1 推薦：3 層深度結構

| 層級 | 範例 | 用途 |
|------|------|------|
| L1 領域 | `hardware/` | 最高層分類 |
| L2 主題 | `usb/` | 技術主題 |
| L3 項目 | `001_usb-firmware-update.md` | 具體文件 |

**為什麼不選 2 層？**
- 2 層在領域擴展時會膨脹成爛攤子
- 例：`usb_firmware`, `usb_debug`, `usb_tools` 全擠在 L1

**為什麼不選 4 層？**
- 4 層路徑太長，違背「3 秒找到」的目標
- 心理負擔高，易迷路

### 2.2 Topic/Category 命名規範

```
規則：
1. 全小寫，英文優先
2. 統一使用 dash(-) 分隔（不要底線或 camelCase）
3. 數字前綴用於排序：001_, 002_
4. 避免使用特殊字符
5. 中文資料夾可並存，但英文 URL 更通用
```

**領域命名（L1）：**
- `hardware/` — 硬體相關
- `software/` — 軟體相關
- `firmware/` — 韌體相關
- `tools/` — 工具相關
- `processes/` — 流程方法論
- `common/` — 跨專案共享

---

## 3. ID 索引法 vs 標籤法

### 3.1 結論：兩者混合，以 ID 為骨幹

```
ID 格式：{領域代碼}-{序號}

領域代碼：
  HW = hardware
  SW = software
  FW = firmware
  TL = tools
  PR = processes
  CM = common

範例：
  FW-001  → 韌體更新流程
  HW-002  → USB 協議基礎
  SW-003  → 驅動程式開發
```

**為什麼混合？**
- ID 提供明確的「位置感」：知道在 FW-001 就知道在 firmware 領域
- 標籤提供「横切」能力：同一概念可跨多領域被找到

**標籤使用場景：**
- 文件抬頭的 `#tags`
- Slack/Discord 中的 `#usb` `#debug` 搜尋
- NOT 作為主要導航

---

## 4. 搜尋機制

### 4.1 推薦方案：GitHub Wiki + 本地資料夾 + README 索引

| 方案 | 優點 | 缺點 | 適合場景 |
|------|------|------|----------|
| **GitHub Wiki** | 協作友好，版本控制 | 需網路，搜尋功能弱 | 文件協作 |
| **Notion** | 強大搜尋，漂亮介面 | 需付費，脫離 Git | 知識沉澱 |
| **資料夾+README** | 簡單，Git 原生 | 全域搜尋靠工具 | **團隊首選** |

### 4.2 實作建議：雙軌制

```
本地（主要）：
  knowledge/
  ├── README.md              ← 總索引（所有領域入口）
  ├── hardware/
  │   └── README.md          ← 硬體領域索引
  ├── common/
  │   ├── README.md          ← 共享知識索引
  │   └── 001_glossary.md    ← 通用術語表
  └── _index/
      └── 00_master-index.md  ← 全域搜尋索引（可用 script 產生）

GitHub（協作）：
  Wiki Pages → 存放較長的知識文章
  Issues → 碎片化知識收集
```

### 4.3 搜尋輔助工具

```bash
# Windows 本地搜尋
Get-ChildItem -Recurse -Filter "*.md" | Select-String "USB"

# GitHub 搜尋（安裝 gh）
gh search code "USB firmware" --repo keroro1768/KeroroTeam
```

---

## 5. 跨專案知識共享

### 5.1 推薦：`common/` 集中式 + 符號引用

```
KeroroTeam/
├── knowledge/
│   ├── common/              ← 共享知識（single source of truth）
│   │   ├── 001_glossary.md  ← 術語表
│   │   ├── 002_usb-basics.md
│   │   └── 003_debug-methods.md
│   ├── M487_ScsiTool/       ← 專案獨立知識
│   │   ├── README.md
│   │   └── hardware/        ← 引用 common/001_usb-basics.md
│   └── FWUPD/
│       └── hardware/        ← 同上
```

### 5.2 共享原則

1. **唯一真相來源（Single Source of Truth）**
   - `common/` 是源頭，各專案引用
   - 不要在專案內複製 `common/` 的內容

2. **引用格式**
   ```markdown
   ## 相關知識
   - [USB 基礎概念](../../common/002_usb-basics.md)
   ```

3. **維護責任**
   - `common/` 由全體成員共同維護
   - 修改前先討論，確保跨專案相容

---

## 6. 推薦目錄結構

```
D:\AiWorkSpace\KeroroTeam\
└── knowledge/
    ├── README.md                    ← 總入口（必讀）
    ├── _index/
    │   ├── 00_master-index.md        ← 所有文件索引（script 產生）
    │   └── 01_topic-map.md          ← 主題地圖（誰負責什麼）
    ├── common/                      ← 跨專案共享知識
    │   ├── README.md
    │   ├── PR-001_glossary.md       ← 術語表
    │   ├── PR-002_debug-methods.md   ← 通用Debug方法
    │   └── PR-003_usb-basics.md     ← USB基礎知識
    ├── hardware/
    │   ├── README.md
    │   ├── HW-001_usb-protocol.md
    │   └── HW-002_spi-basics.md
    ├── software/
    │   ├── README.md
    │   └── SW-001_driver-dev.md
    ├── firmware/
    │   ├── README.md
    │   ├── FW-001_fwupd-guide.md
    │   └── FW-002_m487-firmware.md
    ├── tools/
    │   ├── README.md
    │   └── TL-001_scsi-tool.md
    └── processes/
        ├── README.md
        └── PR-001_task-flow.md
```

---

## 7. 實作步驟

### Phase 1：建立框架（Day 1）
```bash
# 建立目錄結構
mkdir -p knowledge/_index
mkdir -p knowledge/common
mkdir -p knowledge/hardware
mkdir -p knowledge/software
mkdir -p knowledge/firmware
mkdir -p knowledge/tools
mkdir -p knowledge/processes
```

### Phase 2：建立索引文件（Day 1-2）
1. 建立 `knowledge/README.md` — 總入口
2. 建立各 L1 領域的 `README.md`
3. 建立 `_index/00_master-index.md`

### Phase 3：遷移現有知識（Day 2-3）
1. 盤點 M487_ScsiTool 的現有文件
2. 分類遷移到對應目錄
3. 建立 `common/` 的核心共享文件

### Phase 4：建立維護機制（Day 3-4）
1. 建立貢獻規範（每個文件抬頭加 Tags）
2. 設定定期 index 更新（script）
3. 文件變更時更新 `CHANGELOG.md`

### Phase 5：搜尋優化（Day 4+）
1. 啟用 GitHub Wiki（可選）
2. 或使用 `grep` + `rg`（ripgrep）加速本地搜尋
3. 考慮使用 `docsify` 建立本地文件站

---

## 8. 命名規範總結

| 類型 | 格式 | 範例 |
|------|------|------|
| L1 資料夾 | `kebab-case` | `firmware`, `common` |
| L2 資料夾 | `kebab-case` | `usb-protocol`, `debug-tools` |
| 文件 | `{ID}_{slug}.md` | `FW-001_fwupd-guide.md` |
| 標籤 | `#lowercase` | `#usb`, `#debug`, `#m487` |

---

## 9. 成功指標

- [ ] 新成員能在 3 分鐘內理解目錄結構（非 3 秒，學習曲線正常）
- [ ] 任何知識點可在 2 次點擊內找到（3 秒鐘達成）
- [ ] `common/` 目錄有至少 5 個共享文件
- [ ] 每個領域有至少 3 個文件

---

**研究結論：**
3 層結構 + ID 索引 + `common/` 共享是最佳平衡。
過於複雜會增加維護成本，過於簡單則無法擴展。

> 「知識庫的目的不是分類優雅，而是需要時找得到。」— Kururu
