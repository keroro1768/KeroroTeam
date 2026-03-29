# PR-003 USB Basics — USB 基礎知識

> 最後更新：2026-03-29
> 標籤：#common #usb #hardware

---

## USB 基本概念

| 概念 | 說明 |
|------|------|
| HOST | USB 主控端（電腦）|
| DEVICE | USB 裝置（M487）|
| ENDPOINT | 端點，通訊最小單位 |
| INTERFACE | 介面，一個或多個端點 |
| CONFIGURATION | 設定，一個或多個介面 |

## USB 傳輸類型

| 類型 | 速度 | 用途 | 範例 |
|------|------|------|------|
| **Control** | 低 | 列舉、設定 | EP0 |
| **Bulk** | 高 | 大量資料 | USB Storage |
| **Interrupt** | 低延遲 | 事件通知 | Keyboard, HID |
| **Isochronous** | 固定頻寬 | 即時音訊 | 音效卡 |

## USB MSC（H 大容量儲存）

| 元素 | 說明 |
|------|------|
| BOT | Bulk-Only Transfer，MSC 專用協定 |
| CBW | Command Block Wrapper |
| CSW | Command Status Wrapper |

## USB HID（H 人機介面）

| 元素 | 說明 |
|------|------|
| Report Descriptor | 描述 HID 裝置能力 |
| Input Report | 裝置→主機 |
| Output Report | 主機→裝置 |
| Feature Report | 雙向組態 |

## VID / PID

| 欄位 | 說明 |
|------|------|
| VID | Vendor ID，廠商識別碼 |
| PID | Product ID，產品識別碼 |

> ⚠️ VID 需向 USB-IF 申請或購買

---

## 貢獻

修改前請討論，確保跨專案相容。
