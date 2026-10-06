# WazeHUD cho màn hình CYD 2.8 inch - Bản 1 cổng USB (`CYD2.8-V1USB`)

> [!NOTE]
> **Lời cảm ơn & Tôn trọng bản quyền tác giả (Credits & Respect):**
> Dự án này là phiên bản phát triển & tối ưu hóa mở rộng dựa trên mã nguồn gốc [WazeHUD của tác giả ShindouAris](https://github.com/ShindouAris/WazeHUD) và cộng đồng [WazeMod Vietnam](https://wazemod.io.vn). Xin trân trọng ghi nhận và cảm ơn công sức to lớn của tác giả gốc!

---

## 📌 Ghi chú thiết bị phần cứng (Hardware Note)

- **Nhánh này dành riêng cho mạch:** **ESP32 Cheap Yellow Display (CYD) 2.8 inch loại 1 cổng Micro USB**.
- **Đặc điểm phần cứng:**
  - Chip màn hình ILI9341 cấu hình đảo màu `INVON` (`esp_lcd_panel_invert_color = true`).
  - Kết nối không dây Bluetooth Low Energy (BLE Server).
  - Tên thiết bị hiển thị: `WazeHUD v2.8.1`.
- 👉 **Xem tài liệu chi tiết, danh sách tính năng & changelog tại:** **[Nhánh chính `CYD2.8-V2USB`](https://github.com/quangdo92/waze-hud/tree/CYD2.8-V2USB)**.

---

## 📥 Tải Firmware (Download)

👉 **Truy cập trang phát hành chính thức để tải file:** **[GitHub Releases](https://github.com/quangdo92/waze-hud/releases/latest)**

- **File Factory All-in-One cho mạch này:** **[`waze_hud_cyd_28_1usb_factory.bin`](https://github.com/quangdo92/waze-hud/releases/download/v2.8.8-b8/waze_hud_cyd_28_1usb_factory.bin)** (Nạp tại offset **`0x0`**).

---

## ⚡ Hướng dẫn nạp Firmware

### 🌐 Cách 1: Nạp qua Web Flasher (Khuyên dùng - Cực dễ)
1. Tải file **[`waze_hud_cyd_28_1usb_factory.bin`](https://github.com/quangdo92/waze-hud/releases/download/v2.8.8-b8/waze_hud_cyd_28_1usb_factory.bin)** về máy tính hoặc điện thoại Android.
2. Cắm cáp micro USB kết nối mạch CYD với máy.
3. Mở trình duyệt Chrome / Edge vào: 👉 **[https://wazemod.io.vn/flash-firmware](https://wazemod.io.vn/flash-firmware)**
4. Bấm **Kết nối**, chọn đúng cổng COM của mạch ESP32 CYD.
5. Chọn file `.bin` đã tải, đảm bảo địa chỉ nạp là **`0x0`** và bấm **Flash**.

*(Mẹo: Nếu mạch không tự động nhận chế độ flash, hãy nhấn giữ nút **BOOT**, bấm nhả nút **RESET**, sau đó thả nút **BOOT** rồi kết nối lại).*

### 💻 Cách 2: Nạp qua dòng lệnh esptool
```bash
python -m esptool --chip esp32 -b 460800 write_flash 0x0 waze-hud/dist/waze_hud_cyd_28_1usb_factory.bin
```
