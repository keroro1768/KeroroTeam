# ICE-M487 情報系統 — Nu-Link + OpenOCD 連線問題

> 建立時間：2026-03-30  
> 更新時間：2026-03-30 11:20  
> 適用情境：M487 ICE 連線失敗、LIBUSB_ERROR_ACCESS、驅動問題  
> **狀態：✅ 已完全解決**

---

## 🎉 最終結果（2026-03-30 11:15）

**openocd-build (0.12.0+dev 2026-03-25) + 第一代 Nu-Link + M487 = ✅ 完全連線**

```
Info : Nu-Link firmware_version 7946, product_id (0x40012009)
Info : Adapter is Nu-Link
Info : IDCODE: 0x2BA01477
Info : [M487.cpu] Cortex-M4 r0p1 processor detected
Info : [M487.cpu] target has 6 breakpoints, 4 watchpoints
[M487.cpu] halted due to debug-request, current mode: Thread
xPSR: 0x01000000 pc: 0x100028f0 msp: 0x20020000
```

---

## 🔑 成功關鍵

### 1. 使用正確的 OpenOCD Build

| Build | 版本 | 大小 | NULINK HLA | 可用性 |
|-------|------|------|------------|--------|
| `openocd-build/bin/openocd.exe` | **0.12.0+dev (2026-03-25)** | 19MB | ✅ 完整 | ✅ **本機可用** |
| `OpenOCD-Nuvoton/bin/openocd_cmsis-dap.exe` | 0.12.0 (2025-02-17) | 16MB | ❌ 不完整 | ❌ |
| `Tool/openocd/OpenOCD-20260302-0.12.0/bin/openocd.exe` | 0.12.0 (sysprogs) | 5MB | ❌ 無 NULINK layout | ❌ |

**openocd-build 路徑：** `D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin\openocd.exe`

### 2. MSYS2 MinGW DLL 環境

執行前需設定 PATH：
```
C:\msys64\mingw64\bin;D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin;$env:PATH
```

**需要的 DLL（皆在 `C:\msys64\mingw64\bin`）：**
- `libusb-1.0.dll` ✅
- `hidapi.dll` ✅（需確認存在）
- `libgcc_s_dw2-1.dll` ✅
- `libwinpthread-1.dll` ✅

### 3. 正確的命令序列（OpenOCD 0.12 語法）

```
adapter driver hla
hla layout nulink
hla vid_pid 0x0416 0x511C
transport select swd
swd newdap M487 cpu -expected-id 0x2BA01477
dap create M487.dap -chain-position M487.cpu
target create M487.cpu cortex_m -dap M487.dap
```

**注意：** OpenOCD 0.12 的 Target 建立語法已改為 `-dap` 而非 `-chain-position`。

---

## 📋 完整設定檔

**路徑：** `D:\AiWorkSpace\M487_ScsiTool\tool\openocd\nulink_m487_ice.cfg`

```tcl
# OpenOCD Config for M487 via Nu-Link (First Gen)
# Compatible with openocd-build (0.12.0+dev 2026-03-25)
#
# Requirements:
#   PATH must include: C:\msys64\mingw64\bin;D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin;
#
# Usage:
#   openocd.exe -s D:\AiWorkSpace\M487_ScsiTool\tool\OpenOCD-Nuvoton\OpenOCD\scripts -f nulink_m487_ice.cfg

adapter driver hla
hla layout nulink
hla vid_pid 0x0416 0x511C
transport select swd

# M487 SWD DP-ID = 0x2BA01477
swd newdap M487 cpu -expected-id 0x2BA01477
dap create M487.dap -chain-position M487.cpu
target create M487.cpu cortex_m -dap M487.dap

# Optional: set clock speed (kHz)
adapter speed 4000
```

---

## 🚀 快速啟動指令

### 啟動 GDB Server（3333埠）
```powershell
$env:PATH = 'C:\msys64\mingw64\bin;D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin;' + $env:PATH
cd D:\AiWorkSpace\M487_ScsiTool\tool\OpenOCD-Nuvoton\OpenOCD\bin
.\openocd.exe -s ../scripts -f D:/AiWorkSpace/M487_ScsiTool/tool/openocd/nulink_m487_ice.cfg
```

### 連線測試（Halt）
```powershell
$env:PATH = 'C:\msys64\mingw64\bin;D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin;' + $env:PATH
.\openocd.exe -s ../scripts -f D:/AiWorkSpace/M487_ScsiTool/tool/openocd/nulink_m487_ice.cfg -c "init" -c "reset halt" -c "targets" -c "shutdown"
```

