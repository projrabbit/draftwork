### 5.3 Màn Hình 3: Xác Minh Vị Trí Và Gương Mặt

| Thuộc tính | Mô tả |
| --- | --- |
| Đối tượng | Nhân viên |
| Mục đích | Xác nhận nhanh vị trí, thiết bị và khuôn mặt trước khi ghi nhận chấm công |
| Nguồn mở màn hình | Sau khi nhấn chấm công vào hoặc chấm công ra |

Thành phần chính:

- Khung camera selfie để kiểm tra đúng người đang chấm công.
- Danh sách bước xác minh gồm lấy vị trí GPS 3D, kiểm tra độ tin cậy của vị trí, xác nhận khuôn mặt, kiểm tra liveness và kiểm tra vùng cho phép.
- Dữ liệu gửi đi gồm latitude, longitude, altitude, gps_accuracy, device_fingerprint, ảnh selfie và tín hiệu liveness.
- Nút `Gửi yêu cầu chấm công`.
- Nút `Hủy`.

Luồng thao tác:

- Nhân viên nhấn chấm công ở Trang chủ.
- App mở màn hình xác minh.
- Nhân viên chụp selfie hoặc cho phép camera xác nhận.
- App kiểm tra vị trí GPS 3D, độ chính xác GPS, thiết bị và tín hiệu khuôn mặt/liveness.
- Nhân viên nhấn `Gửi yêu cầu chấm công`.
- App chuyển sang màn hình thông báo trạng thái.

Trạng thái cần thể hiện:

- Đang lấy vị trí.
- Camera chưa cấp quyền.
- Vị trí không đủ tin cậy hoặc GPS accuracy kém.
- Xác minh thành công và đang gửi yêu cầu.
