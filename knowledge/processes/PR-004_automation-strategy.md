# PR-004 Automation Strategy — KeroroTeam 自動化開發策略研究報告

> **文件狀態：** 草稿
> **建立日期：** 2026-03-29
> **負責人：** Kururu（研究）/ Keroro（決策）
> **標籤：** #processes #automation #github #research

---

## 📋 執行摘要

KeroroTeam 目前使用**純資料夾式任務管理**，GitHub 僅作為靜態知識庫托管。雖然已有 Enhancement Backlog 流程藍圖，但 GitHub Projects、Actions、Issue 追蹤**均未啟用**。

**核心問題：**
- 任務沒有自動化追蹤
- Enhancement 沒有自動化提案機制
- Review 會議沒有自動化提醒
- 進度沒有視覺化儀表板

**推薦方案：** 採用「**雙軌 Hybrid 方案**」——維持 `tasks/` 資料夾作為離線知識沉積，同時將 GitHub Issues + Projects 作為**真正的任務追蹤系統**，兩者透過命名慣例（連結）手動同步。

---

## 1. 現況分析

### 1.1 目前資料夾結構

```
D:\AiWorkSpace\KeroroTeam\
├── docs/                      ← 團隊文件（RULES.md）
├── knowledge/                 ← 知識庫
│   ├── _index/               ← 主索引
│   ├── common/               ← 通用知識（USB、Debug 方法）
│   ├── firmware/             ← 韌體（M487、fwupd）
│   ├── hardware/             ← 硬體（ARM Cortex-M4、USB 協定）
│   ├── processes/            ← 流程文件（Task Flow、Enhancement Backlog）
│   ├── software/             ← 軟體（Driver Dev）
│   └── tools/                ← 工具設定（VSCode）
├── tasks/                     ← 任務管理
│   └── enhancement/          ← Enhancement Backlog（提案資料夾）
├── projects.md                ← 專案總覽
└── README.md
```

### 1.2 現有流程文件

| 文件 | 用途 | 現況 |
|------|------|------|
| `PR-001_task-flow.md` | 任務執行流程 | ✅ 已建立 |
| `PR-003_enhancement-backlog.md` | Enhancement 提案機制 | ✅ 已建立，尚未連結 GitHub |
| `PR-002_continuous-optimization.md` | 持續優化流程 | ✅ 已建立 |

### 1.3 GitHub 使用現況

| 項目 | 現況 |
|------|------|
| GitHub Repo | ✅ `keroro1768/KeroroTeam` 存在 |
| GitHub Projects | ❌ 未建立 |
| GitHub Issues | ❌ 未啟用 |
| GitHub Actions | ❌ 未設定 |
| GitHub Pages | ❌ 未啟用 |

### 1.4 什麼 GitHub 能自動化，什麼必須靠人工

| 能自動化 ✅ | 必須靠人工 ❌ |
|-----------|--------------|
| Issue 自動歸入 Project 欄位 | 判斷任務價值與優先順序 |
| Issue 開啟 → 自動移至 "In Progress" | 撰寫高品質任務描述 |
| Issue 關閉 → 自動移至 "Done" | 人工 code review |
| Issue 到期前自動通知（via Actions）| 決定 Enhancement 是否執行 |
| PR 開啟時自動要求 review | 處理硬體相關的驗證 |
| 自動化測試（需有 Actions）| 團隊面對面溝通 |
| 自動化文件生成 | 創意發想 |

---

## 2. GitHub 原生自動化能力

### 2.1 GitHub Projects（Kanban 板）

**新版 GitHub Projects（2022+）** 特性：

- **自動化規則（Automation）**：可設定條件觸發自動移動卡片
  - Issue 開啟 → 自動移至指定欄位
  - Issue 關閉 → 自動移至 Done
  - Label 新增 → 自動移至指定欄位
  - Assignee 變更 → 觸發通知
