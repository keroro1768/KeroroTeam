# FW-002 M487 Firmware — M487 韌體開發

> 最後更新：2026-03-29
> 標籤：#firmware #m487 #nuvoton #c

---

## M487 晶片

| 規格 | 數值 |
|------|------|
| 核心 | ARM Cortex-M4F |
| 時脈 | 192 MHz |
| SRAM | 160 KB |
| Flash | 512 KB |
| USB | USB 2.0 High-Speed OTG |

## 專案結構

```
M487_ScsiTool/
├── firmware/
│   └── composite/           ← 主要韌體
│       ├── main.c
│       ├── hid_i2c.c
│       ├── msc_debug.c
│       └── itm.c
├── tool/
│   ├── msc_debug/         ← Host Tool
│   └── M487Filter/         ← Windows Driver
└── doc/
```

## 除錯系統（L1-L4）

| 等級 | 工具 | 狀態 |
|------|------|------|
| L1 | UART Log | ✅ |
| L2 | MSC Debug Channel | ✅ |
| L3 | ITM/SWO Trace | ✅ |
| L4 | GDB + OpenOCD | 🔄 |

## 燒錄方式

| 方式 | 工具 |
|------|------|
| OpenOCD + Nu-Link | 🔄 驅動問題 |
| Keil MDK | 需 License |
| NuStudio | 官方工具 |

## USB VID/PID

| 欄位 | 數值 |
|------|------|
| VID | 0x04F3 |
| PID | 0x0732 |

## Build

```bash
cd firmware/composite
make -f Makefile
```

---

## 詳見

- M487_ScsiTool repo
- M487 Correct Usage Guide
