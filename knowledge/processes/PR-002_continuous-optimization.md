# PR-002 持續優化機制 — Continuous Optimization

> **研究任務：** 如何讓 KeroroTeam 自動持續進行任務優化
> **Kururu 研究成果**
> **建立日期：** 2026-03-29
> **標籤：** #processes #optimization #automation #team #okr

---

## 📌 摘要

本文件定義 KeroroTeam 如何從「完成任務」進化為「持續優化」的機制。核心概念：**維運保持現狀，強化提升水位**。透過自動化目標追蹤、角色責任明確化、以及 CI/CD 輔助，讓每位成員在日常任務之餘自動投入專案強化。

---

## 1. 自動優化機制

### 1.1 核心框架：OKR + KPI 混合模式

| 層級 | 框架 | 週期 | 目的 |
|------|------|------|------|
| **團隊目標** | OKR | 每季 | 設定方向性「強化目標」 |
| **個人指標** | KPI | 每週 | 量化「可執行的優化任務」 |
| **專案追蹤** | 自動化指標 | 每日/每次 PR | 客觀測量「進步幅度」 |

**範例 OKR（季度）：**

```
Objective：每一個專案都變得比上個月更強
├── KR1：錯誤率（Error Rate）較上月降低 10%
├── KR2：測試覆蓋率提升 5%
├── KR3：自動化程度（CI coverage）從 60% → 80%
└── KR4：平均處理時間縮短 15%
```

**範例 KPI（每週）：**

```
Kururu  每週須產出：
  - 1 份優化分析報告
  - 提出 2 項可實作改進點
  - 追蹤上週建議的執行率 ≥ 50%

Dororo  每週須完成：
  - 驗證上週所有 PR 的變更
  - 產出 1 份「已驗證功能」的回饋報告
  - 主動發現 1 個可測試的優化點
```

### 1.2 讓成員自動承擔優化任務的機制

**核心原則：「優化」不是「額外工作」，而是「工作的一部分」**

| 機制 | 說明 |
|------|------|
| **10% 優化時間配額** | 每週 10% 工作時間（約 4 小時）固定用於優化任務，不可被常務覆蓋 |
| **自動生成待辦** | GitHub Actions 每週自動建立 `optimization-tasks` issue 清單 |
| **串聯責任** | 誰實作、誰負責後續觀測（3 個 commit 內的 regression 由該成員處理）|
| **OKR 綁定** | 季 OKR 達成率影響個人貢獻評分 |
| **「沉沒成本」警示** | 當某項技術債存在 > 30 天，自動提高優先度（防止惡化）|

### 1.3 現有框架對照

| 框架 | 適用場景 | 在 KeroroTeam 的應用方式 |
|------|----------|--------------------------|
| **OKR** | 設定團隊級強化方向 | 每季由 Commander（Keroro）制定 |
| **KPI** | 追蹤個人日常優化產出 | 每週由各成員自評 + 系統佐證 |
| **RACI** | 區分「誰做什麼」 | 用於區分維運 vs 強化職責（見 2.2）|
| **PDCA** | 單次優化循環 | 每個優化提案的標準執行模型 |
| **Kaizen** | 持續小改 | 每日站立會議中的「昨天改進了什麼」環節 |

---

## 2. 專案持續強化 vs 維運

### 2.1 如何定義「強化」而非「完成」

> **核心心態：「完成」是句號，「強化」是省略號。**

| 維運（Maintenance） | 強化（Enhancement） |
|---------------------|---------------------|
| 維持現狀 | 提升水位 |
| Bug Fix、修補漏洞 | 效能提升、架構改善 |
| 被動回應問題 | 主動發現問題 |
| 「讓它不要壞」 | 「讓它變得更好」 |
| 指標：可用性（Uptime）| 指標：效能（Performance）、品質（Quality）|

**「強化」的量化標準：**

```
一項變更被視為「強化」而非「維運」，需滿足以下至少一項：
✓ 效能指標提升 ≥ 5%（速度、記憶體、吞吐量）
✓ 錯誤率下降 ≥ 10%
✓ 測試覆蓋率提升 ≥ 3%
✓ 程式碼可讀性/維護性明顯改善（經 Dororo 確認）
✓ 使用者/客戶體驗改善（可量化）
```

### 2.2 維運 vs 強化的 RACI 矩陣