- **多視圖**：Table / Board / Roadmap
- **欄位自訂**：Iteration（對應 Sprint）、Date、Number、Text、Single Select、Milestone
- **與 Issue 深度整合**：直接在 Issue 內看 Project 狀態

**vs 純資料夾：**

| 能力 | 資料夾 | GitHub Projects |
|------|--------|----------------|
| 視覺化 Kanban | ❌ | ✅ |
| 自動狀態遷移 | ❌ | ✅ |
| 多人同時編輯 | ⚠️（衝突）| ✅ |
| 搜尋/篩選任務 | 靠檔名 | ✅ 豐富篩選 |
| 進度追蹤 | ❌ | ✅ |
| 離線存取 | ✅ | ❌（需網路）|

### 2.2 GitHub Issues + Labels + Milestones

**Labels（標籤）建議：**

```
狀態標籤：
- `status:backlog`     — 待處理
- `status:in-progress` — 進行中
- `status:done`        — 已完成
- `status:blocked`    — 阻礙中

類型標籤：
- `type:task`         — 一般任務
- `type:enhancement`  — Enhancement 提案
- `type:bug`          — Bug 回報
- `type:research`     — 研究任務

領域標籤：
- `area:firmware`     — 韌體
- `area:hardware`     — 硬體
- `area:software`     — 軟體
- `area:process`      — 流程

優先級標籤：
- `priority:p0`       — P0（立即處理）
- `priority:p1`       — P1（本 Sprint）
- `priority:p2`       — P2（ backlog）
- `priority:p3`       — P3（未來規劃）
```

**Milestones（里程碑）= Sprint：**

- 用 Milestone 代表一個 Sprint（如 `Sprint-2026-W14`）
- 可設定 Due Date
- Issue 關聯 Milestone 後，Project 可顯示 Sprint 進度

### 2.3 GitHub Actions 能做到什麼

| 自動化場景 | Actions 實現方式 |
|-----------|-----------------|
| Review 會議每週提醒 | `actions/github-script` + Schedule（CRON）|
| Enhancement Issue 開啟時自動通知 | `actions/github-script` + 發 Slack/Discord webhook |
| Issue 標籤變更時自動分類 | `actions/labeler` 或自訂 script |
| Issue Assignee 變更時通知 | 事件觸發 + webhook |
| Issue 接近到期日時提醒 | Schedule + 查詢即將到期 Issue |
| PR 開啟時自動要求 Review | 原生功能（Settings > Branch protection）|
| 自動關閉 30 天未更新的 Issue | Schedule + `actions/close-issue` |
| 自動化測試（CI/CD）| `actions/checkout` + 自訂 script |
| 自動同步 Issue → 本地資料夾 | Schedule + GitHub API + 本地 script（複雜）|

---

## 3. 輕量級自動化選項（推薦）

### 3.1 架構：雙軌 Hybrid 方案

```
┌─────────────────────────────────────────────┐
│           GitHub（雲端，任務追蹤）            │
│  Issues ← → Projects ← → Milestones         │
│  Labels（狀態/類型/領域/優先）               │
│  Actions（自動化規則）                       │
└─────────────────────────────────────────────┘
                    ↕ 手動同步（命名慣例）
┌─────────────────────────────────────────────┐
│           本地（知識沉積）                   │
│  tasks/              ← 任務資料夾            │
│  tasks/enhancement/  ← Enhancement Backlog   │
│  knowledge/          ← 知識庫               │
└─────────────────────────────────────────────┘
```

**核心原則：**
- **GitHub Issues = 真正的任務追蹤**（狀態、歸屬、自動化）
- **本地資料夾 = 知識沉積**（詳細文件、工作日誌、驗收結果）
- **兩邊透過 Issue 編號連結**（如 `T001` 對應 `#1`）

### 3.2 GitHub Projects 設計

**Board 欄位（Kanban）：**

| 欄位 | 自動化規則 |
|------|-----------|
| **Backlog** | Issue 開啟時自動移入 |
| **To Do** | 人工移動 |
| **In Progress** | Assignee 被設定時自動移入 |
| **Review** | Label `status:review` 時移入 |
| **Done** | Issue 關閉時自動移入 |

