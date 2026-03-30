# 全域索引 — Master Index

> 最後更新：2026-03-29
> 自動產生，可用 script 更新

## 📋 索引表

### common/ — 跨專案共享知識

| ID | 文件 | 說明 |
|----|------|------|
| PR-001 | [PR-001_glossary.md](common/PR-001_glossary.md) | 術語表 |
| PR-002 | [PR-002_debug-methods.md](common/PR-002_debug-methods.md) | 通用 Debug 方法 |
| PR-003 | [PR-003_usb-basics.md](common/PR-003_usb-basics.md) | USB 基礎知識 |

### firmware/ — 韌體相關

| ID | 文件 | 說明 |
|----|------|------|
| FW-001 | [FW-001_fwupd-guide.md](firmware/FW-001_fwupd-guide.md) | FWUPD 韌體更新 |
| FW-002 | [FW-002_m487-firmware.md](firmware/FW-002_m487-firmware.md) | M487 韌體開發 |

### hardware/ — 硬體相關

| ID | 文件 | 說明 |
|----|------|------|
| HW-001 | [HW-001_usb-protocol.md](hardware/HW-001_usb-protocol.md) | USB 協定基礎 |
| HW-002 | [HW-002_arm-cortex-m4.md](hardware/HW-002_arm-cortex-m4.md) | ARM Cortex-M4 基礎 |

### software/ — 軟體相關

| ID | 文件 | 說明 |
|----|------|------|
| SW-001 | [SW-001_driver-dev.md](software/SW-001_driver-dev.md) | Windows 驅動程式開發 |

### tools/ — 工具相關

| ID | 文件 | 說明 |
|----|------|------|
| TL-001 | [TL-001_vscode-setup.md](tools/TL-001_vscode-setup.md) | VSCode 開發環境 |
| TL-002 | [TL-002_winget-devtools.md](tools/TL-002_winget-devtools.md) | Winget 安裝 VS/WDK/SDK |

### processes/ — 流程方法論

| ID | 文件 | 說明 |
|----|------|------|
| PR-001 | [PR-001_task-flow.md](processes/PR-001_task-flow.md) | 任務流程 |
| PR-002 | [PR-002_continuous-optimization.md](processes/PR-002_continuous-optimization.md) | 持續優化機制 |

---

## 🔍 快速搜尋

```bash
# 本地搜尋關鍵字
Get-ChildItem -Path . -Filter "*.md" -Recurse | Select-String "USB"
```

## 📝 文件命名規範

```
{ID}_{slug}.md

ID 格式：{領域代碼}-{序號}
領域代碼：HW/SW/FW/TL/PR/CM
```

## 🏷️ Tags

#common #firmware #hardware #software #tools #processes #m487 #fwupd #usb #debug #optimization #automation #team #okr