| 任務類型 | Responsible | Accountable | Consulted | Informed |
|----------|-------------|-------------|-----------|----------|
| Bug Fix（緊急）| Giroro / Tamama | Keroro | Kururu（分析）| Dororo |
| Bug Fix（一般）| Giroro / Tamama | Giroro | Dororo | Keroro |
| 功能新增 | Giroro / Tamama | Keroro | Kururu、Dororo | 全員 |
| **效能優化** | **Kururu** | **Kururu** | Giroro、Dororo | Keroro |
| **架構重構** | **Kururu + Giroro** | **Keroro** | Dororo | 全員 |
| **技術債清理** | **輪值（每人一週）** | **Keroro** | - | 全員 |
| 測試驗證 | Dororo | Dororo | Kururu | Keroro |
| 自動化建置 | Kururu | Kururu | Giroro | 全員 |

### 2.3 如何讓強化任務不被新任務淹沒

**「任務分類閘門」機制：**

```
┌─────────────────────────────────────┐
│       所有新任務 / PR 都需通過       │
│         「分類閘門」檢查              │
└──────────┬──────────────────────────┘
           ↓
    ┌──────────────┐
    │  是緊急 Bug？  │ → YES → 立即處理（維運）
    └───────┬──────┘
            ↓ NO
    ┌──────────────┐
    │  是新功能需求？ │ → YES → 加入 Product Backlog
    └───────┬──────┘
            ↓ NO
    ┌──────────────┐
    │  是強化/優化？  │ → YES → 加入 Enhancement Backlog（優先級固定 20%）
    └───────┬──────┘
            ↓ NO
         技術債 / 文件 等
```

**關鍵保護機制：**

1. ** Enhancement Backlog 最低保留 20% 容量**：每 sprint 結束時，強化任務不得完全被新功能清空
2. **「強化債」預算**：允許累積未執行的強化任務，但需在每季檢視並清理
3. **Commander 否決權**：Keroro 可否決新的非緊急任務，確保強化任務不被插隊
4. **自動化分類助手**：GitHub Actions 自動為每個 issue/PR 打上 `maintenance` / `enhancement` / `feature` 標籤

---

## 3. 團隊協作模式

### 3.1 角色職責深化

| 成員 | 日常任務 | 持續優化責任 | 防止「做完沒事做」的機制 |
|------|----------|-------------|------------------------|
| **Keroro** | Commander，統籌協調 | 監控團隊 OKR 達成率、每週「強化優先度」裁決 | 持續設定新目標，永遠有戰略方向 |
| **Giroro** | Firmware 複雜任務 | 主動發現效能瓶頸、提出重構方案 | 定期「深度 Code Review」，找出可優化點 |
| **Tamama** | Windows Tool 簡單任務 | 維護工具鏈、自動化腳本改善 | 持續改善開發者體驗（DX）|
| **Dororo** | 驗證測試 | 每週產出「驗證覆蓋率報告」、主動發現測試缺口 | 強化測試策略、探索性測試、研究新測試方法 |
| **Kururu** | 分析研究 | 每週「技術研究報告」、追蹤新技術趨勢 | 建立「技術債清單」並持續更新 |

### 3.2 Dororo 持續驗證的驅動機制

```
自動化觸發：
  每當 Giroro/Tamama 提交 PR
        ↓
  GitHub Actions 自動通知 Dororo
        ↓
  Dororo 執行驗證 → 產出 VERIFY.md
        ↓
  若通過 → 合併 + 更新「驗證里程碑」
  若失敗 → 回報問題 + 自動建立 Bug Issue

Dororo 的「強化」職責：
  - 每週分析「驗證失敗原因」TOP3
  - 找出系統性測試缺口
  - 提出測試覆蓋率改善建議
```

### 3.3 Kururu 持續分析的驅動機制

```
自動化觸發：
  每月自動執行「專案體檢」
  （CI 指標蒐集 + 錯誤率追蹤）
        ↓
  Kururu 產出「月度的技術研究報告」
  包含：
    - 錯誤率趨勢分析
    - 效能瓶頸定位
    - 可優化項目 TOP5
    - 新技術/工具建議
        ↓
  轉化為可執行任務 → 進入 Enhancement Backlog
```

### 3.4 避免「做完了沒事做」的狀況

**核心策略：「產出」由系統驅動，而非靠自發性**

