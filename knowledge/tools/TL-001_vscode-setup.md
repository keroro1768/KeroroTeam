# TL-001 VSCode Setup — VSCode 開發環境

> 最後更新：2026-03-29
> 標籤：#tools #vscode #gcc #gdb #openocd

---

## 必需 Extensions

| Extension | ID |
|-----------|-----|
| C/C++ | `ms-vscode.cpptools` |
| Cortex-Debug | `marus25.cortex-debug` |
| ARM Assembly | `dland90.arm-assembly` |

## 設定檔案

| 檔案 | 用途 |
|------|------|
| `.vscode/c_cpp_properties.json` | C/C++ 路徑 |
| `.vscode/launch.json` | Debug 設定 |
| `.vscode/tasks.json` | 編譯任務 |

## 快捷鍵

| 按鍵 | 功能 |
|------|------|
| `Ctrl+Shift+B` | 編譯 |
| `F5` | 開始 Debug |
| `F9` | 設定斷點 |
| `F10` | 單步執行 |

## OpenOCD + GDB Debug

```json
{
  "servertype": "openocd",
  "serverpath": "openocd.exe 路徑",
  "configFiles": ["openocd 設定檔"]
}
```

## 常見問題

| 問題 | 解法 |
|------|------|
| LIBUSB_ERROR_ACCESS | Zadig 安裝 WinUSB |
| 停在 HardFault | 正常，按 F5 繼續 |

---

## 詳見

- M487_ScsiTool IDE Setup 文件
