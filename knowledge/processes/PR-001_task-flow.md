# PR-001 Task Flow — 任務流程

> 最後更新：2026-03-29
> 標籤：#processes #workflow #task-management

---

## 任務狀態

| 標記 | 意義 |
|------|------|
| ⏳ 待處理 | 等待執行 |
| 🔄 進行中 | 正在執行 |
| ✅ 完成 | 已完成 |
| ⏸️ 暫停 | 硬體 Pending |
| ⏰ 延後 | 暫時擱置 |

## 任務指派

| 成員 | 負責 |
|------|------|
| Keroro | Commander，統籌協調 |
| Giroro | 複雜任務（Firmware）|
| Tamama | 簡單任務（Windows Tool）|
| Dororo | 驗證測試 |
| Kururu | 優化研究 |

## 執行流程

```
任務指派
    ↓
成員執行
    ↓
更新 WORKLOG.md
    ↓
完成後更新 VERIFY.md
    ↓
Dororo 驗收
    ↓
完成
```

## 任務資料夾結構

```
tasks/T001/
├── Task.md       ← 任務說明
├── PLAN.md      ← 執行計畫
├── WORKLOG.md   ← 工作日誌
└── VERIFY.md   ← 驗收結果
```

## 命名規範

| 格式 | 範例 |
|------|------|
| 任務資料夾 | `T001/` |
| 文件 | `Task.md`, `PLAN.md` |

---

## 詳見

- tasks/ 目錄
