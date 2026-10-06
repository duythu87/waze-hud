# Nhật Ký Thay Đổi (Changelog) - WazeHUD CYD 2.8"

Dự án WazeHUD tối ưu cho màn hình ESP32-2432S028 (CYD 2.8 inch). Toàn bộ firmware được phát hành dưới định dạng **Factory All-in-One** (đã bao gồm Bootloader, Partition Table và Firmware), nạp duy nhất 1 file tại offset **`0x0`**.

---

## [v2.8.8] - 2026-10-06

> Bản cập nhật lớn toàn diện tối ưu hóa trải nghiệm lái xe thực tế, nâng cấp cụm tốc độ taplo và chế độ chạy tự do so với bản v2.8.2 (05/10/2026).

### 🛑 Thiết kế lại cụm tốc độ Taplo hiện đại (Redesigned Speed Cluster)
- **Biển báo giới hạn tốc độ cỡ đại:** Phóng to vòng tròn biển báo tốc độ lên bán kính tối đa **$R = 54\text{ px}$** (đường kính **$108\text{ px}$**), viền đỏ 100% đỏ cờ dày $11\text{ px}$, số bên trong to đậm nét chuẩn DejaVu Sans Bold giúp tài xế quan sát cực kỳ rõ ràng từ khoảng cách xa trên taplo.
- **Bỏ chữ "km/h" rườm rà:** Loại bỏ chữ km/h dưới số tốc độ giúp giao diện thoáng đãng, hiện đại, loại bỏ chi tiết thừa gây phân tâm khi lái xe.
- **Huy hiệu tốc độ xe thông minh ($44 \times 32\text{ px}$):** Tốc độ thực tế của xe được đặt gọn gàng trong huy hiệu bo góc tại góc dưới bên phải biển giới hạn ($x=177, y=94$).
- **Khắc phục lỗi tràn viền khi chạy tốc độ cao (> 100 km/h):** Khi xe chạy trên cao tốc ở tốc độ 3 chữ số (105, 115, 120 km/h), chữ số tự động chuyển sang cỡ vừa vặn nằm hoàn toàn bên trong huy hiệu, tuyệt đối không bao giờ lấn sang cột cảnh báo bên phải hay che mất đồng hồ.
- **Tối ưu hóa mã nguồn hiển thị:** Gỡ bỏ chế độ chia đôi layout cồng kềnh trước đây, hợp nhất thành một bố cục duy nhất thông minh, giảm tải CPU rendering.

### 🧭 Chế độ Chạy Tự Do hiển thị tối đa cảnh báo (Smart Free Drive Mode)
- **Tận dụng 100% dải đáy màn hình ($320 \times 82\text{ px}$):** Khi không cài lộ trình dẫn đường, dải băng phía dưới tự động hiển thị đồng thời lên đến **4 thẻ cảnh báo độc lập** (Camera phạt nguội, Camera đèn đỏ, Điểm bắn tốc độ, Cảnh báo giao thông...).
- **Chỉ báo lộ trình an toàn:** Khi phía trước hoàn toàn thông thoáng, hiển thị banner màu xanh lá *"LỘ TRÌNH THÔNG THOÁNG / AN TOÀN"*.
- **La bàn số Cruise:** Cột bên trái hiển thị la bàn số chỉ hướng xe chạy kèm trạng thái *"CHẠY TỰ DO - CRUISE"*.

### 🚨 Cảnh báo Quá Tốc Độ kích hoạt ở mọi chế độ lái
- **Cảnh báo ngay cả khi không bật dẫn đường:** Gỡ bỏ giới hạn chỉ cảnh báo khi có lộ trình dẫn đường. Giờ đây khi xe vượt tốc độ giới hạn (cả khi dẫn đường và khi chạy tự do):
  - **Viền đỏ nhấp nháy** toàn màn hình (2 Hz).
  - **Huy hiệu tốc độ xe** chuyển sang nền đỏ rực cảnh báo.
  - **Đèn LED RGB sau lưng** chớp nháy đỏ cảnh báo (khi chạy đúng tốc độ, LED tắt hoàn toàn để chống lóa mắt ban đêm).

### 📡 Ổn định kết nối & Hiệu năng hệ thống
- **Tương thích hoàn hảo WazeMod Android:** Chuẩn hóa gói cấu hình phản hồi 9 phần tử, đảm bảo kết nối Bluetooth BLE và USB Serial luôn duy trì liên tục và ổn định, loại bỏ hiện tượng tự ngắt kết nối.
- **Tối ưu hóa bộ nhớ:** Làm sạch font engine và thuật toán vẽ dirty-region giúp thiết bị phản hồi mượt mà ở tốc độ 4 Hz.

### 📦 Phát hành đồng bộ 4 biến thể phần cứng CYD 2.8" (Factory Offset 0x0)
1. **`waze_hud_cyd_28_factory.bin`**: Dành cho CYD 2.8" (2 Cổng USB) kết nối Bluetooth BLE.
2. **`waze_hud_cyd_28_1usb_factory.bin`**: Dành cho CYD 2.8" (1 Cổng Micro USB) kết nối Bluetooth BLE (đã cấu hình đảo màu & xoay chuẩn mạch 1 cổng).
3. **`waze_hud_cyd_28_usb_factory.bin`**: Dành cho CYD 2.8" (2 Cổng USB) cắm cáp USB Serial CH340.
4. **`waze_hud_cyd_28_1usb_serial_factory.bin`**: Dành cho CYD 2.8" (1 Cổng Micro USB) cắm cáp USB Serial UART.

---

## [v2.8.2] - 2026-10-05

- **Cảnh báo Đèn giao thông mới (Traffic Light Alert - Mã 75):** Bổ sung `AlertKind::TrafficLight = 75` đồng bộ với WazeMod APK, thiết kế icon pill-shape 3 bóng Đỏ - Vàng - Xanh.
- **Lưu giữ làn đường thông minh theo vận tốc (Speed-Aware Lane Retention):** Tạm dừng đếm ngược 15s khi tốc độ < 10 km/h để giữ nguyên chỉ dẫn làn khi dừng đèn đỏ hoặc kẹt xe.

---

## [v2.8.0] - 2026-09-24

- **True Black OLED Mode:** Nền đen tuyệt đối `0x0000` chống lóa trên taplo xe.
- **Vivid Red 100%:** Màu đỏ cờ `0xF800` cho toàn bộ viền biển báo tốc độ và biển cấm.
- **Tự động điều chỉnh độ sáng Ngày / Đêm theo thời gian thực:** 100% ban ngày, 30% ban đêm.
- **Tiết kiệm 27.6 KB Flash ROM:** Gỡ bỏ Boot logo tĩnh, căn giữa màn hình kết nối.
