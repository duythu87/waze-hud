# Nhật Ký Thay Đổi (Changelog) - WazeHUD CYD 2.8"

Dự án WazeHUD tối ưu cho màn hình ESP32-2432S028 (CYD 2.8 inch). Toàn bộ firmware được phát hành dưới định dạng **Factory All-in-One** (đã bao gồm Bootloader, Partition Table và Firmware), nạp duy nhất 1 file tại offset **`0x0`**.

---

## [Beta 8 (v2.8.8)] - 2026-10-06

> Bản cập nhật lớn toàn diện tối ưu hóa trải nghiệm lái xe thực tế, nâng cấp cụm tốc độ taplo và chế độ chạy tự do so với bản Beta 7 (v2.8.2 phát hành ngày 05/10/2026).

### 🛑 Thiết kế lại cụm tốc độ Taplo hiện đại (Redesigned Speed Cluster)
- **Biển báo giới hạn tốc độ cỡ đại:** Phóng to vòng tròn biển báo tốc độ lên mức tối đa với bán kính **$R = 60\text{ px}$** (đường kính **$120\text{ px}$**), viền đỏ 100% đỏ cờ dày $10\text{ px}$, số bên trong to đậm nét giúp tài xế quan sát cực kỳ rõ ràng từ khoảng cách xa trên taplo.
- **Bỏ chữ "km/h" rườm rà:** Loại bỏ chữ km/h dưới số tốc độ giúp giao diện thoáng đãng, hiện đại, loại bỏ chi tiết thừa gây phân tâm khi lái xe.
- **Huy hiệu tốc độ xe thông minh ($38 \times 28\text{ px}$):** Tốc độ thực tế của xe được thu nhỏ số lại cho tinh tế và đặt gọn gàng trong huy hiệu bo góc tại góc dưới bên phải biển giới hạn ($x=100, y=100$).
- **Khắc phục lỗi tràn viền khi chạy tốc độ cao (> 100 km/h):** Khi xe chạy trên cao tốc ở tốc độ 3 chữ số (105, 115, 120 km/h), chữ số tự động chuyển sang cỡ vừa vặn nằm hoàn toàn bên trong huy hiệu, tuyệt đối không bao giờ lấn sang cột cảnh báo bên phải hay che mất đồng hồ.

### 🧭 Chế độ Chạy Tự Do hiển thị tối đa cảnh báo (Smart Free Drive Mode)
- **Tận dụng 100% dải đáy màn hình ($320 \times 82\text{ px}$):** Khi không cài lộ trình dẫn đường (chế độ chạy tự do), dải băng phía dưới tự động hiển thị đồng thời lên đến **4 thẻ cảnh báo độc lập** (Camera phạt nguội, Camera đèn đỏ, Điểm bắn tốc độ, Cảnh báo giao thông...).
- **Cảnh báo Quá Tốc Độ kích hoạt ở mọi chế độ lái:** Gỡ bỏ giới hạn chỉ cảnh báo khi có lộ trình dẫn đường. Giờ đây khi xe vượt tốc độ giới hạn (cả khi dẫn đường và khi chạy tự do), viền đỏ nhấp nháy toàn màn hình (2 Hz) và huy hiệu tốc độ xe chuyển sang nền đỏ rực cảnh báo sẽ luôn hoạt động chuẩn xác.

### 🎨 Tinh chỉnh độ nét đồ họa (Pixel-Perfect Graphics)
- **Khắc phục lỗi mờ/vỡ nét mũi tên phân làn:** Làm mượt các góc cua 90 độ và góc cua chữ U của mũi tên dẫn đường bằng thuật toán điền đầy hình tròn (fillCircle) tại các khớp nối. Các nét vẽ giờ đây sắc nét, mạch lạc và không còn hiện tượng răng cưa trên màn hình 320x240.
- **Biểu tượng Maneuver sắc nét:** Đảm bảo toàn bộ hệ thống biểu tượng chỉ đường (rẽ trái, phải, vòng xuyến) render chính xác từng pixel, mang lại trải nghiệm thị giác cao cấp.

### 📡 Ổn định kết nối & Hiệu năng hệ thống
- **Tương thích hoàn hảo WazeMod Android:** Cập nhật logic xử lý kết nối, đảm bảo kết nối Bluetooth BLE và USB Serial luôn duy trì liên tục và ổn định. Khắc phục triệt để hiện tượng thiết bị tự ngắt kết nối sau khi ghép nối thành công.

### 📦 Phát hành đồng bộ 4 biến thể phần cứng CYD 2.8" (Factory Offset 0x0)
1. **`waze_hud_cyd_28_factory.bin`**: Dành cho CYD 2.8" (2 Cổng USB) kết nối Bluetooth BLE.
2. **`waze_hud_cyd_28_1usb_factory.bin`**: Dành cho CYD 2.8" (1 Cổng Micro USB) kết nối Bluetooth BLE (đã cấu hình đảo màu & xoay chuẩn mạch 1 cổng).
3. **`waze_hud_cyd_28_usb_factory.bin`**: Dành cho CYD 2.8" (2 Cổng USB) cắm cáp USB Serial CH340.
4. **`waze_hud_cyd_28_1usb_serial_factory.bin`**: Dành cho CYD 2.8" (1 Cổng Micro USB) cắm cáp USB Serial UART.

---

## [v2.8.2] - 2026-10-05

- **Cảnh báo Đèn giao thông mới (Traffic Light Alert - Mã 75):** Bổ sung `AlertKind::TrafficLight = 75` đồng bộ với WazeMod APK, thiết kế icon pill-shape 3 bóng Đỏ - Vàng - Xanh.
- **Lưu giữ làn đường thông minh theo vận tốc (Speed-Aware Lane Retention):** Tạm dừng đếm ngược 15s khi tốc độ < 10 km/h để giữ nguyên chỉ dẫn làn khi dừng đèn đỏ hoặc kẹt xe.

