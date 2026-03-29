# HW-002 ARM Cortex-M4 — ARM Cortex-M4 基礎

> 最後更新：2026-03-29
> 標籤：#hardware #arm #cortex-m4

---

## 核心暫存器

| 暫存器 | 說明 |
|--------|------|
| R0-R12 | 通用暫存器 |
| R13 (SP) | Stack Pointer |
| R14 (LR) | Link Register |
| R15 (PC) | Program Counter |
| xPSR | Program Status |

## Debug 元件

| 元件 | 位址 | 用途 |
|------|------|------|
| ITM | 0xE0000000 | Instrumentation Trace |
| DWT | 0xE0001000 | Debug Watchpoint |
| NVIC | 0xE000E100 | Nested Vector Interrupt |

## DWT Breakpoint/Watchpoint

| 功能 | 數量 |
|------|------|
| Hardware Breakpoint | 6 |
| Watchpoint | 4 |

## 記憶體對應

| 區域 | 位址 | 大小 |
|------|------|------|
| Flash | 0x00000000 | 512 KB |
| SRAM | 0x20000000 | 160 KB |
| Peripherals | 0x40000000 | - |

## 常用除錯指令

| 指令 | 說明 |
|------|------|
| `monitor reset halt` | 重置並暫停 |
| `load` | 載入韌體 |
| `break main` | 在 main 設斷點 |

---

## 詳見

- M487_ScsiTool Debug 研究文件
