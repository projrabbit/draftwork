# Mô Tả Data Flow Diagram - Hệ Thống Chấm Công Thông Minh

Tài liệu này mô tả luồng dữ liệu tương ứng với các sơ đồ DFD trong thư mục `DFD/`. Nội dung được viết theo dạng thuật toán từng bước để dễ đưa vào báo cáo.

## Danh Sách Sơ Đồ DFD

| STT | File sơ đồ | Nội dung |
| --- | --- | --- |
| 1 | `DFD_Level0_Context.puml` | DFD Level 0 - Context Diagram |
| 2 | `DFD_Level1_Overview.puml` | DFD Level 1 - Tổng quan các tiến trình chính |
| 3 | `DFD_L2_Authentication.puml` | DFD Level 2 - Xác thực và phân quyền |
| 4 | `DFD_L2_Attendance.puml` | DFD Level 2 - Chấm công |
| 5 | `DFD_L2_FraudDetection.puml` | DFD Level 2 - Kiểm tra chống gian lận |
| 6 | `DFD_L2_GIS_Management.puml` | DFD Level 2 - Quản lý GIS và Geofence |
| 7 | `DFD_L2_SystemManagement.puml` | DFD Level 2 - Quản lý hệ thống |
| 8 | `DFD_L2_Reporting.puml` | DFD Level 2 - Báo cáo và Dashboard |

## 1. DFD Level 0 - Context Diagram

File sơ đồ: `DFD/DFD_Level0_Context.puml`

Sơ đồ ngữ cảnh mô tả hệ thống chấm công thông minh như một tiến trình tổng quát, trao đổi dữ liệu với các tác nhân bên ngoài gồm Nhân viên, HR/Admin, CEO và ArcGIS Platform.

Thuật toán:

Bước 1: Nhân viên sử dụng Mobile App gửi yêu cầu check-in/check-out kèm tọa độ GPS 3D gồm latitude, longitude, altitude và thông tin thiết bị.

Bước 2: Hệ thống tiếp nhận dữ liệu, xử lý xác minh vị trí, kiểm tra thiết bị và trả kết quả chấm công cho nhân viên.

Bước 3: Nếu nhân viên yêu cầu xem lịch sử, hệ thống trả về danh sách bản ghi chấm công cá nhân và thông báo trạng thái liên quan.

Bước 4: HR/Admin sử dụng Web Dashboard để cấu hình geofence, ca làm việc, nhân viên, thiết bị, ngoại lệ và audit log.

Bước 5: Hệ thống trả về cho HR/Admin báo cáo chấm công, dashboard KPI, cảnh báo gian lận và bản đồ GIS 3D real-time.

Bước 6: CEO gửi yêu cầu xem dashboard hoặc báo cáo tổng hợp. Hệ thống trả về KPI tổng quan và các số liệu chấm công đã tổng hợp.

Bước 7: Hệ thống gửi layer ID tòa nhà và tọa độ marker đến ArcGIS Platform. ArcGIS Platform trả về Scene View 3D và kết quả xác nhận layer hợp lệ.

Bước 8: Kết thúc luồng ngữ cảnh.

## 2. DFD Level 1 - Tổng Quan Các Tiến Trình Chính

File sơ đồ: `DFD/DFD_Level1_Overview.puml`

Sơ đồ Level 1 phân rã hệ thống thành các tiến trình chính gồm xác thực, chấm công, chống gian lận, quản lý GIS/geofence, quản lý hệ thống và báo cáo/dashboard.

Thuật toán:

Bước 1: Người dùng gửi thông tin đăng nhập đến tiến trình 1.0 Xác thực và Phân quyền.

Bước 2: Tiến trình 1.0 truy vấn kho dữ liệu Account & Employee để xác thực tài khoản, cập nhật `last_login_at` và trả JWT token cho người dùng nếu hợp lệ.

