# USB Filter Driver 與 Bus Reset 技術調查

**作者：** Giroro  
**日期：** 2026-03-31  
**用途：** 軟體觸發 USB Bus Reset 之可行性研究

---

## 1. 摘要

Windows 系統可透過軟體方式觸發 USB Bus Reset，主要途徑有二：

| 層級 | 方法 | 所需條件 |
|------|------|----------|
| **User Mode** | WinUSB API / libusb | 需已安裝對應 Driver，且裝置已枚舉 |
| **Kernel Mode** | Filter Driver + `IOCTL_INTERNAL_USB_RESET_PORT` | 需自行開發 KMDF/WDM Filter Driver |

> **結論：** Filter Driver 可實現軟體觸發 Bus Reset，但前提是 M487 必須先被 Windows 成功枚舉（至少出現在 Device Manager）。

---

## 2. Windows USB Driver 架構分層

```
┌──────────────────────────────────────────────┐
│           User Mode Applications              │
│  (WinUSB API / libusb / HID API)             │
└────────────────────┬─────────────────────────┘
                     │ DeviceIoControl / WinUsb_ResetDevice
┌────────────────────▼─────────────────────────┐
│         Upper Filter Driver (可選)            │
│  - 介入特定 IRP                               │
│  - 可阻擋/修改/轉發 requests                  │
└────────────────────┬─────────────────────────┘
                     │
┌────────────────────▼─────────────────────────┐
│           USB Device Driver                   │
│  (WinUSB / USBCCGP / 自定義 driver)          │
└────────────────────┬─────────────────────────┘
                     │
┌────────────────────▼─────────────────────────┐
│         Lower Filter Driver (可選)           │
│  - 安裝在 Device Stack 下方                   │
│  - 攔截 Hub 與 Device 之間的 IRP             │
└────────────────────┬─────────────────────────┘
                     │ IOCTL_INTERNAL_USB_XXX
┌────────────────────▼─────────────────────────┐
│           USB Hub Driver (USBHUB.sys)         │
│  - 負責 enumeration                           │
│  - 處理 Port Reset / Port Status             │
└────────────────────┬─────────────────────────┘
                     │
┌────────────────────▼─────────────────────────┐
│    USB Host Controller Driver (USBPORT.sys)   │
└────────────────────┬─────────────────────────┘
                     │ USB Hardware Commands
┌────────────────────▼─────────────────────────┐
│              USB Hardware (PHY)               │
└──────────────────────────────────────────────┘
```

---

## 3. User Mode 實現方式

### 3.1 WinUSB API

```c
#include <winusb.h>

WINUSB_INTERFACE_HANDLE hDevice;
// 開啟裝置取得 handle...
WinUsb_ResetDevice(hDevice);
```

**限制：**
- 需為裝置安裝 WinUSB driver（inf）
- 裝置必須已成功枚舉
- 若 M487 未被識別，無法取得 handle

### 3.2 libusb-win32

```c
#include <lusb0_usb.h>

usb_dev_handle *handle = usb_open(device);
usb_reset_device(handle);
```

### 3.3 HID API（僅限 HID 裝置）

```c
HIDD_ATTRIBUTES attrib = { sizeof(HIDD_ATTRIBUTES), 0 };
HidD_GetAttributes(hidDevice, &attrib);
// 透過 IOCTL_HID_RESET_DEVICE 發送 reset
```

---

## 4. Filter Driver 實現方式

### 4.1 什麼是 Filter Driver

Filter Driver 安插在 USB 軟體棧的中間層，可選擇：

| 類型 | 安裝位置 | 用途 |
|------|----------|------|
| **Upper Filter** | Device Stack 上方 | 攔截上層應用程式的 IRP |
| **Lower Filter** | Device Stack 下方 | 攔截送往下層 Hub 的 IRP |

### 4.2 核心 IOCTL

| IOCTL | 說明 |
|-------|------|
| `IOCTL_INTERNAL_USB_RESET_PORT` | 觸發 Bus Reset |
| `IOCTL_INTERNAL_USB_GET_PORT_STATUS` | 取得 Port 狀態 |
| `IOCTL_INTERNAL_USB_DISABLE_PORT` | 停用 Port |

### 4.3 KMDF（WDF）實作範例

**發送 Internal IOCTL 到下層：**

