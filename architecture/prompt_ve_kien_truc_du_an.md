# Prompt Vẽ Kiến Trúc Dự Án

Hãy vẽ sơ đồ kiến trúc hệ thống cho dự án. Sơ đồ cần trình bày rõ các lớp kiến trúc, luồng dữ liệu chính và công nghệ sử dụng. Dùng tiếng Việt trong toàn bộ nhãn, tiêu đề và chú thích.

Yêu cầu phong cách sơ đồ:

- Vẽ theo dạng kiến trúc hệ thống nhiều lớp, bố cục từ trái sang phải hoặc từ trên xuống dưới.
- Sử dụng các khối rõ ràng, có tiêu đề, icon hoặc màu sắc khác nhau cho từng lớp.
- Không đưa chi tiết quá kỹ thuật như tên bảng database, tên endpoint cụ thể hoặc code.
- Tập trung vào mối quan hệ giữa người dùng, frontend, backend, lớp dữ liệu và dịch vụ ngoài.
- Có mũi tên thể hiện luồng giao tiếp: người dùng -> frontend -> FastAPI backend -> lớp dữ liệu/dịch vụ ngoài -> phản hồi về người dùng.
- Có phần chú thích công nghệ ở cạnh hoặc phía dưới sơ đồ.

Nội dung kiến trúc cần có:

1. Nhóm người dùng

- Nhân viên sử dụng Mobile App để đăng nhập, chấm công vào/ra, xác minh vị trí và khuôn mặt, nhận thông báo, xem lịch sử chấm công.
- HR/Admin/CEO sử dụng Web Dashboard để xem tổng quan, bản đồ 3D, báo cáo, ngoại lệ, audit log và quản trị hệ thống.

2. Lớp Frontend

- Web Dashboard: React với TypeScript.
- Mobile App: React Native với TypeScript.
- Web Dashboard giao tiếp với backend qua HTTPS API.
- Mobile App gửi dữ liệu chấm công gồm vị trí GPS, ảnh selfie và thông tin thiết bị về backend.

3. Lớp Backend

- Backend dùng FastAPI Framework.
- Bên trong backend thể hiện các module chính: Authentication, Attendance, Geofence, Fraud Detection, Face Verification, Notification, Report, Audit Log, Admin Management.
- Backend xử lý xác thực, phân quyền, chấm công, kiểm tra thiết bị, kiểm tra gian lận, xử lý ngoại lệ, tổng hợp báo cáo và ghi nhật ký.

4. Lớp dữ liệu và dịch vụ ngoài

- ArcGIS: hiển thị và tương tác với bản đồ GIS 3D, tòa nhà, tầng, vùng geofence và marker nhân viên.
- PostgreSQL trên Supabase: lưu cả dữ liệu nghiệp vụ và dữ liệu GIS/3D như nhân viên, tài khoản, phòng ban, thiết bị, ca làm, bản ghi chấm công, tòa nhà, tầng, metadata mô hình 3D, lớp bản đồ, geofence, ngoại lệ và audit log.
- Redis: cache dữ liệu, lưu trạng thái ngắn hạn, hỗ trợ tác vụ nền và hàng đợi xử lý.
- MinIO: lưu ảnh selfie, file báo cáo Excel/PDF và các file phát sinh từ hệ thống.

5. Luồng chính cần thể hiện trên sơ đồ

- Luồng chấm công: Nhân viên -> Mobile App -> FastAPI Backend -> Fraud Detection/Face Verification/Geofence -> Supabase PostgreSQL/PostGIS -> phản hồi trạng thái về Mobile App.
- Luồng giám sát: HR/Admin/CEO -> Web Dashboard -> FastAPI Backend -> Supabase PostgreSQL/PostGIS -> ArcGIS hiển thị bản đồ 3D -> Web Dashboard hiển thị dashboard/bản đồ/báo cáo.
- Luồng lưu file: Mobile App hoặc Web Dashboard -> FastAPI Backend -> MinIO.
- Luồng cache/tác vụ nền: FastAPI Backend -> Redis -> xử lý thông báo, báo cáo hoặc dữ liệu cập nhật nhanh.

Ngoài sơ đồ kiến trúc, hãy trình bày thêm phần "3.1 Công nghệ sử dụng" bên dưới sơ đồ với nội dung ngắn gọn theo đúng cấu trúc sau:

## 3.1.1 Backend

### FastAPI Framework

Khái niệm: FastAPI là framework backend Python dùng để xây dựng REST API hiệu năng cao, hỗ trợ kiểu dữ liệu với Pydantic và tự động sinh tài liệu OpenAPI/Swagger.

Ưu điểm:

- Hiệu năng tốt và phù hợp với hệ thống API.
- Cú pháp rõ ràng, dễ tổ chức module nghiệp vụ.
- Tự động sinh tài liệu API giúp frontend/mobile dễ tích hợp.
- Dùng chung hệ sinh thái Python cho xử lý ảnh, phát hiện gian lận và báo cáo.

Nhược điểm:

- Cần thiết kế cấu trúc project tốt khi hệ thống lớn.
- Một số tác vụ nền cần kết hợp thêm Redis, Celery hoặc RQ.
- Việc tối ưu xử lý đồng thời và bảo mật vẫn cần cấu hình cẩn thận.

## 3.1.2 Frontend

### React với TypeScript

Khái niệm: React là thư viện JavaScript dùng để xây dựng giao diện web theo component. TypeScript bổ sung kiểu dữ liệu tĩnh giúp giảm lỗi trong quá trình phát triển.

Ưu điểm:

- Phù hợp xây dựng Web Dashboard có nhiều bảng dữ liệu, biểu đồ và form quản trị.
- Component dễ tái sử dụng và bảo trì.
- TypeScript giúp code rõ ràng, dễ kiểm soát dữ liệu.
- Hệ sinh thái thư viện phong phú.

