# WazeHUD cho màn hình CYD 2.8 inch - Bản kết nối USB Serial (`2.8-in-CYD-USB`)

> [!NOTE]
> **Lời cảm ơn & Tôn trọng bản quyền tác giả (Credits & Respect):**
> Dự án này là phiên bản phát triển & tối ưu hóa mở rộng dựa trên mã nguồn gốc [WazeHUD của tác giả ShindouAris](https://github.com/ShindouAris/WazeHUD) và cộng đồng [WazeMod Vietnam](https://wazemod.io.vn). Xin trân trọng ghi nhận và cảm ơn công sức to lớn của tác giả gốc!

---

## 📌 Ghi chú thiết bị phần cứng (Hardware Note)

- **Nhánh này dành cho:** **Kết nối có dây qua cổng USB Serial CH340 (115200 baud, 8N1)**.
- **Đặc điểm nổi bật:**
  - **Loại bỏ hoàn toàn Bluetooth Low Energy (BLE)**, giải phóng hơn **80 KB RAM**.
  - Điện thoại Android cắm cáp USB OTG vào cổng dữ liệu của mạch CYD: Nhận ngay lập tức, cực kỳ ổn định, triệt tiêu độ trễ sóng.
  - Sử dụng cấu hình màu chuẩn `INVOFF` (`esp_lcd_panel_invert_color = false`), nền đen OLED, giao diện tối ưu cho taplo ô tô.
- 👉 **Nếu bạn muốn sử dụng kết nối không dây Bluetooth (BLE) hoặc xem tài liệu chi tiết đầy đủ, vui lòng xem tại:** **[Nhánh chính `CYD2.8-V2USB`](https://github.com/quangdo92/waze-hud/tree/CYD2.8-V2USB)**.

---

## 📥 Tải Firmware (Download)

👉 **Truy cập trang phát hành chính thức để tải file:** **[GitHub Releases](https://github.com/quangdo92/waze-hud/releases/latest)**

- **File Factory All-in-One cho nhánh này:** **[`waze_hud_cyd_28_usb_factory.bin`](https://github.com/quangdo92/waze-hud/releases/download/v2.8.2/waze_hud_cyd_28_usb_factory.bin)** (Nạp tại offset **`0x0`**).

---

## ⚡ Hướng dẫn nạp Firmware

### 🌐 Cách 1: Nạp qua Web Flasher (Khuyên dùng - Cực dễ)
1. Tải file **[`waze_hud_cyd_28_usb_factory.bin`](https://github.com/quangdo92/waze-hud/releases/download/v2.8.2/waze_hud_cyd_28_usb_factory.bin)** về máy tính hoặc điện thoại Android.
2. Cắm cáp USB kết nối mạch CYD với máy (cắm đúng cổng có chip truyền dữ liệu CH340).
3. Mở trình duyệt Chrome / Edge vào: 👉 **[https://wazemod.io.vn/flash-firmware](https://wazemod.io.vn/flash-firmware)**
4. Bấm **Kết nối**, chọn đúng cổng COM của mạch ESP32 CYD.
5. Chọn file `.bin` đã tải, đảm bảo địa chỉ nạp là **`0x0`** và bấm **Flash**.

*(Mẹo: Nếu mạch không tự động nhận chế độ flash, hãy nhấn giữ nút **BOOT**, bấm nhả nút **RESET**, sau đó thả nút **BOOT** rồi kết nối lại).*

### 💻 Cách 2: Nạp qua dòng lệnh esptool
```bash
python -m esptool --chip esp32 -b 460800 write_flash 0x0 waze-hud/dist/waze_hud_cyd_28_usb_factory.bin
```