```c
#include <ntddk.h>
#include <wdf.h>

NTSTATUS
FilterResetPort(WDFDEVICE Device)
{
    WDFREQUEST Request;
    WDF_MEMORY_DESCRIPTOR MemDesc;
    NTSTATUS Status;

    // 建立 reset request
    Status = WdfRequestCreate(
        WdfDeviceGetIoTarget(Device),
        WDF_NO_OBJECT_ATTRIBUTES,
        &Request);

    if (!NT_SUCCESS(Status))
        return Status;

    // 發送 IOCTL 到下層驅動
    Status = WdfIoTargetSendInternalIoctlSynchronously(
        WdfDeviceGetIoTarget(Device),
        Request,
        IOCTL_INTERNAL_USB_RESET_PORT,  // <-- 核心
        NULL,   // input buffer
        NULL,   // output buffer
        &MemDesc,
        NULL,   // bytesReturned
        NULL);  // timeout

    WdfObjectDelete(Request);
    return Status;
}
```

### 4.4 WDM 傳統 IRP 方式

```c
PIRP ResetIrp;
IO_STACK_LOCATION *stack;

// 配置 Lower Device
PDEVICE_OBJECT lowerDevice = deviceExtension->LowerDevice;

// 配置 IRP
ResetIrp = IoAllocateIrp(lowerDevice->StackSize, FALSE);
if (!ResetIrp)
    return STATUS_INSUFFICIENT_RESOURCES;

stack = IoGetNextIrpStackLocation(ResetIrp);
stack->MajorFunction = IRP_MJ_INTERNAL_DEVICE_CONTROL;
stack->Parameters.DeviceIoControl.IoControlCode =
    IOCTL_INTERNAL_USB_RESET_PORT;

// 發送到下層
IoCallDriver(lowerDevice, ResetIrp);
```

---

## 5. 安裝與部署

### 5.1 註冊 Upper Filter

在 inf 檔中加入：

```inf
[Manufacturer]
%MfgName%=MyDevice

[MyDevice]
%DeviceName%=MyDevice_Install,USB\VID_04F3&PID_0732

[MyDevice_Install]
CopyFiles=MyDriver.Copy
AddReg=MyDevice_AddReg

[MyDevice_AddReg]
HKR,,"UpperFilters",0x00010000,"MyFilterDriver"
```

### 5.2 風險提醒

| 風險 | 說明 |
|------|------|
| **影響範圍** | Filter 會作用於該 Hub 下的所有 USB 裝置，非單一 VID/PID |
| **簽署問題** | 未簽署的 driver 安裝時 Windows 會彈出警告 |
| **開發環境** | 建議使用測試機而非正式機 |
| **穩定性** | Bug 可能導致系統 USB 功能全失效 |

---

## 6. 針對 M487 的適用性評估

| 情境 | 是否可行 | 說明 |
|------|----------|------|
| M487 **已成功枚舉** | ✅ 可行 | 可用 Filter Driver 攔截並觸發 reset |
| M487 **枚舉失敗** | ❌ 不可行 | Filter Driver 無法綁定不存在的裝置 |

**前置條件：**
1. M487 必須在 Windows 中出現（即使 Device Manager 顯示驚嘆號）
2. 需為該 VID/PID 安裝一個 Base Driver（WinUSB/self-made）
3. 再於其上安裝 Filter Driver

**替代方案（枚舉失敗時）：**
- 設計看門狗：當 USB disconnect 時自動重試枚舉
- 在 M487 韌體層加入策略：開機延遲、枚舉失敗重試
- 依賴 power-cycle 硬體方式（目前最穩定）

---

## 7. 建議後續方向

1. **短期：** 繼續使用 power-cycle 硬體方式確認 M487 基本功能
2. **中期：** 開發 M487 的 WinUSB Base Driver → 再加 Filter Driver 實現軟體 reset
3. **長期：** 在 M487 韌體中加入 USB 狀態機，自動從枚舉失敗恢復

---

## 8. 參考資料

- Microsoft Docs: `IOCTL_INTERNAL_USB_RESET_PORT`
- WDK: `WdfUsbTargetDeviceResetPortSynchronously`
- OSR Online: "Writing Windows USB Filters"
- Walter Oney's "Programming the Microsoft Windows Driver Model"
