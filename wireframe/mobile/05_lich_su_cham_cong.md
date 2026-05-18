### 5.5 Màn Hình 5: Lịch Sử Chấm Công Cá Nhân

| Thuộc tính | Mô tả |
| --- | --- |
| Đối tượng | Nhân viên |
| Mục đích | Cho nhân viên tự xem lại lịch sử chấm công cá nhân |
| Bottom nav active | Lịch sử |

Thành phần chính:

- Tab lọc nhanh theo ngày, tuần và tháng.
- Trường chọn khoảng thời gian.
- Card tổng giờ làm.
- Card số lần đi trễ.
- Danh sách lịch sử theo ngày gồm giờ vào, giờ ra, vị trí và trạng thái.
- Wire note cho trường hợp không có dữ liệu.

Luồng thao tác:

- Nhân viên mở tab `Lịch sử`.
- Nhân viên chọn ngày, tuần hoặc tháng.
- Nhân viên chọn khoảng thời gian cần xem.
- App hiển thị tổng giờ, số lần đi trễ và danh sách chi tiết.
- Nếu không có dữ liệu, app hiển thị thông báo trống.

Trạng thái cần thể hiện:

- Có dữ liệu chấm công.
- Không có dữ liệu trong khoảng thời gian chọn.
- Có ngày thiếu lượt chấm công ra.
- Có lượt bị từ chối do ngoài vùng cho phép.
