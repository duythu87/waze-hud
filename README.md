# WazeHUD cho màn hình CYD 2.8 inch (Bản kết nối USB Serial - Tối ưu Taplo Ô tô)

> [!IMPORTANT]
> **PHIÊN BẢN KẾT NỐI CÓ DÂY QUA CỔNG USB SERIAL (CH340):**
> Nhánh này loại bỏ hoàn toàn Bluetooth Low Energy (BLE), chuyển sang truyền nhận dữ liệu HLP/1 trực tiếp qua cổng USB Serial ở tốc độ **115200 baud (8N1)**. Điện thoại Android cắm cáp USB OTG vào cổng micro/type-C của mạch CYD. Kết nối cực kỳ ổn định, khởi động nhận ngay lập tức, không có độ trễ sóng và giải phóng hơn **80 KB RAM** trên vi điều khiển ESP32.

---

## 📥 Tải Firmware (Download)

Các file binary đã được biên dịch hoàn chỉnh sẵn trong thư mục [`waze-hud/dist/`](./waze-hud/dist/):

| Tên file nhị phân | Offset Flash | Dung lượng | Mô tả & Khuyên dùng |
|---|:---:|:---:|---|
| **[`waze_hud_cyd_28_usb_factory.bin`](./waze-hud/dist/waze_hud_cyd_28_usb_factory.bin)** | `0x0` | ~1.2 MB | **⭐ KHUYÊN DÙNG:** Bản Flash All-in-One duy nhất (bao gồm Bootloader, Partition Table, OTA data và App). Nạp 1 lần chạy ngay. |
| **[`waze_hud_cyd_28_usb.bin`](./waze-hud/dist/waze_hud_cyd_28_usb.bin)** | `0x20000` | ~1.0 MB | Bản cập nhật ứng dụng (App only) khi mạch đã có sẵn phân vùng chuẩn. |

### Lệnh nạp nhanh qua esptool (Windows PowerShell / Linux Terminal)

```bash
# Nạp file Factory tại offset 0x0
python -m esptool --chip esp32 -b 460800 write_flash 0x0 waze-hud/dist/waze_hud_cyd_28_usb_factory.bin
```

*(Thay cổng COM tương ứng, ví dụ `--port COM12` trên Windows hoặc `--port /dev/ttyUSB0` trên Linux).*

> [!TIP]
> Nếu mạch không tự động vào chế độ Download: Giữ nút **BOOT (GPIO 0)**, nhấn nhả nút **RESET**, sau đó thả nút **BOOT** rồi tiến hành flash.

---

## ⚙️ Cấu hình Driver & Phần cứng

