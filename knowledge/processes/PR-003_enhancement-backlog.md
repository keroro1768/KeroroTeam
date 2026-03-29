# PR-003 Enhancement Backlog — 強化待辦清單

> 最後更新：2026-03-29
> 標籤：#processes #enhancement #backlog

---

## 什麼是 Enhancement Backlog？

**一個永遠不為空的工作清單。**

任何時候想到「這裡可以更好」，就加進來。

---

## Enhancement vs Bug Fix

| 類型 | 說明 | 優先 |
|------|------|------|
| **Bug Fix** | 讓它能用 | P0 |
| **Enhancement** | 讓它更好 | P1-P3 |

---

## 格式

```markdown
## [HW-001] USB 協定文件可以更詳細

**當前狀態：** 提出
**預估效益：** 讓新成員更快上手
**負責人：** 待認領
**標籤：** #enhancement #hardware
```

---

## GitHub Project Board 設定

### Column（欄位）

| 欄位 | 說明 |
|------|------|
| **Backlog** | 所有增強構想 |
| **To Do** | 確認要做的 |
| **In Progress** | 正在做的 |
| **Done** | 完成的 |

---

## Enhancement 範例

### 電腦效能

| 項目 | 說明 |
|------|------|
| 重構模組 | 某個函式太肥，可以拆小 |
| 增加注釋 | 關鍵演算法加上說明 |
| 單元測試 | 核心邏輯補測試 |
| 文件更新 | 讓新成員更快上手 |

### 流程優化

| 項目 | 說明 |
|------|------|
| CI/CD | 自動化測試流程 |
| 文件結構 | 讓大家更快找到資料 |
| 工具改進 | 升級開發工具版本 |

---

## 機制

### 提案流程

```
任何時候想到
    ↓
提出 Enhancement（格式如上）
    ↓
Dororo 驗收價值
    ↓
放入 Enhancement Backlog
    ↓
每週 Review，安排負責人
```

### 防止被淹沒

- **固定配額**：每週至少完成 1 個 Enhancement
- **不可高於 P0**：Bug Fix 完成後，Enhancement 升為 P0

---

## 維護原則

1. **任何時候都可以提案**
2. **每週至少 Review 一次**
3. **完成的 Enhancement 要記錄學到的東西**
4. **一個月內沒人認領，提高優先度**