| 策略 | 實作方式 |
|------|----------|
| **系統性問題庫** | GitHub Issues 永遠有「待分析」清單（由 Actions 自動補充）|
| **「空白時間」自動填補** | 若成員閒置 > 3 天，系統自動推薦 Enhancement Backlog 中的任務 |
| **技術債輪值** | 每週輪值一人負責「清理一件小事」|
| **研究無上限** | Kururu 任何時間都可以研究新技術，研究成果存入 `knowledge/` |
| **Dororo 的探索時間** | 每週 2 小時「自由測試」時間，專門測試邊界條件、破壞性測試 |
| **OKR 連動** | 季 OKR 與個人綁定，達成率影響評估 |

---

## 4. 工具支援

### 4.1 GitHub Actions 可自動化的事項

| 自動化類型 | 具體實作 |
|-----------|----------|
| **指標追蹤** | 每次 PR 自動計算：錯誤率、測試覆蓋率、構建時間 |
| **任務生成** | 每週一自動建立 `optimization-tasks` issue |
| **分類標籤** | 自動為 Issue/PR 打上 `maintenance` / `enhancement` / `feature` |
| **提醒通知** | 當 Enhancement Backlog < 5 項時自動提醒 Kururu |
| **OKR 進度儀表板** | 每週自動更新 OKR 進度並發布 comment |
| **沉沒成本警示** | 當技術債 issue 存在 > 30 天，自動提高優先度並通知 Keroro |
| **程式碼品質掃描** | 每次 PR 自動執行 linter、static analysis |
| **自動化測試** | 每次 PR 執行完整測試 suite |
| **安全掃描** | 自動化 dependency 漏洞掃描 |

**範例 GitHub Actions Workflow（`optimization-tracker.yml`）：**

```yaml
name: Weekly Optimization Tracker

on:
  schedule:
    - cron: '0 9 * * 1'  # 每周一 09:00 UTC

jobs:
  generate-tasks:
    runs-on: ubuntu-latest
    steps:
      - name: Generate optimization tasks
        run: |
          # 讀取上週指標
          # 自動產生 TOP5 優化建議
          # 建立 GitHub Issue
      - name: Notify team
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              body: '本週優化任務已生成，請至 [Enhancement Backlog](url) 查看'
            })
```

### 4.2 優化進度追蹤工具建議

| 工具 | 用途 | 建議程度 |
|------|------|---------|
| **GitHub Projects** | 任務看板、Backlog 管理 | ⭐⭐⭐ 必用 |
| **GitHub Actions** | 自動化指標收集、任務生成 | ⭐⭐⭐ 必用 |
| **GitHub Insights** | 團隊效能儀表板 | ⭐⭐ 可用 |
| **Codecov / Coveralls** | 測試覆蓋率追蹤 | ⭐⭐ 建議 |
| **Dependabot** | 自動化 dependency 更新 + 安全掃描 | ⭐⭐⭐ 必用 |
| **SonarCloud** | 程式碼品質掃描 | ⭐⭐ 建議 |
| **Prometheus + Grafana** | 效能指標視覺化 | ⭐ 加強時使用 |
| **Notion / Obsidian** | 團隊知識庫（存放分析報告）| ⭐⭐ 建議 |

### 4.3 CI/CD 能做到什麼程度

```
CI/CD 能力光譜：

Level 1（基本）✓ 已達成或易達成：
  - 自動化建置（Build）
  - 自動化測試（Unit Test）
  - 自動化部署（至 staging）

Level 2（推薦）○ 值得投入：
  - 自動化部署（至 production，with approval gate）
  - 測試覆蓋率門檻（coverage < 80% block merge）
  - 自動化效能基準測試（每次 PR）
  - 自動化文件生成

Level 3（加強）△ 可選：
  - 自動化安全掃描（SAST/DAST in CI）
  - A/B 部署策略
  - 自動化的「技術債」檢測
  - 持續監控（Error Rate, Latency in production）
```

**CI/CD 保護強化任務的策略：**

```
當新功能 PR 試圖合併時：
  ↓
CI 檢查清單：
  □ 測試覆蓋率未下降
  □ 錯誤率未增加
  □ 建置時間未惡化
  □ 沒有新增高優先級技術債
  ↓
任一失敗 → PR Blocked + 自動建立 Enhancement Issue
```

---

## 5. 具體執行流程

### 5.1 每週循環（Weekly Loop）

