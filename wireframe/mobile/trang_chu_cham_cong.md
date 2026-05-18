### 5.2 Màn Hình 2: Trang Chủ Chấm Công

| Thuộc tính | Mô tả |
| --- | --- |
| Đối tượng | Nhân viên |
| Mục đích | Là màn hình chính để chấm công vào/ra và xem trạng thái trong ngày |
| Bottom nav active | Trang chủ |

Thành phần chính:

- Lời chào nhân viên và thông tin ca làm hiện tại.
- Card `Chấm công vào gần nhất`.
- Card `Giờ làm hôm nay`.
- Khối trạng thái gồm vị trí, định vị, độ chính xác GPS và trạng thái thiết bị tin cậy.
- Bản đồ nhỏ hoặc vùng hiển thị vị trí hiện tại trong vùng cho phép.
- Nút `Chấm công vào`.
- Nút `Chấm công ra`.
- Wire note giải thích app tự kiểm tra vị trí, thiết bị và khuôn mặt khi chấm công.

Luồng thao tác:

- Nhân viên mở tab `Trang chủ`.
- App hiển thị ca làm và trạng thái vị trí hiện tại.
- Nhân viên nhấn `Chấm công vào` khi bắt đầu ca.
- Nhân viên nhấn `Chấm công ra` khi kết thúc ca.
- Sau khi nhấn, app chuyển sang màn hình xác minh.

Trạng thái cần thể hiện:

- Chưa chấm công vào.
- Đã chấm công vào.
- Đã chấm công ra.
- Vị trí chưa sẵn sàng.
- Thiết bị đang chờ duyệt tin cậy.