**Iteration 設定：**

- `Sprint-2026-W14`（每週一 ~ 週日）
- `Sprint-2026-W15`
- ...

### 3.3 `.github/ISSUE_TEMPLATE` 設計

建立 `tasks/` 的 Issue 模板：

**檔案：`D:\AiWorkSpace\KeroroTeam\.github\ISSUE_TEMPLATE\task.md`**

```markdown
---
name: Task
about: 建立新任務
title: '[T???] '
labels: type:task
assignees: ''
---

## 任務名稱

（一句話描述任務）

## 背景

（為什麼要做這個任務）

## 驗收標準

- [ ] 
- [ ] 

## 估計工時

（時數或天數）

## 負責人

（待認領 / @username）

## 本地任務資料夾

（建立後填入，例如：`tasks/T001/`）
```

**檔案：`.github/ISSUE_TEMPLATE/enhancement.md`**

```markdown
---
name: Enhancement
about: 提出 Enhancement 提案
title: '[ENH-???] '
labels: type:enhancement
assignees: ''
---

## Enhancement 名稱

（一句話描述）

## 當前狀態

（描述現有問題或可以改進的地方）

## 預期效益

（做了之後有什麼好處）

## 估計複雜度

- [ ] Low（< 4h）
- [ ] Medium（4-16h）
- [ ] High（> 16h）

## 負責人

（待認領 / @username）
```

### 3.4 Enhancement 提案流程自動化

```
任何人想到 Enhancement
    ↓
在 GitHub 開 Issue（enhancement template）
    ↓
自動歸入 GitHub Project「Backlog」
    ↓
每週 Review（Miro / Google Meet）
    ↓
決定是否執行 → 移至「To Do」→ 認領
    ↓
執行 → PR → Review → Merge → Done
```

---

## 4. 自動化方案對比

### 4.1 方案對比矩陣

| 方案 | 視覺化 | 自動化 | 離線可用 | 多人協作 | 學習成本 | 實作時間 |
|------|--------|--------|----------|----------|---------|---------|
| **純資料夾** | ❌ | ❌ | ✅ | ⚠️ | 低 | 0 |
| **GitHub Issues Only** | ⚠️ | ⚠️ | ⚠️ | ✅ | 低 | 1 天 |
| **GitHub Projects** | ✅ | ✅ | ❌ | ✅ | 中 | 1-2 天 |
| **⭐ Hybrid（推薦）** | ✅ | ✅ | ✅（本地）| ✅ | 中 | 2-3 天 |
| **Linear** | ✅ | ✅ | ❌ | ✅ | 低 | 1 天 |
| **Notion** | ✅ | ⚠️ | ⚠️ | ✅ | 中 | 2-3 天 |

### 4.2 各方案詳細分析

#### 方案 A：純資料夾（現狀）
- **優點：** 簡單、離線、版本控制
- **缺點：** 無自動化、無視覺化、無搜尋
- **結論：** 適合知識沉積，不適合任務追蹤

#### 方案 B：GitHub Issues Only
- **優點：** 簡單、多人協作、自動化（Labels/Milestones）
- **缺點：** 無 Kanban 視覺化
- **結論：** 適合小型團隊（< 5 人）

#### 方案 C：GitHub Projects（完整）
- **優點：** 視覺化 Kanban、自動化規則、Sprint 追蹤
- **缺點：** 需要網路、GitHub 熟悉度
- **結論：** 適合有 GitHub 經驗的團隊

#### 方案 D：Hybrid（推薦）⭐
- **優點：** 兼顧離線（本地）+ 自動化（GitHub）
- **缺點：** 需維持兩邊同步
- **結論：** 最適合 KeroroTeam 現況

---

## 5. 具體實作步驟

### Phase 1：GitHub Projects 設定（1 天）