### 燒錄韌體
```powershell
$env:PATH = 'C:\msys64\mingw64\bin;D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin;' + $env:PATH
.\openocd.exe -s ../scripts -f D:/AiWorkSpace/M487_ScsiTool/tool/openocd/nulink_m487_ice.cfg -c "init" -c "reset halt" -c "flash write_image erase D:/AiWorkSpace/M487_ScsiTool/firmware/composite/build/firmware.bin 0" -c "shutdown"
```

### GDB 連線
```bash
arm-none-eabi-gdb D:/AiWorkSpace/M487_ScsiTool/firmware/composite/build/firmware.elf
(gdb) target remote localhost:3333
```

---

## 🔍 第一代 Nu-Link 為何不能用 CMSIS-DAP？

**原因：Interface 1 的 WinUSB 不實作標準 CMSIS-DAP Protocol**

```
USB\VID_0416&PID_511C&MI_01 (WinUSB Interface)
├── 驅動：WINUSB ✅（已正確安裝）
├── 但：收到的 CMD_INFO (0x00) → 回 0x80 (STALL)
└── 結論：Proprietary HID Protocol，非標準 CMSIS-DAP
```

**Proprietary Protocol Commands（来自 nulink_usb.c）：**
| Command | Opcode | 用途 |
|---------|--------|------|
| CMD_CHECK_ID | 0xA3 | 檢查晶片 ID |
| CMD_READ_REG | 0xB5 | 讀取 CPU 暫存器 |
| CMD_WRITE_REG | 0xB8 | 寫入 CPU 暫存器 |
| CMD_READ_RAM | 0xB1 | 讀取 RAM |
| CMD_WRITE_RAM | 0xB9 | 寫入 RAM |
| CMD_MCU_RESET | 0xE2 | 重置 MCU |
| CMD_MCU_STOP_RUN | 0xD2 | 停止 MCU |
| CMD_MCU_FREE_RUN | 0xD3 | 自由運行 |
| CMD_SET_CONFIG | 0xA2 | 設定配置 |

---

## ⚠️ 重要發現

### 為何之前 LIBUSB_ERROR_ACCESS？
- 錯誤的 OpenOCD binary + 錯誤的 driver
- `openocd_cmsis-dap.exe` 的 CMSIS-DAP mode 吃到 STALL（因為 Interface 1 不是 CMSIS-DAP）
- 正確的 binary 是 `openocd-build\bin\openocd.exe`，使用 `hla` driver

### 為何 openocd_cmsis-dap.exe 的 `hla layout nulink` 無效？
- `openocd_cmsis-dap.exe` 預設使用 CMSIS-DAP HID 底層
- `hla` driver 雖然列出，但 `nulink` layout 未正確初始化
- `openocd-build` 的 NULINK support 是完整編譯的

### Nuvoton numicroM4.cfg 為何Hang？
- 舊版 OpenOCD 語法（`-chain-position` 在 target create）
- 新版需要 `-dap` 參數
- **不要使用 Nuvoton 內建的 numicroM4.cfg**，用我們自訂的設定檔

---

## 📁 關鍵檔案

| 用途 | 路徑 |
|------|------|
| ✅ 工作 OpenOCD | `D:\AiWorkSpace\M487_ScsiTool\tool\openocd-build\bin\openocd.exe` |
| ✅ 設定檔 | `D:\AiWorkSpace\M487_ScsiTool\tool\openocd\nulink_m487_ice.cfg` |
| ✅ 韌體 | `D:\AiWorkSpace\M487_ScsiTool\firmware\composite\build\firmware.bin` |
| ✅ ELF (含 DWARF) | `D:\AiWorkSpace\M487_ScsiTool\firmware\composite\build\firmware.elf` |
| ✅ Scripts 目錄 | `D:\AiWorkSpace\M487_ScsiTool\tool\OpenOCD-Nuvoton\OpenOCD\scripts\` |
| MSYS2 DLL | `C:\msys64\mingw64\bin\` |
| VSCode launch.json | `D:\AiWorkSpace\M487_ScsiTool\firmware\composite\.vscode\launch.json` |

---

## 🔧 VSCode Debug 設定更新

VSCode launch.json 需要更新 `serverpath` 和 configFiles 指向新的設定檔：

```json
{
  "serverpath": "D:/AiWorkSpace/M487_ScsiTool/tool/openocd-build/bin/openocd.exe",
  "configFiles": [
    "D:/AiWorkSpace/M487_ScsiTool/tool/openocd/nulink_m487_ice.cfg"
  ]
}
```

**注意：** launch.json 中的 `preLaunchTask` 如果是指向 `Build M487 Firmware`，記得先編譯。
