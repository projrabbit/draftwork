### 5.4 Màn Hình 4: Thông Báo Trạng Thái

| Thuộc tính | Mô tả |
| --- | --- |
| Đối tượng | Nhân viên |
| Mục đích | Hiển thị kết quả xử lý ngay sau khi chấm công |
| Tính chất | Màn hình phản hồi trong luồng chấm công, không phải tab bottom nav |

Thành phần chính:

- Card trạng thái chính gồm kết quả, thông báo, vị trí và thời gian chờ.
- Khối trường hợp ngoài vùng cho phép.
- Khối trường hợp vị trí, thiết bị hoặc xác minh khuôn mặt/liveness không tin cậy.
- Khối trường hợp timeout hoặc lỗi kết nối.
- Nút `Về trang chủ`.

Luồng thao tác:

- App nhận kết quả xử lý chấm công.
- Nếu thành công, app hiển thị thông báo chấm công thành công và vị trí ghi nhận.
- Nếu ngoài vùng, app thông báo nhân viên đang ngoài khu vực chấm công.
- Nếu vị trí, thiết bị, khuôn mặt hoặc liveness không tin cậy, app thông báo bị từ chối và hướng dẫn liên hệ HR.
- Nếu lỗi mạng, app cho phép thử lại.
- Nhân viên nhấn `Về trang chủ` để quay lại màn hình chính.

Trạng thái cần thể hiện:

- Thành công.
- Ngoài vùng cho phép.
- Vị trí, thiết bị hoặc xác minh khuôn mặt/liveness không tin cậy.
- Lỗi kết nối hoặc timeout.