**Step 1.1：建立 GitHub Project**
```bash
# 在 GitHub UI 或 gh cli 建立
gh project create --owner keroro1768 --title "KeroroTeam" --type board
```

**Step 1.2：設定欄位**
- `Status`（Single Select）：Backlog / To Do / In Progress / Review / Done
- `Iteration`（Iteration）：Sprint-2026-W14、Sprint-2026-W15...
- `Priority`（Single Select）：P0 / P1 / P2 / P3
- `Type`（Single Select）：Task / Enhancement / Bug / Research
- `Area`（Single Select）：Firmware / Hardware / Software / Process
- `Estimate`（Number）：預估小時數
- `Due Date`（Date）：截止日期

**Step 1.3：設定自動化規則**
- Issue 開啟 → Status = Backlog
- Assignee 新增 → Status = In Progress
- Issue 關閉 → Status = Done
- Label `status:review` 新增 → Status = Review

**Step 1.4：建立 Labels**
```bash
gh label create "status:backlog" --color "ededed"
gh label create "status:in-progress" --color "fbca04"
gh label create "status:review" --color "a128ea"
gh label create "status:done" --color "0e8a16"
gh label create "type:task" --color "1d76db"
gh label create "type:enhancement" --color "c5def5"
gh label create "type:bug" --color "d73a4a"
gh label create "type:research" --color "d4a5c9"
gh label create "area:firmware" --color "006b75"
gh label create "area:hardware" --color "0052cc"
gh label create "area:software" --color "6b8e23"
gh label create "area:process" --color "eab5a1"
gh label create "priority:p0" --color "b60205"
gh label create "priority:p1" --color "d93f0b"
gh label create "priority:p2" --color "f9d0c4"
gh label create "priority:p3" --color "cccccc"
```

### Phase 2：Issue Template 設定（0.5 天）

建立 `.github/ISSUE_TEMPLATE/` 資料夾：
- `task.md` — 任務模板
- `enhancement.md` — Enhancement 提案模板
- `bug_report.md` — Bug 回報模板

### Phase 3：GitHub Actions 自動化（0.5 天）

**建立 `.github/workflows/` 工作流：**

#### 3.1 Review 會議每週提醒
```yaml
# .github/workflows/weekly-review-reminder.yml
name: Weekly Review Reminder
on:
  schedule:
    - cron: '0 1 * * 1'  # 每週一 09:00 (GMT+8)
  workflow_dispatch:

jobs:
  remind:
    runs-on: ubuntu-latest
    steps:
      - name: 發送提醒
        run: |
          echo "本週 Sprint Review 提醒"
          # 取得即將到期的 Issue
          gh issue list --assignee @me --milestone "Sprint-$(date +%Y-W%V)" --state open
```

#### 3.2 Enhancement 提案自動通知
```yaml
# .github/workflows/enhancement-notify.yml
name: Enhancement Notification
on:
  issues:
    types: [opened, labeled]

jobs:
  notify:
    runs-on: ubuntu-latest
    if: contains(github.event.issue.labels.*.name, 'type:enhancement')
    steps:
      - name: 顯示新 Enhancement
        run: |
          echo "新 Enhancement 提案：${{ github.event.issue.title }}"
          echo "連結：${{ github.event.issue.html_url }}"
```

#### 3.3 自動化 Sprint 報告
```yaml
# .github/workflows/sprint-report.yml
name: Sprint Report
on:
  schedule:
    - cron: '0 3 * * 5'  # 每週五 11:00 (GMT+8) Sprint 結束前
  workflow_dispatch:

jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - name: Sprint 統計
        run: |
          echo "## Sprint 報告" > $GITHUB_STEP_SUMMARY
          echo "### 完成任務" >> $GITHUB_STEP_SUMMARY
          gh issue list --state closed --milestone "Sprint-$(date +%Y-W%V)" --limit 10 >> $GITHUB_STEP_SUMMARY
          echo "### 進行中任務" >> $GITHUB_STEP_SUMMARY
          gh issue list --state open --milestone "Sprint-$(date +%Y-W%V)" --limit 10 >> $GITHUB_STEP_SUMMARY
```

