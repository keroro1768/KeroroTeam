# HW-001 USB Protocol — USB 協定基礎

> 最後更新：2026-03-29
> 標籤：#hardware #usb #protocol

---

## USB 2.0 基本架構

| 層 | 說明 |
|----|------|
| 實體層 | D+/D- 差動訊號 |
| 傳輸層 | 封包結構 |
| 協定層 | 端點、傳輸類型 |
| 應用層 | HID、MSC、CDC |

## 端點（Endpoint）

| EP | 類型 | 方向 | 用途 |
|----|------|------|------|
| EP0 | Control | 雙向 | 列舉、設定 |
| EP1 | Interrupt | IN | HID Input |
| EP2 | Interrupt | OUT | HID Output |
| EP3 | Bulk | IN | MSC Data |
| EP4 | Bulk | OUT | MSC Data |

## USB MSC BOT 協定

```
Host → Device
  CBW (31 bytes)     ← Command
  Data Phase (可選)   ← 傳輸資料
Device → Host
  CSW (13 bytes)     ← Status
```

## USB HID 協定

| Report 類型 | 方向 | 說明 |
|------------|------|------|
| Input | Device→Host | 按鍵、滑鼠移動 |
| Output | Host→Device | LED 控制 |
| Feature | 雙向 | 組態資料 |

---

## 詳見

- [PR-003 USB Basics](../common/PR-003_usb-basics.md)
