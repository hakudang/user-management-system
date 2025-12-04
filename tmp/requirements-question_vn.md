# 1. Làm hệ thống này để làm gì?

- Quản lý tài khoản cho nội bộ? cho khách hàng? cho nhiều hệ thống khác?

- Ví dụ: “Quản lý user dùng chung cho các web/app của công ty, có phân quyền.”

# Ai sẽ dùng hệ thống? (Role)

- Admin hệ thống

- Nhân viên CSKH

- Người dùng cuối (user thường)
Mỗi loại được phép / không được phép làm gì?

# Cần những màn hình / chức năng gì?
Ví dụ:

- Đăng nhập / đăng xuất / quên mật khẩu

- Đăng ký tài khoản (hoặc chỉ admin tạo?)

- Danh sách user, tìm kiếm, lọc
- Tạo / sửa / khóa / xóa user

- Gán quyền (role) / nhóm quyền

- Nhật ký thao tác (log)

# Cần quản lý những thông tin nào của user?

- Email, password, họ tên, số điện thoại, vai trò, trạng thái (active/inactive), ngày tạo, ngày cập nhật…

- Trường nào bắt buộc? Trường nào optional?

# Quy tắc & ràng buộc quan trọng?

- Password dài bao nhiêu ký tự, bắt buộc chữ hoa/thường/số/ký tự đặc biệt?

- Một email có được dùng cho nhiều tài khoản không?

- Khi khóa user, user có đăng nhập được nữa không?

- Có cần xác thực email (verification) không?

# Tích hợp với hệ thống khác không?

- Ví dụ: sau này dùng chung account với website khác, dùng SSO, hay dùng Google Login…

- Nếu chưa rõ, cứ ghi “để open, tương lai có thể mở rộng”.

# Các yêu cầu phi chức năng (NFR)

- Bảo mật: SSL/HTTPS, mã hóa password (BCrypt…), giới hạn login sai, log lại thao tác…

- Hiệu năng: bao nhiêu user dự kiến? cùng lúc bao nhiêu?

- Bàn giao: tài liệu hướng dẫn, source code, tài khoản admin, sơ đồ DB, môi trường staging/production,…