| Thông số / Phần cứng | Cấu hình trên nhánh `2.8-in-CYD-USB` |
|---|---|
| **Mạch mục tiêu (Target Board)** | ESP32-2432S028 (Cheap Yellow Display 2.8" ILI9341) |
| **Giao thức truyền dữ liệu** | **USB Serial UART0** (GPIO 1 TX, GPIO 3 RX) qua chip CH340 onboard |
| **Tốc độ truyền (Baud Rate)** | **115200 baud, 8 data bits, no parity, 1 stop bit (8N1)** |
| **Driver Màn hình (LCD Driver)** | `esp_lcd_ili9341` SPI2 @ 40 MHz, BGR element order |
| **Cấu hình Đảo màu (Color Invert)**| `INVOFF` (`esp_lcd_panel_invert_color = false`) - Sửa lỗi đảo màu trên CYD thực tế |
| **Cơ chế xoay (Rotation Transform)**| Xoay phần mềm dirty-stripe 240×320 ➔ 320×240 landscape mượt mà |
| **Sơ đồ chân SPI LCD** | MOSI: 13 \| MISO: 12 \| SCLK: 14 \| CS: 15 \| DC: 2 |
| **Đèn nền (Backlight)** | GPIO 21 (LEDC PWM 5 kHz, Tự động Ngày/Đêm) |
| **Đèn LED RGB sau lưng** | GPIO 4 (Đỏ), GPIO 16 (Xanh lá), GPIO 17 (Xanh dương) (Active-low) |
| **Nút bấm BOOT** | GPIO 0 (Active-low, đa tác vụ 1/2/nhấn giữ) |
| **Flash & Phân vùng** | 4MB Flash DIO 40MHz, 2 phân vùng OTA 1856 KB |

---

## 📋 Nhật ký thay đổi (Changelog)

### [2.8-in-CYD-USB] - 2026-10-05 (Bản nâng cấp toàn diện cho Taplo Ô tô)

#### 🚀 Giao tiếp & Hiệu năng (USB Serial Transport)
- **Truyền nhận USB Serial cực ổn định:** Loại bỏ hoàn toàn Bluetooth Low Energy, thay bằng `SerialTransport` đọc ghi frame JSON HLP/1 trực tiếp qua cổng USB-UART CH340 ở 115200 baud.
- **Tiết kiệm RAM & Khởi động tức thì:** Tiết kiệm hơn **80 KB RAM** so với bản BLE, triệt tiêu hoàn toàn nguy cơ rớt kết nối hoặc tràn heap.
- **Vô hiệu hóa Console Log:** Tắt bootloader và console log (`CONFIG_ESP_CONSOLE_NONE=y`) để bảo đảm đường truyền serial JSON cho Waze hoàn toàn sạch rác.

#### ✨ Đồ họa & Màu sắc chuẩn (Display & Colors)
- **Sửa lỗi đảo màu màn hình CYD:** Thiết lập `esp_lcd_panel_invert_color(panel, false)`. Khắc phục triệt để hiện tượng nền bị trắng và màu đỏ bị biến thành màu xanh lơ trên màn ILI9341 thực tế.
- **True Black OLED Mode:** Nền đen thuần túy `rgb565(0, 0, 0)` khử hoàn toàn ánh sáng xám mờ ban đêm trên taplo xe.
- **Viền biển báo Đỏ cờ rực rỡ (`0xF800`):** Nâng cấp toàn bộ biển báo tốc độ và biển cấm sang màu đỏ tươi rực rỡ 100% bão hòa, chuẩn nhận diện biển báo giao thông Việt Nam.

#### 📐 Bố cục & Typography (Layout Enhancements)
- **Đồng hồ thời gian:** Căn sát biên trên cùng bên phải (`x = width - textWidth - 5`, `y = 4`), hiển thị font trắng tinh (`colors::White`).
- **Tên đường tiếp theo (Next Street):** Font lớn 22px (`assets::kTextMedium`) ở góc trên trái, tự động chạy chữ (marquee scroll) mượt mà khi chiều dài vượt quá 75px.
- **Cân đối khu vực rẽ:** Hạ thấp mũi tên chỉ đường và kéo số mét rẽ xuống `y = 108` màu trắng rõ nét, loại bỏ hoàn toàn khoảng trống thừa ở đáy cột trái.
- **Thông tin chuyến đi (Trip Info):** Khi không có làn đường, hàng dưới cùng tự động chuyển sang hiển thị số **KM còn lại** và **Thời gian dự kiến (phút)**.

#### 🧠 Thuật toán & An toàn lái xe (Smart Logic & Alerts)
- **Cảnh báo quá tốc độ khẩn cấp (Overspeed Emergency Visual Alert):** Khung viền đỏ 4px bao quanh toàn bộ 4 cạnh màn hình chớp nháy 2Hz + số tốc độ xe nhấp nháy đỏ/trắng khi chạy quá tốc độ giới hạn.
- **Ưu tiên thông minh các cảnh báo (Alert Prioritization):**
  - Biển giảm tốc độ `SpeedDrop` (Điểm 100).
  - Biển cấm (Cấm vượt, cấm rẽ, cấm ô tô...) (Điểm 90).
  - Nguy hiểm, tai nạn, đóng đường (Điểm 70).
  - Camera tốc độ gần (< 250m) (Điểm 60).
  - Ùn tắc giao thông (Điểm 50).
  - Camera ở xa > 250m (Điểm 10).
  *(Tránh triệt để việc camera cách 1km đè mất cảnh báo giảm tốc độ nguy hiểm ngay trước mặt).*
- **Giữ làn đường thông minh (Lane Retention):** Giữ làn đường thêm 15 giây khi xe tiến vào ngã tư, không bị mất làn đột ngột khi qua giao lộ.
- **Tự động chỉnh độ sáng Ngày / Đêm:**
  - `06:00 – 17:30`: Sáng 100% chống chói nắng.
  - `17:30 – 19:00`: Giảm dần đều từ 100% về 30%.
  - `19:00 – 05:00`: Duy trì 30% chống lóa mắt ban đêm.
  - `05:00 – 06:00`: Tăng dần đều từ 30% lên 100%.

---

## 🔌 Hướng dẫn kết nối với Waze Mod

1. Dùng điện thoại Android hỗ trợ USB OTG và cáp USB truyền dữ liệu (Type-C sang Type-C hoặc Type-C sang Micro-USB tùy loại cổng trên mạch CYD).
2. Cắm mạch CYD vào điện thoại. Khi Android hiện thông báo yêu cầu cấp quyền truy cập USB cho ứng dụng Waze Mod, chọn **Đồng ý / Luôn mở**.
3. Trong menu **HUD Link** của Waze Mod:
   - Chọn phương thức kết nối: **USB Serial**.
   - Chọn thiết bị USB CH340 theo danh sách.
   - Đặt tốc độ truyền: **115200 baud**, **8N1**.
4. Mở lộ trình dẫn đường trong Waze, màn hình HUD sẽ lập tức hiển thị dữ liệu dẫn đường theo thời gian thực.

---

## 🔘 Thao tác với nút bấm cứng (BOOT)

| Thao tác nút BOOT | Tác dụng |
|---|---|
| **Nhấn 1 lần** | Xoay ngược màn hình 180° (tiện cho việc cắm cáp từ trên xuống hoặc từ dưới lên) |
| **Nhấn đúp (2 lần)** | Bật / Tắt chế độ lật gương (Mirror HUD) chiếu hắt kính lái |
| **Nhấn giữ** | Mở màn hình chẩn đoán (hiển thị trạng thái kết nối `USB ĐÃ KẾT NỐI`) |

*Các thiết lập xoay và lật gương được tự động lưu vào bộ nhớ flash NVS và không bị mất khi rút nguồn.*

---

## 💡 Trạng thái đèn LED RGB phía sau

| Trạng thái xe & kết nối | Màu LED RGB | Ý nghĩa |
|---|---|---|
| Chưa kết nối điện thoại | Đổi màu RGB liên tục | Đang chờ cắm cáp USB |
| Đã cắm cáp, chờ dữ liệu dẫn đường | Xanh dương | Đã nhận diện cổng USB Serial |
| Tốc độ bình thường (đang dẫn đường) | **TẮT** | Giữ khoang lái tối dịu mắt ban đêm |
| Chạy quá tốc độ giới hạn | **Nháy đỏ 2 Hz** | Cảnh báo xe đang chạy vượt tốc độ cho phép |
