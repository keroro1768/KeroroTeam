# FW-001 FWUPD Guide — Linux 韌體更新

> 最後更新：2026-03-29
> 標籤：#firmware #fwupd #linux #update

---

## 什麼是 FWUPD？

**FWUPD（Linux Vendor Firmware Update）** 是 Linux 的開源韌體更新框架。

## 組成架構

```
┌──────────────────┐
│  GNOME Software  │
│    fwupdmgr     │
└────────┬─────────┘
         │ D-Bus
         ▼
┌──────────────────┐
│    fwupd daemon  │
│   (Plugin)       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    LVFS Server    │
│  lvfs.fwupd.org  │
└──────────────────┘
```

## Plugin 類型

| 類型 | 說明 | 難度 |
|------|------|------|
| Quirk-only | 只需設定檔 | ⭐ |
| Simple Plugin | 有 Helper 可用 | ⭐⭐ |
| Complex Plugin | 完全自訂 | ⭐⭐⭐⭐ |

## LVFS 上架流程

1. 申請 LVFS 帳號（免費）
2. 上傳 `.cab` 檔
3. 等待審核
4. 發布

## 常用指令

```bash
# 列出裝置
fwupdmgr get-devices

# 安裝更新
fwupdmgr install firmware.cab

# 查看歷史
fwupdmgr history
```

## M487 評估

| 項目 | 評估 |
|------|------|
| Plugin 類型 | Desktop-style（可行）|
| 難度 | 中等 |
| 所需文件 | Nuvoton USB Download Protocol |

---

## 詳見

- KeroroTeam FWUPD 研究報告
