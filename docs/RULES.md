# KeroroTeam 工作規則

> 最後更新：2026-03-29
> 所有成員必須遵循

---

## 1. 任務執行流程

```
Keroro 指派任務
    ↓
成員執行
    ↓
更新 WORKLOG.md
    ↓
完成後更新 VERIFY.md
    ↓
Dororo 驗收
```

---

## 2. Review 會議規則

### 每週 Review 會議

**主持：** Keroro  
**參與：** 全體成員  
**頻率：** 每週一次

### Review 檢查清單

#### ✅ 檢查所有正在進行的專案

| 檢查項目 | 行動 |
|---------|------|
| 任務有未完成？ | 追蹤原因 |
| 有被阻塞的任務？ | 解決阻塞 |
| 有新 Enhancement 可提案？ | 建立 E0XX |

#### ✅ 檢查 Enhancement Backlog

| 檢查項目 | 行動 |
|---------|------|
| 有超過 30 天未處理的 Enhancement？ | 提高優先度 |
| 有新的優化構想？ | 提案加入 Backlog |
| 上週完成的 Enhancement？ | 記錄學到的 |

#### ✅ 檢查知識庫

| 檢查項目 | 行動 |
|---------|------|
| 有新知識可以入库？ | 更新 knowledge/ |
| 文件有過時？ | 更新或移除 |

---

## 3. Enhancement 提案規則

### 提案時機

- 任何時候想到「這裡可以更好」
- Review 會議時提出
- 不需要等到有時間，提案只是構想

### 提案格式

```
tasks/enhancement/E001_xxx/
├── Task.md       ← 強化構想
├── PLAN.md      ← 執行計畫
├── WORKLOG.md   ← 工作日誌
└── VERIFY.md   ← 完成後填寫
```

### 負責人分配

| 成員 | 負責 |
|------|------|
| Dororo | 驗收 Enhancement 價值 |
| Kururu | 提出優化構想、分析 |
| Giroro | 執行複雜 Enhancement |
| Tamama | 執行簡單 Enhancement |

---

## 4. 專案狀態追蹤

### projects.md

所有正在進行的專案都列在 `projects.md`。

### 任務狀態

| 標記 | 意義 |
|------|------|
| ⏳ 待處理 | 等待執行 |
| 🔄 進行中 | 正在執行 |
| ✅ 完成 | 已完成 |
| ⏸️ 暫停 | 硬體 Pending |
| ⏰ 延後 | 暫時擱置 |

---

## 5. 知識共享規則

### 開始任務前

**必須先在 `D:\AiWorkSpace\KeroroTeam\knowledge\` 查找方法**

### 任務完成後

**將學到的知識更新到 knowledge/**

---

## 6. 違規處理

| 違規 | 處理 |
|------|------|
| 任務遲遲未完成 | Keroro 了解原因 |
| Enhancement 被忽略 | 提高優先度 |
| 知識未共享 | 提醒並補上 |

---

## 7. 檔案命名規範

| 類型 | 格式 | 範例 |
|------|------|------|
| 任務資料夾 | `T0XX/` | `T001/` |
| Enhancement | `E0XX_xxx/` | `E001_optimize-usb/` |
| 知識文件 | `{ID}_{slug}.md` | `FW-001_fwupd-guide.md` |

---

## 8. Git 提交規範

```
feat: 新功能
fix: 錯誤修復
refactor: 重構
docs: 文件變更
test: 測試
chore: 維護
```

---

*KeroroTeam 工作規則 — 2026-03-29*