Bước 3: Khi nhân viên check-in/check-out, tiến trình 2.0 Chấm công nhận dữ liệu GPS, device_fingerprint và loại yêu cầu.

Bước 4: Tiến trình 2.0 lấy thông tin Employee, Device, Shift và geofence đang hoạt động từ các kho dữ liệu liên quan.

Bước 5: Tiến trình 2.0 chuyển dữ liệu GPS và thiết bị sang tiến trình 3.0 Kiểm tra chống gian lận.

Bước 6: Tiến trình 3.0 phân tích mock location, GPS spoofing, thiết bị lạ, chấm công hộ và trả FraudResult về tiến trình Chấm công.

Bước 7: Tiến trình 2.0 tạo AttendanceRecord, ghi AuditLog và trả kết quả chấm công cho nhân viên.

Bước 8: HR/Admin gửi yêu cầu cấu hình tòa nhà, tầng và geofence đến tiến trình 4.0 Quản lý GIS & Geofence.

Bước 9: Tiến trình 4.0 cập nhật dữ liệu Building, Floor, Geofence hoặc GeofenceRule và trao đổi dữ liệu hiển thị với ArcGIS Platform.

Bước 10: HR/Admin gửi yêu cầu quản lý nhân viên, thiết bị, ca làm việc hoặc phòng ban đến tiến trình 5.0 Quản lý hệ thống.

Bước 11: Tiến trình 5.0 cập nhật các kho dữ liệu Account & Employee, Shift & Department và Audit Log.

Bước 12: HR/Admin hoặc CEO gửi yêu cầu báo cáo đến tiến trình 6.0 Báo cáo & Dashboard.

Bước 13: Tiến trình 6.0 tổng hợp AttendanceRecord, FraudDetection và Employee để tạo bảng báo cáo, dashboard KPI và dữ liệu bản đồ mini.

Bước 14: Kết thúc.

## 3. DFD Level 2 - Xác Thực Và Phân Quyền

File sơ đồ: `DFD/DFD_L2_Authentication.puml`

Sơ đồ này mô tả chi tiết tiến trình 1.0. Luồng dữ liệu tập trung vào việc xác thực username/password, kiểm tra trạng thái tài khoản, cấp JWT token và ghi nhận AuditLog đăng nhập.

Thuật toán:

Bước 1: Người dùng nhập username và password trên Mobile App hoặc Web Dashboard.

Bước 2: Hệ thống gửi username và password đến tiến trình 1.1 Xác thực credentials.

Bước 3: Tiến trình 1.1 truy vấn bảng Account theo username để lấy password_hash.

Bước 4: Nếu không tìm thấy tài khoản hoặc mật khẩu không khớp, hệ thống trả lỗi đăng nhập cho người dùng.

Bước 5: Nếu tài khoản tồn tại, tiến trình 1.2 kiểm tra trạng thái `is_active` của tài khoản.

Bước 6: Nếu tài khoản bị khóa, hệ thống trả thông báo tài khoản không hợp lệ hoặc đã bị khóa.

Bước 7: Nếu tài khoản hợp lệ, tiến trình 1.3 tạo JWT token dựa trên role và employee_id.

Bước 8: Tiến trình 1.4 cập nhật `last_login_at` vào bảng Account và ghi AuditLog hành động LOGIN.

Bước 9: Hệ thống trả JWT token và role về cho client.

Bước 10: Client điều hướng người dùng đến màn hình chính phù hợp với vai trò.

## 4. DFD Level 2 - Chấm Công

File sơ đồ: `DFD/DFD_L2_Attendance.puml`

Sơ đồ này mô tả tiến trình 2.0 Chấm công, bao gồm thu thập GPS, đối chiếu geofence 3D, tạo bản ghi chấm công, trả thông báo và xem lịch sử cá nhân.

Thuật toán:

Bước 1: Nhân viên gửi yêu cầu check-in hoặc check-out kèm GPS 3D, gps_accuracy, device_fingerprint và loại yêu cầu.

