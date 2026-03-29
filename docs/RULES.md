# KeroroTeam 工作規則

> Heartbeat 每 30 分鐘自動讀取此檔案，確保團隊遵循統一流程。
> 最後更新：2026-03-30

---

## 1. 專案管理方式

### 當前進行中的專案

| 專案 | 路徑 |
|------|------|
| M487_ScsiTool | `D:\AiWorkSpace\M487_ScsiTool\` |

> Heartbeat 自動掃描 `D:\AiWorkSpace\` 下所有有 `tasks/` 的資料夾，列為專案。
> 不需要手動維護清單。

---

## 2. 任務管理方式

**全部本地資料夾，不使用 GitHub Projects / Issues**

所有任務都在各專案的 `tasks/` 資料夾，Enhancement 在 `tasks/enhancement/`。

```
D:\AiWorkSpace\M487_ScsiTool\
└── tasks/
    ├── TaskList.md         ← 專案任務總覽
    ├── Task_Template.md    ← 任務模板
    ├── Verify_Template.md  ← 驗收模板
    ├── Readme.md           ← 任務說明
    ├── T001/               ← 任務資料夾
    │   ├── Task.md         ← 任務描述
    │   ├── PLAN.md         ← 執行計畫
    │   ├── WORKLOG.md      ← 工作日誌
    │   └── VERIFY.md       ← 驗收結果
    ├── T002/
    └── enhancement/
        └── E001_xxx/       ← Enhancement 提案
```

| 工具 | 用處 |
|------|------|
| `tasks/TaskList.md` | 專案任務總覽（Heartbeat 讀取）|
| `tasks/Task_Template.md` | 新任務模板 |
| `tasks/Verify_Template.md` | 驗收結果模板 |
| `tasks/T0XX/Task.md` | 任務描述 |
| `tasks/T0XX/PLAN.md` | 執行計畫 |
| `tasks/T0XX/WORKLOG.md` | 工作日誌 |
| `tasks/T0XX/VERIFY.md` | 驗收結果 |
| `tasks/enhancement/E0XX/` | Enhancement |

---

## 3. 任務執行流程

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

## 4. 任務指派

| 成員 | 職責 |
|------|------|
| **Keroro** | Commander（統籌協調）|
| **Giroro** | 複雜任務相關，需要使用 Claude Code |
| **Tamama** | 簡單任務相關，需要 OpenCode |
| **Dororo** | 驗證測試相關 |
| **Kururu** | 優化相關，提供建議交 Keroro 決策執行 |

---

## 5. 任務狀態

| 標記 | 意義 |
|------|------|
| ⏳ 待處理 | 等待執行 |
| 🔄 進行中 | 正在執行 |
| ✅ 完成 | 已完成 |
| ⏸️ 暫停 | 硬體 Pending |
| ⏰ 延後 | 暫時擱置 |

---

## 6. Enhancement 提案規則

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

## 7. Review 會議規則

### 每週 Review 會議

**主持：** Keroro  
**參與：** 全體成員  
**頻率：** 每週一次

### Review 檢查清單

#### 檢查所有正在進行的專案

| 檢查項目 | 行動 |
|---------|------|
| 任務有未完成？ | 追蹤原因 |
| 有被阻塞的任務？ | 解決阻塞 |
| 有新 Enhancement 可提案？ | 建立 E0XX |

#### 檢查 Enhancement Backlog

| 檢查項目 | 行動 |
|---------|------|
| 有超過 30 天未處理的 Enhancement？ | 提高優先度 |
| 有新的優化構想？ | 提案加入 Backlog |
| 上週完成的 Enhancement？ | 記錄學到的 |

#### 檢查知識庫

| 檢查項目 | 行動 |
|---------|------|
| 有新知識可以入库？ | 更新 knowledge/ |
| 文件有過時？ | 更新或移除 |

---

## 8. Heartbeat 自動化流程

```
Heartbeat 觸發（每 30 分鐘）
    ↓
① 讀取 KeroroTeam 規則
    └── D:\AiWorkSpace\KeroroTeam\docs\RULES.md
    ↓
② 掃描所有專案
    └── D:\AiWorkSpace\*
        └── 讀取 tasks\TaskList.md（專案任務總覽）
    ↓
③ 根據 RULES.md + TaskList.md 執行相應動作
        ├── 有 🔄 → 回報進度
        ├── 有 ⏳ → 自動執行
        └── 無 ⏳ → Review & 新增任務
```

---

## 9. 知識共享規則

### 開始任務前

**必須先在 `D:\AiWorkSpace\KeroroTeam\knowledge\` 查找方法**

### 任務完成後

**將學到的知識更新到 knowledge/**

---

## 10. 檔案命名規範

| 類型 | 格式 | 範例 |
|------|------|------|
| 任務資料夾 | `T0XX/` | `T001/` |
| Enhancement | `E0XX_xxx/` | `E001_optimize-usb/` |
| 知識文件 | `{ID}_{slug}.md` | `FW-001_fwupd-guide.md` |

---

## 11. Git 提交規範

```
feat: 新功能
fix: 錯誤修復
refactor: 重構
docs: 文件變更
test: 測試
chore: 維護
```

---

## 12. 違規處理

| 違規 | 處理 |
|------|------|
| 任務遲遲未完成 | Keroro 了解原因 |
| Enhancement 被忽略 | 提高優先度 |
| 知識未共享 | 提醒並補上 |

---

*KeroroTeam 工作規則 — 2026-03-30*
