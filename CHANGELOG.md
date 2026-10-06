# Nhật Ký Thay Đổi (Changelog) - WazeHUD CYD 2.8"

Dự án WazeHUD tối ưu cho màn hình ESP32-2432S028 (CYD 2.8 inch). Toàn bộ firmware được phát hành dưới định dạng **Factory All-in-One** (đã bao gồm Bootloader, Partition Table và Firmware), nạp duy nhất 1 file tại offset **`0x0`**.

---

## [Bản B8] - 2026-10-06

### 🚀 Cải tiến nổi bật & Đột phá trong bản B8

#### 1. 🛑 Cụm Biển Giới Hạn Tốc Độ Cỡ Đại (Enlarged Speed Limit Cluster R=54, D=108 px)
- Phóng to vòng tròn biển báo tốc độ lên tối đa bán kính **$R = 54\text{ px}$** (đường kính **$108\text{ px}$**), chiếm trọn chiều cao khu vực trung tâm ($130\text{ px}$).
- Đường viền đỏ bão hòa 100% dày $11\text{ px}$, diện tích lòng trắng được tối ưu vừa vặn cho con số to.
- Chữ số giới hạn tốc độ được nâng cấp lên font đậm lớn nhất (`kSpeedLargeBold` 38 px cho 2 chữ số, `kSpeedMediumBold` 26 px cho 3 chữ số như 100, 120 km/h).
- Biển báo hiển thị cực kỳ sắc nét, loại bỏ hoàn toàn hiện tượng hiển thị lỗi dấu hỏi `???`.

#### 2. 🏷️ Huy Hiệu Tốc Độ Xe Hiện Tại Thông Minh (Smart Bottom-Right Speed Badge)
- **Bỏ chữ "km/h" rườm rà:** Loại bỏ chữ km/h dưới tốc độ xe giúp không gian gọn gàng, hiện đại và tập trung lái xe.
- **Huy hiệu chuyên dụng $44 \times 32\text{ px}$:** Đặt tinh tế tại góc dưới bên phải biển giới hạn ($x=177, y=94$).
- **Không bao giờ tràn viền:** Khi xe chạy $> 100\text{ km/h}$ (105, 115, 120 km/h), chữ số tự động căn chỉnh font vừa vặn nằm hoàn toàn bên trong huy hiệu, tuyệt đối không lấn sang cột cảnh báo bên phải hay che mất đồng hồ.
- **Gọn gàng mã nguồn:** Bỏ chế độ chia đôi 1 bên biển 1 bên tốc độ cồng kềnh, quy tụ về 1 layout duy nhất thông minh, tiết kiệm chu kỳ vẽ CPU.

#### 3. 🧭 Chế Độ Chạy Tự Do Tối Ưu (Smart Free Drive Mode with Full Multi-Alert Display)
- Khi xe chạy tự do (không bật dẫn đường): Tận dụng triệt để toàn bộ dải băng bên dưới ($320 \times 82\text{ px}$) để hiển thị đồng thời lên đến **4 thẻ cảnh báo độc lập** (Camera phạt nguội, Camera đèn đỏ, Điểm bắn tốc độ, Chướng ngại vật...).
- Khi cung đường phía trước an toàn: Hiển thị banner xanh lá dịu mắt *"LỘ TRÌNH THÔNG THOÁNG / AN TOÀN"*.
- Cột bên trái chuyển đổi linh hoạt hiển thị la bàn số & chỉ báo *"CHẠY TỰ DO - CRUISE"*.

#### 4. 🚨 Kích Hoạt Cảnh Báo Quá Tốc Độ Mọi Chế Độ (Overspeed Warning in Free Drive)
- **Khắc phục lỗi cảnh báo:** Gỡ bỏ điều kiện `navigationActive` trong thuật toán cảnh báo quá tốc độ.
- Khi tốc độ xe vượt quá giới hạn (dù đang dẫn đường hay đang chạy tự do):
  - **Viền đỏ nhấp nháy** toàn màn hình với tần số 2 Hz.
  - **Huy hiệu tốc độ** chuyển sang màu đỏ rực rỡ và nhấp nháy cảnh báo.
  - **Đèn LED RGB sau lưng** chớp nháy đỏ 2 Hz (tắt hoàn toàn khi chạy đúng tốc độ để không chói mắt).

#### 5. 📡 Đồng Bộ Giao Thức HLP & Ổn Định Kết Nối Tuyệt Đối
- Chuẩn hóa gói cấu hình phản hồi 9 phần tử (`device_config`) tương thích 100% với ứng dụng WazeMod Android.
- Khắc phục triệt để hiện tượng ngắt kết nối đột ngột sau khi vừa kết nối BLE trên bản B3.
- Cơ chế giải mã gói HLP an toàn, chống tràn bộ đệm và lọc nhiễu gói tin.

#### 6. 📦 Phát Hành Đồng Bộ 4 Biến Thể Phần Cứng (Factory Offset 0x0)
1. **`waze_hud_cyd_28_factory_20261006_b8.bin`**: Dành cho CYD 2.8" (2 cổng USB Type-C/Micro) kết nối Bluetooth BLE.
2. **`waze_hud_cyd_28_1usb_factory_20261006_b8.bin`**: Dành cho CYD 2.8" (1 cổng USB Micro) kết nối Bluetooth BLE (đã cấu hình đảo màu & xoay màn hình chuẩn phần cứng v1).
3. **`waze_hud_cyd_28_usb_factory_20261006_b8.bin`**: Dành cho CYD 2.8" (2 cổng USB) kết nối qua Cáp USB Serial CH340.
4. **`waze_hud_cyd_28_1usb_serial_factory_20261006_b8.bin`**: Dành cho CYD 2.8" (1 cổng USB Micro) kết nối qua Cáp USB Serial UART.

---

## [Bản B2 - B7] - 2026-10-06
- Tối ưu hóa bộ font số đậm `kSpeedLargeBold` và `kSpeedMediumBold` từ DejaVu Sans Bold.
- Thử nghiệm các bố cục cụm tốc độ: Biển tròn R=44px, R=48px và chuẩn hóa R=54px.
- Tinh chỉnh khoảng cách giữa các khu vực hiển thị rẽ, cụm tốc độ, cảnh báo phụ và đồng hồ.

---

## [Bản v2.8.2] - 2026-10-05
- **Cảnh báo Đèn giao thông mới (Traffic Light Alert - Mã 75):** Bổ sung `AlertKind::TrafficLight = 75` đồng bộ với WazeMod APK, thiết kế icon pill-shape 3 bóng Đỏ - Vàng - Xanh.
- **Lưu giữ làn đường thông minh theo vận tốc (Speed-Aware Lane Retention):** Tạm dừng đếm ngược 15s khi tốc độ < 10 km/h để giữ nguyên chỉ dẫn làn khi dừng đèn đỏ hoặc kẹt xe.

---

## [Bản v2.8.0] - 2026-09-24
- **True Black OLED Mode:** Nền đen tuyệt đối `0x0000` chống lóa trên taplo xe.
- **Vivid Red 100%:** Màu đỏ cờ `0xF800` cho toàn bộ viền biển báo tốc độ và biển cấm.
- **Tự động điều chỉnh độ sáng Ngày / Đêm theo thời gian thực:** 100% ban ngày, 30% ban đêm.
- **Tiết kiệm 27.6 KB Flash ROM:** Gỡ bỏ Boot logo tĩnh, căn giữa màn hình kết nối.