Bước 2: Tiến trình 2.1 Thu thập & Validate GPS truy vấn kho Employee & Device để lấy thông tin nhân viên, thiết bị và trạng thái tin cậy.

Bước 3: Tiến trình 2.1 gửi dữ liệu GPS và thông tin thiết bị sang tiến trình 3.0 Chống gian lận.

Bước 4: Tiến trình 3.0 trả về FraudResult gồm điểm tin cậy và các cờ gian lận.

Bước 5: Tiến trình 2.1 chuyển tọa độ GPS đã validate sang tiến trình 2.2 Đối chiếu Geofence 3D.

Bước 6: Tiến trình 2.2 truy vấn geofence hoặc GeofenceRule đang hoạt động theo tầng và khoảng độ cao.

Bước 7: Tiến trình 2.2 kiểm tra latitude/longitude có nằm trong bán kính cho phép và altitude có nằm trong khoảng độ cao hợp lệ hay không.

Bước 8: Kết quả đối chiếu geofence hoặc lý do từ chối được chuyển sang tiến trình 2.3 Tạo bản ghi chấm công.

Bước 9: Tiến trình 2.3 lấy thông tin Shift để tính trạng thái đi trễ hoặc về sớm.

Bước 10: Tiến trình 2.3 tạo AttendanceRecord với type, status, tọa độ GPS, geofence_id hoặc geofence_rule_id, shift_id và các cờ liên quan.

Bước 11: Tiến trình 2.3 ghi AuditLog cho hành động check-in/check-out.

Bước 12: Tiến trình 2.4 trả thông báo tức thời cho nhân viên, gồm approved hoặc rejected và lý do nếu bị từ chối.

Bước 13: Nếu nhân viên xem lịch sử, tiến trình 2.5 nhận khoảng thời gian và truy vấn AttendanceRecord theo employee_id.

Bước 14: Tiến trình 2.5 trả danh sách bản ghi chấm công và tổng giờ làm cho nhân viên.

Bước 15: Kết thúc.

## 5. DFD Level 2 - Kiểm Tra Chống Gian Lận

File sơ đồ: `DFD/DFD_L2_FraudDetection.puml`

Sơ đồ này mô tả tiến trình 3.0 Kiểm tra chống gian lận, được kích hoạt trong quy trình chấm công. Dữ liệu đầu vào gồm GPS, raw signals, thiết bị, ảnh gương mặt và tín hiệu liveness.

Thuật toán:

Bước 1: Tiến trình 2.0 Chấm công gửi GPS data, raw_signals và thông tin thiết bị đến tiến trình 3.1 Kiểm tra Mock Location.

Bước 2: Tiến trình 3.1 đọc cờ mock location từ hệ điều hành thiết bị và tạo kết quả `mock_location_detected`.

Bước 3: Tiến trình 3.2 nhận latitude, longitude, altitude, gps_accuracy và employee_id.

Bước 4: Tiến trình 3.2 truy vấn lịch sử 3 vị trí gần nhất từ AttendanceRecord để tính tốc độ di chuyển và phát hiện GPS spoofing.

Bước 5: Tiến trình 3.2 kiểm tra các dấu hiệu bất thường như tốc độ quá cao, altitude bất thường hoặc gps_accuracy không hợp lệ.

Bước 6: Tiến trình 3.3 kiểm tra device_fingerprint trong kho Device & Employee Face Template.

Bước 7: Tiến trình 3.3 xác định thiết bị có được tin cậy hay không và kiểm tra khả năng một thiết bị được dùng bởi nhiều nhân viên trong thời gian gần.

Bước 8: Tiến trình 3.3 phân tích impossible travel để phát hiện nghi ngờ chấm công hộ.

Bước 9: Tiến trình 3.4 lấy face_template của nhân viên và gửi face_image, liveness_signals đến Face Verification Service.

