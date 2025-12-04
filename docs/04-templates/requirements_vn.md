# Yêu Cầu Dự Án - User Management System

## 1) Bạn muốn xây dựng hệ thống này để làm gì? (Mục tiêu sử dụng)
Dự án nhằm xây dựng một hệ thống **User Management System** để quản lý tập trung toàn bộ tài khoản người dùng, bao gồm Admin, Staff và End User.  
Hệ thống phải có khả năng phân quyền rõ ràng theo vai trò, đảm bảo bảo mật và có tính mở rộng trong tương lai (như thêm SSO hoặc tích hợp nhiều hệ thống khác).

## 2) Ai sẽ sử dụng hệ thống? (Người dùng & vai trò)
Hệ thống được sử dụng bởi 3 loại người dùng:
- **Admin** – Quản trị toàn bộ hệ thống.
- **Staff** – Quản lý một phần user.
- **End User** – Quản lý thông tin cá nhân.

## 3) Hệ thống cần những chức năng gì? (Danh sách chức năng)
- Đăng nhập / Đăng xuất / Session Timeout  
- Quên mật khẩu (gửi email reset)  
- Đăng ký tài khoản  
- Danh sách user + tìm kiếm + lọc + phân trang  
- Tạo / sửa / xóa (soft delete) / khóa user  
- Quản lý role & permission  
- Xem log hành động

## 4) Hệ thống cần lưu những thông tin gì của người dùng? (User Data Model)
**Bắt buộc:** email, password, full_name, phone_number, role, status, created_at, updated_at  
**Không bắt buộc:** address, gender, date_of_birth, note

## 5) Các quy tắc và validation
- Email: đúng format, không trùng  
- Password: ≥ 8 ký tự, gồm chữ hoa/thường, số, ký tự đặc biệt  
- Name: ≤ 255 ký tự  
- Phone: ≥ 13 , ≤ 20 ký tự  
- Address: ≤ 255 ký tự  
- Gender: 0 hoặc 1  
- Date_of_birth: YYYY-MM-DD, ≥ 18 tuổi  

## 6) Yêu cầu phi chức năng (NFR)
- Chịu tải 10.000 user  
- Tối thiểu 100 user online  
- UI/UX dễ dùng  
- Backup DB hàng ngày  

## 7) Yêu cầu bảo mật, bàn giao và quy trình
**Bảo mật:** HTTPS, BCrypt/Argon2, giới hạn login fail, log hành động, session timeout  
**Bàn giao:** Source code, hướng dẫn deploy, manual, ERD, tài khoản admin  
**Quy trình:** Analysis → Design → Development → Test/UAT → Deploy → Bảo hành