Nhược điểm:

- Cần quản lý tốt cấu trúc component khi dashboard nhiều màn hình.
- Cần chọn thư viện UI, routing và state management phù hợp.
- Có thể phức tạp với người mới nếu kết hợp nhiều thư viện.

### React Native với TypeScript

Khái niệm: React Native là framework xây dựng ứng dụng di động bằng JavaScript/TypeScript, cho phép phát triển app Android và iOS từ một codebase chung.

Ưu điểm:

- Phát triển được ứng dụng mobile đa nền tảng.
- Tận dụng kiến thức React và TypeScript từ frontend web.
- Phù hợp với các chức năng mobile như đăng nhập, chấm công, camera, định vị và thông báo.
- Giảm thời gian phát triển so với viết native riêng cho Android/iOS.

Nhược điểm:

- Một số tính năng liên quan GPS, camera, thiết bị có thể cần native module.
- Cần kiểm thử kỹ trên nhiều thiết bị thật.
- Hiệu năng có thể cần tối ưu nếu xử lý nhiều tác vụ nặng trên thiết bị.

## 3.1.3 Lớp dữ liệu

### ArcGIS

Khái niệm: ArcGIS là nền tảng GIS dùng để hiển thị, tương tác và phân tích dữ liệu bản đồ, bao gồm bản đồ 2D/3D, tòa nhà, tầng và dữ liệu không gian. Trong hệ thống này, dữ liệu nghiệp vụ và metadata mô hình 3D được lưu ở Supabase PostgreSQL, còn ArcGIS chủ yếu phục vụ hiển thị và tương tác bản đồ 3D.

Ưu điểm:

- Hỗ trợ bản đồ GIS 3D và Scene View phù hợp với bài toán tòa nhà nhiều tầng.
- Dễ hiển thị marker nhân viên, vùng geofence và lớp bản đồ.
- Có hệ sinh thái mạnh cho dữ liệu không gian.
- Phù hợp với yêu cầu trực quan hóa vị trí theo thời gian gần thực.

Nhược điểm:

- Có thể phát sinh chi phí bản quyền hoặc giới hạn theo gói sử dụng.
- Cần cấu hình layer, dữ liệu tòa nhà và quyền truy cập chính xác.
- Phụ thuộc vào dịch vụ bên ngoài khi hiển thị bản đồ.

### PostgreSQL (Supabase)

Khái niệm: PostgreSQL là hệ quản trị cơ sở dữ liệu quan hệ mạnh và ổn định. Supabase cung cấp PostgreSQL dưới dạng dịch vụ backend được quản lý. Trong hệ thống này, Supabase PostgreSQL lưu cả dữ liệu nghiệp vụ và dữ liệu GIS/3D như tòa nhà, tầng, metadata mô hình 3D, lớp bản đồ và geofence.

Ưu điểm:

- Phù hợp lưu dữ liệu nghiệp vụ có quan hệ như nhân viên, phòng ban, tài khoản, ca làm và chấm công.
- Lưu tập trung dữ liệu GIS/3D như tòa nhà, tầng, geofence, lớp bản đồ và metadata mô hình 3D.
- Ổn định, phổ biến và hỗ trợ truy vấn phức tạp.
- Supabase giúp triển khai nhanh, dễ quản lý database.
- Có thể mở rộng với PostGIS để hỗ trợ dữ liệu không gian.

Nhược điểm:

- Cần thiết kế schema và index tốt để truy vấn nhanh.
- Dữ liệu vị trí lớn có thể cần tối ưu lưu trữ và phân vùng.
- Khi dùng Supabase cần lưu ý giới hạn gói dịch vụ và bảo mật quyền truy cập.

### Redis

Khái niệm: Redis là hệ thống lưu trữ key-value trong bộ nhớ, thường dùng cho cache, queue, session và dữ liệu tạm thời cần truy xuất nhanh.

Ưu điểm:

- Truy xuất rất nhanh, phù hợp cache dữ liệu dashboard hoặc trạng thái ngắn hạn.
- Hỗ trợ hàng đợi tác vụ nền khi kết hợp Celery/RQ.
- Giảm tải cho PostgreSQL trong các truy vấn lặp lại.
- Phù hợp cho dữ liệu cập nhật gần thời gian thực.

Nhược điểm:

- Dữ liệu nằm chủ yếu trong RAM nên cần quản lý dung lượng.
- Không thay thế database chính.
- Cần cấu hình persistence và backup nếu lưu dữ liệu quan trọng.

### MinIO

Khái niệm: MinIO là hệ thống object storage tương thích S3, dùng để lưu file như ảnh selfie, file báo cáo và tài liệu sinh ra từ hệ thống.

Ưu điểm:

- Tự triển khai được, không phụ thuộc hoàn toàn vào cloud provider.
- Tương thích API S3, dễ tích hợp backend.
- Phù hợp lưu file lớn, ảnh và tài liệu xuất báo cáo.
- Dễ tách file khỏi database để giảm tải hệ quản trị dữ liệu.

Nhược điểm:

- Cần tự quản lý hạ tầng, dung lượng và backup nếu tự host.
- Cần cấu hình quyền truy cập file, chính sách bảo mật và lifecycle.
- Không phù hợp để lưu dữ liệu quan hệ hoặc truy vấn nghiệp vụ phức tạp.

Yêu cầu đầu ra:

- Tạo một sơ đồ kiến trúc rõ ràng, dễ đưa vào báo cáo đồ án.
- hãy dùng phong cách clean architecture, màu sắc chuyên nghiệp, dễ đọc khi chèn vào Word.
