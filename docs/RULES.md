# KeroroTeam 工作規則

> 最後更新：2026-03-30

---

## 進行中的專案

| 專案 | 路徑 |
|------|------|
| AutoVerifier | `D:\Aiworkspace\AutoVerifier\` |
| USBBusMonitor | `D:\Aiworkspace\USBBusMonitor\` |

> Heartbeat 自動掃描 `D:\AiWorkSpace\` 下所有有 `tasks/` 的資料夾。

---

## 各專案職責

### AutoVerifier

**描述：** 自動化驗證框架（WinForms + JSON TestSuite）  
**路徑：** `D:\Aiworkspace\AutoVerifier\`  
**GitHub：** `https://github.com/RinRyanJi/AutoVerifier`  
**架構：**
- `Engine/` — 測試引擎核心（TestSuiteRunner, StepByStepSession）
- `Gui/` — WinForms GUI
- `TestSuites/` — JSON 測試套件（*.test.json）
- `Infra/` — 底層設施工具（IPC, Screenshot, UIAuto, LogReader）
- `Report/` — Markdown / XUnit 報告生成器
- `docs/` — 完整使用文件  
**目前進度：** ✅ 初始 commit 完成，已推送至 GitHub（2026-03-30）

### USBBusMonitor

**描述：** USB Bus Monitor（Serial Filter Driver）  
**路徑：** `D:\Aiworkspace\USBBusMonitor\`  
**任務資料夾：** `tasks\`  
**目前進度：** Phase 3 執行中（USBBusMonitor Serial Filter）

---

## 專案選擇與任務執行流程

**每次 Heartbeat 執行任務前，依照以下流程：**

```
Heartbeat 觸發
    ↓
讀取 KeroroTeam/docs/RULES.md（專案清單）
    ↓
找到該專案目錄所在位置
    ↓
執行專案任務
    ↓
在專案目錄下，找到 tasks/ 目錄
    ↓
根據 TaskList.md 找到任務
    ↓
讀取 TaskRules.md（該專案的任務規則）← 重要！
    ↓
按照 TaskRules.md 的規則執行
```

**每次執行任務前，必須先讀取該專案的 `TaskRules.md`**

---

## 使命必達命令（Keroro 最高指令）

### 定義

當 Keroro 發布「**使命必達**」命令時，代表：
- **全權負責**：執行者擁有完整授權直到任務完成
- **完美標準**：Dororo 的驗證與舉證必須完美通過
- **不可阻擋**：所有成員努力克服萬難直到達成

### 使命必達流程

```
Keroro 發布「使命必達」
    ↓
Giroro 執行（中間進度主動匯報）
    ↓
Dororo 驗證（研究階段已完成）
    ↓
驗證失敗 → Giroro 繼續修復
    ↓
驗證失敗 → 重複直到通過
    ↓
✅ 完美通過 → 任務完成
```

### Dororo 驗證標準

Dororo 驗證時，必須提供：
- **事實查核**：文件中每項技術資訊是否屬實
- **來源確認**：官方文件或原始碼出處
- **完整性評估**：是否缺少關鍵資訊
- **符號說明**：
  - ✅ = 資料正確、來源可靠、內容完整
  - ⚠️ = 大致正確但需要補充或修正
  - ❌ = 資料有誤或來源不可靠

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

### 研究 → 實作 → 驗證 循環

```
 Kururu 研究（收集資料、分析）
         ↓
   Dororo 驗證研究（正確性、完整性）
         ↓
   Giroro 實作（根據研究結果）
         ↓
   Dororo 驗證實作（交叉比對）
         ↓
   驗證失敗 → Giroro 修復
         ↓
   直到 Dororo 完美通過
```

---

---

## 任務狀態

| 標記 | 意義 |
|------|------|
| ⏳ 待處理 | 等待執行 |
| 🔄 進行中 | 正在執行 |
| ✅ 完成 | 已完成（通過驗證）|
| ⏸️ 暫停 | 硬體/外部 Pending |
| ⏰ 延後 | 暫時擱置 |

---

## 成員職責

| 成員 | 職責 |
|------|------|
| **Keroro** | Commander（統籌協調）|
| **Giroro** | 複雜任務相關，需要使用 Claude Code |
| **Tamama** | 簡單任務相關，需要 OpenCode |
| **Dororo** | 驗證測試相關（確保完美通過）|
| **Kururu** | 優化相關，提供建議交 Keroro 決策執行 |

### 自動指派邏輯

| 條件 | 指派 |
|------|------|
| 複雜任務、需要 Claude Code | Giroro |
| 簡單任務、需要 OpenCode | Tamama |
| 驗證測試 | Dororo |
| 研究、分析、優化 | Kururu |
| 使命必達命令 | Giroro（主）+ Dororo（驗證）|

---

## 任務資料夾結構

每個專案的 `tasks/` 目錄結構：

```
tasks/
├── TaskList.md         ← 專案任務總覽（Heartbeat 讀取）
├── TaskRules.md        ← 本專案的任務規則
├── Task_Template.md    ← 新任務模板
├── Verify_Template.md  ← 驗收模板
├── T001/               ← 任務編號
│   ├── Task.md         ← 任務描述
│   ├── PLAN.md         ← 執行計畫
│   ├── WORKLOG.md      ← 工作日誌
│   └── VERIFY.md       ← 驗收結果
└── enhancement/        ← Enhancement 提案
    └── E001_xxx/
```

---

## Git 操作規範

所有修改後的檔案都必須：
1. `git add`
2. `git commit -m "類型: 簡要說明"`
3. `git push`

**推送時如果遲遲無輸出**：設定遙控器 URL 使用 Token
```bash
git remote set-url origin https://ghp_TOKEN@github.com/OWNER/REPO.git
git push
```

---

---

## Knowledge 文件更新記錄

當 knowledge/ 新增文件時，同步更新於此。

| 日期 | ID | 文件 | 說明 |
|------|----|------|------|
| 2026-03-29 | TL-002 | [TL-002_winget-devtools.md](../knowledge/tools/TL-002_winget-devtools.md) | Winget 安裝 VS/WDK/SDK 開發工具鏈 |

---

*KeroroTeam 工作規則 — 2026-03-30*
