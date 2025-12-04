# 📄 Tài liệu yêu cầu dự án  
# **Web quản lý tài khoản người dùng (User Management System)**  
*Phiên bản: 1.1 – Đã bổ sung thông tin địa chỉ, giới tính, ngày sinh*

## 1. Thông tin chung
- **Tên dự án**: User Management System
- **Khách hàng**: *ABC*  
- **Người phụ trách phía khách**: *CusBCD / Manager / dthakushaku@gmail.com / 08041914832*  
- **Thời gian mong muốn**:  
  - Bắt đầu: *2024-12-01*  
  - Go-live: *2025-03-31*  
- **Mục tiêu chính**:
  - Quản lý tập trung dữ liệu tài khoản.
  - Phân quyền rõ ràng theo vai trò.
  - Hỗ trợ mở rộng trong tương lai (SSO, tích hợp nhiều hệ thống).

## 2. Phạm vi (Scope)

### 2.1. Trong phạm vi
- Xây dựng Web quản trị tài khoản (backend + frontend).  
- Đăng nhập / đăng xuất / quên mật khẩu.  
- Danh sách user, tạo–sửa–xóa–khóa user.  
- Quản lý role & phân quyền.  
- Reset mật khẩu qua email.  
- Triển khai môi trường staging & production.  
- Bàn giao tài liệu + source code.

### 2.2. Ngoài phạm vi
- App mobile.  
- SSO Google/Facebook.  
- Tích hợp hệ thống ngoài.

## 3. Loại người dùng (Roles)
| Loại tài khoản | Mô tả |
|----------------|-------|
| **Admin** | Quản trị toàn hệ thống. |
| **Staff** | Quản lý một phần user. |
| **End User** | Quản lý thông tin cá nhân. |

## 4. Chức năng chi tiết
### 4.1. Đăng nhập / Đăng xuất
- Email + mật khẩu.  
- Giới hạn sai password.  
- Session timeout.  
- Đăng xuất.

### 4.2. Quên mật khẩu
- Nhập email → gửi link reset có hạn 30 phút.

### 4.3. Đăng ký tài khoản
- User tự đăng ký (email xác thực) hoặc  
- Admin tạo tài khoản.

### 4.4. Danh sách user
- Tìm kiếm: email, tên, role, trạng thái.  
- Lọc: role, trạng thái.  
- Phân trang.

### 4.5. Tạo / Sửa / Xóa / Khóa user
- **Tạo**: email, password/auto-password, role, trạng thái.  
- **Sửa**: họ tên, số điện thoại, địa chỉ, giới tính, ngày sinh, role, trạng thái.  
- **Xóa**: xóa mềm.  
- **Khóa**: không cho đăng nhập.

### 4.6. Role/Permission
| Chức năng | Admin | Staff | End User |
|----------|:-----:|:-----:|:--------:|
| Xem user | ✔ | ✔ | ✖ |
| Tạo user | ✔ | optional | ✖ |
| Sửa user | ✔ | optional | ✖ |
| Sửa thông tin cá nhân | ✔ | ✔ | ✔ |
| Khóa/Xóa user | ✔ | ✖ | ✖ |
| Quản lý quyền | ✔ | ✖ | ✖ |
| Xem log | ✔ | ✖ | ✖ |

## 5. Thiết kế dữ liệu
### 5.1. Bảng User
| Trường | Bắt buộc | Mô tả |
|--------|:--------:|-------|
| user_id | ✔ | ID nội bộ |
| email | ✔ | Email đăng nhập |
| password | ✔ | Mật khẩu mã hóa |
| full_name | ✔ | Họ tên |
| phone_number | ✖ | SĐT |
| address | ✖ | Địa chỉ |
| gender | ✖ | Giới tính |
| date_of_birth | ✖ | Ngày sinh |
| role | ✔ | admin/staff/user |
| status | ✔ | active/inactive/locked |
| note | ✖ | Ghi chú |
| created_at | ✔ | Ngày tạo |
| updated_at | ✔ | Ngày cập nhật |

## 6. Validation
- Email: đúng format, không trùng.  
- Password: ≥ 8 ký tự, có số + chữ + ký đặc biệt.  
- Name: tối đa 255 ký tự.  
- Address/gender/date_of_birth: không bắt buộc.  
- Phone: tối đa 20 ký tự.
- address: tối đa 255 ký tự.
- gender: chỉ nhận giá trị 1 = Mate, 0 Female.
- date_of_birth: đúng format ngày tháng. Trên 18 tuổi.

## 7. Bảo mật
- HTTPS.  
- BCrypt/Argon2.  
- Giới hạn login fail.  
- Log hành động.  
- Session timeout.

## 8. Phi chức năng
- 10.000 user.  
- 100 user online cùng lúc.  
- Backup hàng ngày.  
- UI dễ dùng.

## 9. Bàn giao
- Source code + hướng dẫn deploy.  
- Manual Admin/Staff/User.  
- ERD chi tiết.  
- Tài khoản Admin.

## 10. Quy trình dự án
1. Phân tích yêu cầu  
2. Thiết kế  
3. Phát triển  
4. Test & UAT  
5. Deploy Production  
6. Bảo hành