### Phase 4：本地資料夾對應（0.5 天）

**建立對應關係：**

| GitHub | 本地 |
|--------|------|
| Issue `#1` | `tasks/T001/` |
| Issue `#2` | `tasks/T002/` |
| Enhancement `#10` | `tasks/enhancement/ENH-010.md` |

**在本地任務資料夾加入 GitHub Issue 連結：**

```markdown
<!-- tasks/T001/Task.md -->
# T001 任務標題

**GitHub Issue：** [#1](https://github.com/keroro1768/KeroroTeam/issues/1)
**狀態：** 🔄 進行中
**負責人：** @Giroro
```

### Phase 5：現有 Enhancement 遷移（0.5 天）

將 `tasks/enhancement/README.md` 中的 Enhancement 轉為 GitHub Issues。

---

## 6. 責任分工

| 工作項目 | 負責人 | 截止 |
|---------|--------|------|
| Phase 1：GitHub Projects 設定 | Kururu | 2026-04-05 |
| Phase 2：Issue Template 建立 | Kururu | 2026-04-05 |
| Phase 3：GitHub Actions Workflows | Kururu | 2026-04-06 |
| Phase 4：本地資料夾對應 | Keroro | 2026-04-06 |
| Phase 5：現有 Enhancement 遷移 | Dororo | 2026-04-07 |
| 流程文件更新（PR-001, PR-003）| Keroro | 2026-04-07 |
| 全員 GitHub 操作培訓 | Keroro | 2026-04-08 |

---

## 7. 推薦的輕量級自動化組合

**最小可行方案（1-2 天可上線）：**

1. ✅ **GitHub Projects** — Kanban 視覺化
2. ✅ **Issue Templates** — 統一提案格式
3. ✅ **Labels + Milestones** — 分類 + Sprint 管理
4. ⚠️ **Actions（Schedule）** — 週期性提醒（可選）
5. ✅ **本地 `tasks/` 維持** — 詳細文件沉積

**完整方案（1 週）：**

- 上述全部
- ✅ Actions 自動化提醒
- ✅ 自動化 Sprint 報告
- ✅ Slack/Discord 通知整合

---

## 8. 風險與緩解

| 風險 | 影響 | 緩解措施 |
|------|------|---------|
| 團隊不熟悉 GitHub | 採用率低 | 1 小時培訓（Keroro 負責）|
| 雙軌造成同步負擔 | 資訊不一致 | 嚴格命名慣例（Issue# 必填）|
| 無網路時無法存取 Project | 中斷工作 | `tasks/` 離線備份仍保留 |
| GitHub 免費版限制（3 個 Project）| 不夠用 | 付費或只用 1 個 Project 多視圖 |
| Actions 使用超時 | 自動化失敗 | 优化 workflow，减少 API 调用 |

---

## 9. 結論與下一步行動

### 推薦方案

**採用 Hybrid 雙軌方案**，理由：
1. 不丟失現有知識庫（本地 `knowledge/`、`tasks/`）
2. 獲得 GitHub 原生自動化（Project 視覺化、Issue 追蹤）
3. 學習成本低，1-2 天可上線最小可行版本
4. GitHub Actions 可處理 80% 的自動化需求

### 立即行動（明天）

- [ ] Kururu 建立 GitHub Project（Board 格式）
- [ ] Kururu 建立 Issue Labels
- [ ] Kururu 建立 Issue Templates
- [ ] Keroro 確認 Labels 命名
- [ ] Keroro 建立第一個 Sprint Milestone

### 一週內完成

- [ ] 所有成員了解 GitHub Issues 操作
- [ ] GitHub Actions 提醒 Workflow 上線
- [ ] 現有 Enhancement 遷移至 GitHub Issues
- [ ] 更新 `PR-001_task-flow.md` 納入 GitHub 流程

---

*文件結束*
