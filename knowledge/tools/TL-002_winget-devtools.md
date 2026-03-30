# TL-002 Winget 安裝開發工具鏈

**標籤：** `#winget` `#visual-studio` `#wdk` `#sdk` `#driver-development`

---

## 📌 議題

如何使用 winget 安裝驅動程式開發工具鏈（Visual Studio / WDK / SDK）。

---

## 🔧 安裝順序

```
VS2022 → SDK → WDK
```

**關鍵約束：** SDK 與 WDK 版本號（Build Number）必須匹配。

---

## 🔧 Step 1：Visual Studio 2022

```powershell
winget install Microsoft.VisualStudio.2022.Community --silent --accept-package-agreements --accept-source-agreements
```

### ⚠️ 驅動開發必備元件（Individual Components）

在 VS Installer → Modify → Individual Components，搜尋並勾選：

- MSVC v143 - VS 2022 C++ ARM64/ARM64EC Spectre-mitigated libs (Latest)
- MSVC v143 - VS 2022 C++ x64/x86 Spectre-mitigated libs (Latest)
- C++ ATL for latest v143 build tools with Spectre Mitigations (ARM64/ARM64EC)
- C++ ATL for latest v143 build tools with Spectre Mitigations (x86 & x64)
- C++ MFC for latest v143 build tools with Spectre Mitigations (ARM64/ARM64EC)
- C++ MFC for latest v143 build tools with Spectre Mitigations (x86 & x64)
- **Windows Driver Kit** ← WDK VSIX 擴展

快速搜尋關鍵字：`64 latest spectre`（英文系統）

### 版本參考

| 版本 | winget ID | 最新版本 |
|------|-----------|---------|
| Community | `Microsoft.VisualStudio.2022.Community` | 17.14.28 |
| Professional | `Microsoft.VisualStudio.2022.Professional` | 17.14.28 |
| Enterprise | `Microsoft.VisualStudio.2022.Enterprise` | 17.14.29 |

---

## 🔧 Step 2：Windows SDK

```powershell
winget install Microsoft.WindowsSDK.10.0.26100 --silent --accept-package-agreements --accept-source-agreements
```

### 版本矩陣

| winget ID | 版本 | 對應 WDK 版本 |
|-----------|------|-------------|
| `Microsoft.WindowsSDK.10.0.26100` | 10.0.26100.7705 | WDK 10.1.26100 ✅（最新） |
| `Microsoft.WindowsSDK.10.0.22621` | 10.0.22621.2428 | WDK 10.1.22621 |

**注意：** VS2022 安裝時不會自動帶入最新 SDK，必須獨立安裝。

---

## 🔧 Step 3：Windows Driver Kit

```powershell
winget install Microsoft.WindowsWDK.10.0.26100 --silent --accept-package-agreements --accept-source-agreements
```

### ⚠️ WDK VSIX 仍需手動

WDK 本體可 winget 安裝，但 **WDK VSIX 擴展** 仍需透過 VS Installer 安裝：

1. 啟動 **Visual Studio Installer**
2. 選擇 **Modify**
3. 切換到 **Individual Components** 頁籤
4. 搜尋並勾選 **Windows Driver Kit**
5. 點擊 **Modify**

若找不到驅動程式專案範本，表示 WDK VSIX 未正確安裝。

### 版本矩陣

| winget ID | 版本 | 對應 SDK 版本 |
|-----------|------|-------------|
| `Microsoft.WindowsWDK.10.0.26100` | 10.1.26100.6584 | SDK 10.0.26100 ✅（最新） |
| `Microsoft.WindowsWDK.10.0.22621` | 10.1.22621.2428 | SDK 10.0.22621 |

---

## ✅ 驗證方式

```powershell
devenv --version
dir "C:\Program Files (x86)\Windows Kits\10\Include"
```

---

## ⚠️ 重要提醒

1. **WDK 17.11+：WDK VSIX 無法完全自動化**
   - WDK 本體可 winget 安裝
   - WDK VSIX 擴展仍是 VS Individual Component，需手動透過 VS Installer UI 安裝

2. **VS2022 不會自動帶入最新 SDK**
   - SDK 必須獨立安裝
   - VS2026 尚未通過 WDK 驗證，驅動開發請使用 VS2022

---

## 🔌 離線安裝

- WDK 安裝程式：`https://download.microsoft.com/download/.../wdksetup.exe`
- SDK 安裝程式：`https://download.microsoft.com/download/.../winsdksetup.exe`
- **EWDK**：包含 VS Build Tools + SDK + WDK 的 ISO，適用於純命令列環境

---

*建立時間：2026-03-29*
*執行者：Kururu*
