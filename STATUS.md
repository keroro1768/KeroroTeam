# KeroroTeam 專案狀態總覽

> 更新時間：2026-03-30 21:47
> Heartbeat 自動更新

---

## 📊 專案總覽

| 專案 | 路徑 | 總任務 | 完成 | 進行中 | 待處理 |
|------|------|--------|------|--------|--------|
| **M487_ScsiTool** | `D:\AiWorkSpace\M487_ScsiTool\` | 35+ | 25+ | 1 | 9+ |
| **FWUPD** | `D:\AiWorkSpace\FWUPD\` | 2 | 2 | 0 | 1 |

---

## 🔥 M487_ScsiTool — 重大更新（下午 15:00-15:45）

### 🎉 ICE 連線完全突破

**OpenOCD + Nu-Link (第一代) + M487 = ✅ 完全連線**

### OpenOCD 整合版建立

```
tool/OpenOCD/
├── openocd.bat      (wrapper，含 MSYS2 DLL PATH)
├── bin/openocd.exe  (19MB, 0.12.0+dev)
├── bin/*.dll         (7 個 MSYS2 DLL)
├── scripts/
└── cfg/nulink_m487_ice.cfg
```
**驗證成功：** GDB server enabled on port 3333 ✅

### Sample Code Reference 整理完成

`doc/Samples/` — 統一入口
- HowTo/（8 篇操作指南，已進 Git）
- Reference/tools/（TCL 燒錄腳本，已進 Git）
- Projects/Library/USB_HS_Samples/ — 本機磁碟（Windows 路徑限制）

### 文件修正（根據 ICE 突破）

已修正所有 OpenOCD 相關文件：
- `doc/NuLink/NuLink_Experience_Compilation.md`
- `doc/IDE_Setup/VSCode_OpenOCD_GDB_M487_IDE_Setup.md`
- `doc/hardware/M487/02_TOOLS.md`
- `doc/hardware/M487/03_FLASH_READ_WRITE.md`
- `doc/hardware/M487/05_OPENOCD_REF.md`
- `doc/hardware/M487/06_HSUSBD_COMPILE_FLASH.md`

### 任務狀態

| 任務 | 狀態 | 變更 |
|------|------|------|
| T001 | 🔄 ICE 可用 | +ICE Debug |
| T003 | ✅ 更新 | OpenOCD 路徑更正 |
| T005 | ✅ 更新 | VSCode F5 驗證 |
| T010 | 🔄 可執行 | ICE Debug 可用 |

### Debug 整備矩陣

| Lv | 工具 | 狀態 |
|----|------|------|
| L1 | UART Log | ✅ |
| L2 | MSC Debug Channel + CLI | ✅ |
| **L3** | **ICE + GDB** | **✅ 🔥 新增** |
| L3 | USB Filter Driver | ⏳ Pending（需 WDK）|
| L4 | ITM/SWO + Flash Error Log | ✅ |
| L5 | USBPcap + Wireshark | ⏳ Reference |

---

## 📦 FWUPD

### 任務狀態

| 任務 | 狀態 | 說明 |
|------|------|------|
| F001 研究 | ✅ Finish | Kururu + Dororo 驗證 |
| F002 Plugin | ✅ Finish | Giroro + Dororo 驗證通過 |
| F003 LVFS 帳號 | ⏳ Pending | 待 Keroro 指派 |

---

## ⏳ Enhancement Backlog

| ID | 專案 | 說明 | 提出日期 |
|----|------|------|---------|
| E001 | M487_ScsiTool | ICE 連線突破 — OpenOCD Debug 可用 | 2026-03-30 |

---

*下次 Heartbeat：22:17*

---

## 📌 待確認事項（15:45 來自 Caro）

**Caro 詢問：** `firmware/` 中還有什麼資料夾？

當時已回報：
```
firmware/
├── composite/           ← 主程式（M487 USB 複合裝置）
├── composite_original/   ← 原始版本（待確認用途）
└── hid-over-i2c/       ← 獨立的 HID-over-I2C 方案
```

等待 Caro 進一步指示如何處理 `composite_original/` 和 `hid-over-i2c/`。