Bước 10: Face Verification Service trả về điểm khớp gương mặt, điểm liveness, kết quả face_matched và liveness_passed.

Bước 11: Tiến trình 3.5 tổng hợp các cờ gian lận để tính confidence_score.

Bước 12: Tiến trình 3.6 lưu FraudDetection vào database, gồm các flags, score và raw_signals.

Bước 13: Nếu có cảnh báo, tiến trình 3.6 ghi AuditLog tương ứng.

Bước 14: FraudResult được trả về tiến trình 2.0 để quyết định approve hoặc reject bản ghi chấm công.

Bước 15: Kết thúc.

## 6. DFD Level 2 - Quản Lý GIS Và Geofence

File sơ đồ: `DFD/DFD_L2_GIS_Management.puml`

Sơ đồ này mô tả tiến trình 4.0 Quản lý GIS & Geofence, bao gồm quản lý tòa nhà/tầng, cấu hình geofence 3D và hiển thị bản đồ GIS 3D real-time.

Thuật toán:

Bước 1: Admin nhập thông tin tòa nhà gồm tên, địa chỉ, tọa độ trung tâm, tổng số tầng và arcgis_layer_id.

Bước 2: Tiến trình 4.1 gửi arcgis_layer_id sang ArcGIS Platform để kiểm tra layer tồn tại và hợp lệ.

Bước 3: Nếu layer hợp lệ, tiến trình 4.1 lưu Building và Floor vào kho Building & Floor, đồng thời ghi AuditLog.

Bước 4: Hệ thống xác nhận tạo tòa nhà/tầng thành công và render tòa nhà trên bản đồ nếu có layer phù hợp.

Bước 5: Admin/HR cấu hình geofence 3D bằng cách nhập floor_id, tọa độ trung tâm, bán kính và khoảng altitude_min/altitude_max.

Bước 6: Tiến trình 4.2 truy vấn geofence hiện có theo floor_id để kiểm tra overlap.

Bước 7: Nếu vùng bị trùng, hệ thống trả cảnh báo cho Admin/HR.

Bước 8: Nếu hợp lệ, tiến trình 4.2 lưu hoặc cập nhật Geofence/GeofenceRule và ghi AuditLog.

Bước 9: Tiến trình 4.2 gửi dữ liệu vùng geofence sang ArcGIS Platform để vẽ hoặc xóa trên bản đồ.

Bước 10: Admin/HR yêu cầu xem bản đồ 3D real-time với bộ lọc tòa nhà, tầng hoặc phòng ban.

Bước 11: Tiến trình 4.3 truy vấn AttendanceRecord để lấy danh sách nhân viên đang check-in và chưa check-out.

Bước 12: Tiến trình 4.3 lấy thông tin Building và Floor để xác định tầng/tòa nhà tương ứng.

Bước 13: Tiến trình 4.3 gửi tọa độ marker và arcgis_layer_id sang ArcGIS Platform.

Bước 14: ArcGIS Platform render Scene View và trả về bản đồ 3D với markers nhân viên cho Admin/HR.

Bước 15: Bản đồ được cập nhật định kỳ theo cấu hình auto-refresh.

## 7. DFD Level 2 - Quản Lý Hệ Thống

File sơ đồ: `DFD/DFD_L2_SystemManagement.puml`

Sơ đồ này mô tả tiến trình 5.0 Quản lý hệ thống, gồm quản lý nhân viên/phòng ban, ca làm việc, ngoại lệ chấm công, thiết bị tin cậy và audit log.

Thuật toán:

Bước 1: Admin/HR gửi thông tin nhân viên mới hoặc thông tin cần cập nhật đến tiến trình 5.1 Quản lý Nhân viên & Phòng ban.

Bước 2: Tiến trình 5.1 insert/update Employee, Account, Device và Department trong các kho dữ liệu liên quan.