```
星期一：
  ┌─ GitHub Actions 自動產生本週「優化任務清單」
  ├─ Keroro 審視並確認優先順序
  └─ 各成員領取優化任務

星期二～星期四：
  ┌─ 各成員執行日常任務 + 優化任務（9:1 時間配比）
  ├─ Kururu 持續監控指標、產出分析
  └─ Dororo 持續驗證新 PR + 探索性測試

星期五：
  ┌─ 週會：每人報告「本週完成」+「下週強化計畫」
  ├─ 更新 OKR 進度
  └─ 確認 Enhancement Backlog 有足够的「下週任務」
```

### 5.2 每季循環（Quarterly Loop）

```
季度初：
  ┌─ Keroro 制定新季度 OKR
  ├─ 各成員認領季目標中的 KR
  └─ 技術債總清理（移除長期累積的待優化項目）

季度中：
  ┌─ 每月「技術研究日」（Kururu 主導）
  └─ 每月 OKR Check-in（落後的 KR 優先處理）

季度末：
  ┌─ 季 OKR Review
  ├─ 各成員產出「季度貢獻報告」
  └─ 制定下季優化方向
```

### 5.3 單次優化提案流程（PDCA）

```
Plan（Kururu）：
  辨識問題 → 分析根因 → 提出解決方案 → 預估效益
        ↓
Do（Giroro / Tamama）：
  實作解決方案
        ↓
Check（Dororo）：
  驗證改善效果（效能測試、錯誤率追蹤）
        ↓
Act：
  成功 → 文件化 → 更新到 knowledge/
  失敗 → 回歸分析 → 調整方案
```

---

## 6. 責任分工總覽

| 成員 | 主要任務 | 優化職責 | 對外介面 |
|------|----------|----------|----------|
| **Keroro** | 統籌協調、OKR 制定 | 每週裁決優先順序、確保強化任務不被淹沒 | Caro |
| **Giroro** | Firmware 開發 | 主動提出效能優化、重構方案 | 外部供應商 |
| **Tamama** | Windows Tool 開發 | 改善 DX、維護工具鏈 | - |
| **Dororo** | 驗證測試 | 發現測試缺口、提升驗證覆蓋率 | - |
| **Kururu** | 分析研究 | 技術研究、錯誤率分析、Enhancement Backlog 管理 | - |

---

## 7. 立即可執行的行動項目

| 優先順序 | 行動項目 | 負責人 | 預計時程 |
|---------|---------|--------|---------|
| 🔴 高 | 建立 `Enhancement Backlog` GitHub Project Board | Keroro | 本週 |
| 🔴 高 | 設定 GitHub Actions 每週自動生成優化任務 | Kururu | 本週 |
| 🔴 高 | 為所有 Repo 啟用 Dependabot | Giroro | 本週 |
| 🟡 中 | 建立錯誤率追蹤（Error Rate Dashboard）| Kururu | 2 週 |
| 🟡 中 | 設定測試覆蓋率門檻（block merge if coverage ↓）| Giroro + Dororo | 2 週 |
| 🟢 低 | 引入 SonarCloud 程式碼品質掃描 | Kururu | 1 個月 |
| 🟢 低 | 設定 Prometheus + Grafana 效能監控 | Giroro | 1 個月 |

---

## 8. 附錄：參考框架

### OKR 範本（季 OKR）

```
Objective：讓 KeroroTeam 的專案品質持續提升

KR1：错误率每月降低 10%
KR2：測試覆蓋率從 X% 提升至 Y%
KR3：CI/CD 自動化程度達到 80%
KR4：每季完成 3 項架構優化
```

### 技術債追蹤範本

```markdown
## 技術債追蹤

| ID | 描述 | 嚴重程度 | 存在天數 | 負責人 | 狀態 |
|----|------|---------|---------|--------|------|
| TD-001 | [描述] | 高/中/低 | 45 | Giroro | 待處理 |
```

### Enhancement Backlog 範本

```markdown
## Enhancement Backlog

- [ ] E-001：改善錯誤處理邏輯（預期效益：錯誤率↓10%）
- [ ] E-002：重構 USB 驅動以提升穩定性（預期效益：連線失敗率↓50%）
- [ ] E-003：建立自動化效能基準測試（預期效益：每次 PR 自動獲得效能數據）
```

---

## 📎 相關文件

| 文件 | 說明 |
|------|------|
| [PR-001_task-flow.md](PR-001_task-flow.md) | 任務流程基本定義 |
| [PR-002_continuous-optimization.md](PR-002_continuous-optimization.md) | 本文件 |

---

_最後更新：2026-03-29 by Kururu_
