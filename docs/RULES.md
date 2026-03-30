# KeroroTeam 工作規則

> 最後更新：2026-03-30

---

## 進行中的專案

| 專案 | 路徑 |
|------|------|
| M487_ScsiTool | `D:\AiWorkSpace\M487_ScsiTool\` |
| FWUPD | `D:\AiWorkSpace\FWUPD\` |

> Heartbeat 自動掃描 `D:\AiWorkSpace\` 下所有有 `tasks/` 的資料夾。

---

## 各專案職責

### M487_ScsiTool

**描述：** M487 USB 複合裝置（MSC + HID I2C Bridge）  
**任務資料夾：** `tasks\`  
**目前進度：** Debug 整備矩陣（T024-T034 完成），等待實體硬體測試

### FWUPD

**描述：** Linux 韌體更新框架（fwupd）知識庫與 Plugin 開發  
**知識庫位置：** `D:\AiWorkSpace\FWUPD\`  
**目前進度：**
- F001：✅ 知識庫研究完成（Kururu + Dororo 驗證）
- F002：🔄 Plugin 實作進行中（Giroro 修復 Critical 問題中）

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

## FWUPD 專屬工作流程

### 知識庫建置流程

```
Caro 提出需求
    ↓
Keroro 指派 Kururu 研究
    ↓
Kururu 產出文件（存入 doc/I2C_HID_Company_Product/）
    ↓
Dororo 驗證（與官方文件交叉比對）
    ↓
修正錯誤
    ↓
Commit + Push
    ↓
更新 TaskList.md
```

### Plugin 開發流程

```
F001 研究完成
    ↓
Keroro 指派 Giroro 實作 Plugin
    ↓
Giroro 參考 elantp Plugin（最佳範本）
    ↓
產出 Plugin 原始碼（plugin/company-i2c-hid/）
    ↓
Dororo 交叉比對驗證（與 elantp 原始碼對照）
    ↓
發現問題 → 使命必達模式啟動
    ↓
Giroro 修復 Critical/High 問題
    ↓
Dororo 重新驗證
    ↓
直到完美通過
```

### 多層次測試策略（虛擬硬體）

| 層次 | 方案 | 需要硬體 | 優先順序 |
|------|------|----------|----------|
| 第一層 | Self-Test Framework | ❌ | ✅ 立即可行 |
| 第二層 | JSON 模擬設備定義 | ❌ | 🔄 本週內 |
| 第三層 | 離線 .cab 安裝測試 | ✅ | ⏳ 硬體就緒後 |

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

*KeroroTeam 工作規則 — 2026-03-30*