Bước 3: Tiến trình 5.1 ghi AuditLog và trả xác nhận cho Admin/HR.

Bước 4: Admin/HR cấu hình ca làm việc gồm tên ca, giờ bắt đầu, giờ kết thúc, mức chịu trễ, mức về sớm và trạng thái áp dụng cuối tuần.

Bước 5: Tiến trình 5.2 kiểm tra xung đột giờ giữa các ca.

Bước 6: Nếu hợp lệ, tiến trình 5.2 lưu Shift, gán ca cho nhân viên nếu có và ghi AuditLog.

Bước 7: Admin/HR yêu cầu xem ngoại lệ chấm công.

Bước 8: Tiến trình 5.3 truy vấn AttendanceRecord có trạng thái rejected, đi trễ hoặc về sớm.

Bước 9: Tiến trình 5.3 lấy thêm FraudDetection để hiển thị các cờ gian lận và chi tiết bất thường.

Bước 10: Nếu Admin/HR phê duyệt thủ công, tiến trình 5.3 cập nhật AttendanceRecord và ghi AuditLog MANUAL_APPROVE.

Bước 11: Admin cập nhật trạng thái tin cậy của thiết bị trong tiến trình 5.4.

Bước 12: Tiến trình 5.4 cập nhật `Device.is_trusted`, ghi AuditLog DEVICE_TRUST hoặc DEVICE_UNTRUST và trả xác nhận.

Bước 13: Admin nhập điều kiện lọc audit log trong tiến trình 5.5.

Bước 14: Tiến trình 5.5 truy vấn AuditLog theo actor, action_type hoặc khoảng thời gian và trả danh sách log chỉ đọc.

Bước 15: Kết thúc.

## 8. DFD Level 2 - Báo Cáo Và Dashboard

File sơ đồ: `DFD/DFD_L2_Reporting.puml`

Sơ đồ này mô tả tiến trình 6.0 Báo cáo & Dashboard, phục vụ xem báo cáo chấm công, xuất file và xem KPI tổng quan.

Thuật toán:

Bước 1: HR/Admin nhập bộ lọc báo cáo gồm khoảng thời gian, phòng ban và nhân viên nếu có.

Bước 2: Tiến trình 6.1 truy vấn AttendanceRecord theo điều kiện lọc.

Bước 3: Tiến trình 6.1 truy vấn Employee & Department để bổ sung thông tin nhân viên/phòng ban cho báo cáo.

Bước 4: Tiến trình 6.1 tổng hợp số ngày công, tổng giờ làm, số lần trễ, số lần về sớm, số lần vắng mặt và số bản ghi bị từ chối.

Bước 5: Tiến trình 6.1 trả bảng báo cáo preview cho HR/Admin.

Bước 6: Nếu HR/Admin chọn xuất file, tiến trình 6.2 nhận định dạng Excel hoặc PDF.

Bước 7: Tiến trình 6.2 render file từ dữ liệu đã tổng hợp và trả file download cho người dùng.

Bước 8: HR/Admin hoặc CEO gửi yêu cầu xem Dashboard tổng quan.

Bước 9: Tiến trình 6.3 truy vấn AttendanceRecord để đếm check-in hôm nay, lượt đi trễ và nhân viên đang làm việc.

Bước 10: Tiến trình 6.3 truy vấn FraudDetection để đếm cảnh báo gian lận trong ngày.

Bước 11: Tiến trình 6.3 truy vấn Employee để lấy tổng số nhân viên active.

Bước 12: Tiến trình 6.3 tính tỷ lệ đúng giờ, số nhân viên đang check-in và số cảnh báo gian lận.

Bước 13: Hệ thống trả dashboard KPI và dữ liệu mini-map cho HR/Admin hoặc CEO.

Bước 14: Dashboard tự động refresh mỗi 60 giây để cập nhật số liệu mới nhất.

Bước 15: Kết thúc.
