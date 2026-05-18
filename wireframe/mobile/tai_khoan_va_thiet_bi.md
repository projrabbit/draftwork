### 5.7 Màn Hình 7: Tài Khoản Và Thiết Bị

| Thuộc tính | Mô tả |
| --- | --- |
| Đối tượng | Nhân viên |
| Mục đích | Cho nhân viên xem thông tin cá nhân và thiết bị đang dùng để chấm công |
| Bottom nav active | Tài khoản |

Thành phần chính:

- Placeholder ảnh đại diện hoặc khuôn mặt đã đăng ký.
- Thông tin cá nhân gồm nhân viên, phòng ban, email và quyền.
- Thông tin thiết bị hiện tại gồm hệ điều hành, dòng máy, mã thiết bị đã ẩn và trạng thái tin cậy.
- Nút `Đăng xuất`.
- Wire note giải thích thiết bị chưa được duyệt tin cậy sẽ nhắc nhân viên liên hệ HR/Admin.

Luồng thao tác:

- Nhân viên mở tab `Tài khoản`.
- App hiển thị thông tin cá nhân.
- App hiển thị thiết bị hiện tại và trạng thái tin cậy.
- Nếu thiết bị chưa được duyệt tin cậy, nhân viên biết cần liên hệ HR/Admin.
- Nhân viên có thể nhấn `Đăng xuất` để thoát tài khoản.

Trạng thái cần thể hiện:

- Thiết bị đã được duyệt tin cậy.
- Thiết bị đang chờ duyệt tin cậy.
- Tài khoản đang hoạt động.
- Tài khoản bị khóa sẽ không vào được màn hình này do bị chặn từ đăng nhập.
