# ERD Review – Hệ thống Chấm công Thông minh (Nhóm 9)

## Tổng quan

ERD hiện tại gồm 11 thực thể, phân thành 3 nhóm: Tổ chức, GIS, và Vận hành. Cấu trúc cơ bản hợp lý cho POC/MVP.

---

## Vấn đề phát hiện & Điều chỉnh

### 1. Thiếu cơ chế gán ca làm việc (Shift Assignment) ⚠️ Cần bổ sung

**Vấn đề:** `Shift` tồn tại nhưng không có bảng/cột nào liên kết ca làm với nhân viên hoặc phòng ban. `AttendanceRecord.shift_id` ghi nhận ca của một lần chấm công, nhưng hệ thống không có cách xác định ca nào áp dụng cho nhân viên khi họ check-in.

**Điều chỉnh (MVP – tối giản):** Thêm cột `default_shift_id` vào bảng `Employee`:

```sql
ALTER TABLE Employee ADD COLUMN default_shift_id INT REFERENCES Shift(shift_id);
```

Khi nhân viên check-in, server đọc `employee.default_shift_id` để điền vào `AttendanceRecord.shift_id` và tính `is_late`.

> Không cần bảng junction `EmployeeShift` ở mức POC – một ca mặc định/nhân viên là đủ.

---

### 2. Circular FK: Department ↔ Employee ⚠️ Lưu ý khi migrate

**Vấn đề:** `Department.manager_id → Employee` và `Employee.department_id → Department` tạo vòng tròn FK.

**Không cần thay đổi schema**, nhưng khi tạo bảng:
- Tạo `Department` trước với `manager_id NULL`
- Tạo `Employee`, sau đó `UPDATE Department SET manager_id = ...`

Đây là pattern chuẩn (self-referential hierarchy), không phải lỗi thiết kế.

---

### 3. AttendanceRecord thiếu liên kết ngày công (Work Session) — Bỏ qua cho POC

**Vấn đề tiềm ẩn:** Check-in và Check-out là 2 bản ghi riêng. Để tính tổng giờ làm, cần query pair check_in/check_out theo employee + ngày + shift. Không có FK nối cặp này.

**Quyết định:** Giữ nguyên cho POC. Query bằng `WHERE employee_id = ? AND DATE(timestamp) = ? AND type IN ('check_in','check_out')` là đủ.

---

### 4. FraudDetection.record_id là UNIQUE FK — Hợp lý ✅

Mỗi lần chấm công chỉ có một kết quả fraud check. Quan hệ 1-1 là đúng.

---

### 5. AuditLog.target_id là BIGINT không có FK — Chủ ý ✅

AuditLog ghi nhận mọi thực thể (polymorphic). Không có FK là đúng – dùng `(target_entity, target_id)` để tra cứu thủ công.

---

## ERD sau điều chỉnh (chỉ thay đổi duy nhất)

```
Employee
  + default_shift_id  INT  FK → Shift  NULL
```

Tất cả các thực thể và quan hệ còn lại giữ nguyên như thiết kế gốc.

---

## Sơ đồ quan hệ tóm tắt (văn bản)

```
Department (1) ──< (N) Employee >── (1) Shift [qua default_shift_id]
Employee (1) ──< (N) Account
Employee (1) ──< (N) Device
Building (1) ──< (N) Floor ──< (N) Geofence
AttendanceRecord >── Employee, Device, Geofence, Shift
AttendanceRecord (1) ── (1) FraudDetection
AuditLog >── Account
```
