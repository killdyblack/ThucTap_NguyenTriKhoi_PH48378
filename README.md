# 📌 Task Management Project – Spring Boot

## 👨‍💻 Sinh viên thực hiện: Nguyễn Trí Khôi – PH48378  
## 📝 Báo cáo Thực tập Cuối kỳ

---

## 📚 Nội dung thực tập theo tuần

| Tuần | Chủ đề | Tài liệu |
|------|--------|----------|
| Tuần 1 | Ôn tập Java & Làm quen Spring Boot | [week01-hello-springboot.md](./docs/Tuan1.md) |
| Tuần 2 | Làm việc với Database (MySQL) & JPA | [week02-database-jpa.md](./docs/Tuan2-csdl-jpa.md) |
| Tuần 3 | Đăng ký & Đăng nhập (JWT) | [week03-auth-jwt.md](./docs/Tuan3-auth.md) |
| Tuần 4 | RESTful API Quản lý công việc | [week04-task-api.md](./docs/Tuan4-api.md) |
| Tuần 5 | Quản lý người dùng & phân quyền | [week05-user-role.md](./docs/Tuan5-user.md) |
| Tuần 6 | Logging, Xử lý ngoại lệ, Validation | [week06-logging-validation.md](./docs/Tuan6-login.md) |
| Tuần 7 | Pagination, Search, Sort | [week07-pagination-filtering.md](./docs/Tuan7-phantrang.md) |
| Tuần 8 | Unit Test & Integration Test | [week08-testing.md](./docs/Tuan8-unitTess.md) |
| Tuần 9 | Triển khai với Docker | [week09-docker.md](./docs/Tuan9-docket.md) |
| Tuần 10 | Tổng kết, hoàn thiện tài liệu | [week10-report.md](./docs/Tuan10-baocao.md) |

---

🛠️ Công nghệ sử dụng
Java 17

Spring Boot

Spring Data JPA

Spring Security + JWT

MySQL

Maven

Docker

JUnit 5

📌 Chức năng chính
Đăng ký, đăng nhập

Tạo, sửa, xóa, xem danh sách công việc

Phân quyền người dùng (Admin, User)

Tìm kiếm, phân trang, sắp xếp

API RESTful chuẩn hóa

Triển khai bằng Docker

## 🚀 Hướng dẫn chạy dự án

# BƯỚC 1: Cài đặt Docker (nếu máy bạn chưa có)
# Tải và cài đặt tại: https://www.docker.com/products/docker-desktop/

# BƯỚC 2: Mở Terminal và clone source code từ GitHub
git clone https://github.com/killdyblack/ThucTap_NguyenTriKhoi_PH48378.git

# BƯỚC 3: Di chuyển vào thư mục dự án
cd ThucTap_NguyenTriKhoi_PH48378

# BƯỚC 4: Build Docker image (ví dụ đặt tên là thuctap-app)
docker build -t thuctap-app .

# BƯỚC 5: Chạy Docker container từ image vừa build
docker run -p 8080:8080 thuctap-app

# => Ứng dụng sẽ chạy tại: http://localhost:8080


 