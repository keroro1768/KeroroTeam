# KeroroTeam 工作規則

> 最後更新：2026-03-30

---

## 進行中的專案

| 專案 | 路徑 |
|------|------|
| M487_ScsiTool | `D:\AiWorkSpace\M487_ScsiTool\` |

> Heartbeat 自動掃描 `D:\AiWorkSpace\` 下所有有 `tasks/` 的資料夾。

---

## 各專案職責

### M487_ScsiTool

**描述：** M487 USB 複合裝置（MSC + HID I2C Bridge）  
**任務資料夾：** `tasks\`  
**目前進度：** Debug 整備矩陣（T024-T034 完成），等待實體硬體測試

---

## 任務執行原則

**每次執行任務前，必須先讀取該專案的 `TaskRules.md`**

```
Heartbeat 觸發
    ↓
讀取 KeroroTeam/docs/RULES.md（專案清單）
    ↓
根據 TaskList.md 找到任務
    ↓
讀取 TaskRules.md（該專案的任務規則）← 重要！
    ↓
按照 TaskRules.md 的規則執行
```

---

## 通用工作流程

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

## 任務狀態

| 標記 | 意義 |
|------|------|
| ⏳ 待處理 | 等待執行 |
| 🔄 進行中 | 正在執行 |
| ✅ 完成 | 已完成 |
| ⏸️ 暫停 | 硬體 Pending |
| ⏰ 延後 | 暫時擱置 |

---

## 成員職責

| 成員 | 職責 |
|------|------|
| **Keroro** | Commander（統籌協調）|
| **Giroro** | 複雜任務相關，需要使用 Claude Code |
| **Tamama** | 簡單任務相關，需要 OpenCode |
| **Dororo** | 驗證測試相關 |
| **Kururu** | 優化相關，提供建議交 Keroro 決策執行 |

---

*KeroroTeam 工作規則 — 2026-03-30*
