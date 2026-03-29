# SW-001 Driver Development — Windows 驅動程式開發

> 最後更新：2026-03-29
> 標籤：#software #driver #windows #kmdf #hid

---

## Windows Driver 架構

| 類型 | 說明 |
|------|------|
| WDM | Windows Driver Model（老）|
| KMDF | Kernel-Mode Driver Framework（新）|
| UMDF | User-Mode Driver Framework |

## KMDF HID Filter

```
應用程式
    ↓ IOCTL
HID Class Driver
    ↓ IRP
M487Filter (Upper Filter) ← 我們的
    ↓
底層 USB HID 裝置
```

## 重要 IOCTL

| IOCTL | 說明 |
|-------|------|
| IOCTL_HID_GET_FEATURE | 取得 Feature Report |
| IOCTL_HID_SET_FEATURE | 設定 Feature Report |
| IOCTL_HID_READ_REPORT | 讀取 Report |
| IOCTL_HID_WRITE_REPORT | 寫入 Report |

## 安裝驅動

```bat
# 安裝
pnputil /add-driver M487Filter.inf

# 解除安裝
pnputil /remove-device
```

## 簽章

| 方式 | 說明 |
|------|------|
| Test Signing | 開發用 |
| EV Code Signing | 正式發布 |

---

## 詳見

- M487_ScsiTool M487Filter Driver
