# 🧪 Tuần 1 – Ôn tập Java & Làm quen Spring Boot

## 🎯 Mục tiêu
- Ôn tập OOP, Collections, và Exception Handling trong Java.
- Cài đặt môi trường lập trình đầy đủ.
- Tạo project Spring Boot cơ bản.
- Viết REST API đầu tiên: "Hello World".
- Nắm mô hình MVC trong Spring Boot.

---

## 1. Ôn tập Java

**🔸 OOP:**
- Sử dụng class, object, kế thừa, đa hình, đóng gói.
- VD: class `User` gồm `id`, `name`, `email`.

**🔸 Collections:**
- Làm việc với `List`, `Map`, `Set`.
- VD: `ArrayList<User>` để quản lý danh sách người dùng.

**🔸 Exception Handling:**
- Dùng `try-catch-finally` để xử lý lỗi.
- VD: chia cho 0, truy cập sai chỉ số mảng.
- Tạo custom exception: `UserNotFoundException`.

---

## 2. Cài đặt công cụ

| Phần mềm       | Mục đích                        |
|----------------|----------------------------------|
| Java JDK 17+   | Chạy ứng dụng Spring Boot        |
| Spring Initializr | Tạo nhanh project Spring Boot |
| Maven / Gradle | Quản lý thư viện                 |
| IntelliJ / Eclipse | IDE viết code                |
| Git            | Quản lý phiên bản mã nguồn       |
| Postman        | Test REST API                    |

---

## 3. Khởi tạo Spring Boot Project

- Truy cập: https://start.spring.io/
- Cấu hình:
  - Group: `com.sakurahino`
  - Artifact: `task-management-internship`
  - Dependencies: `Spring Web`

📁 *Sau khi tạo, mở trong IntelliJ sẽ có cấu trúc thư mục chuẩn của Spring Boot.*

---

## 4. Viết REST API đầu tiên – "Hello World"

**HelloController.java:**
```java
@RestController
public class HelloController {
  @GetMapping("/hello-world")
  public String sayHello() {
    return "Hello World";
  }
}
```

**Test bằng Postman:**
- Method: `GET`
- URL: `http://localhost:8080/api/hello`
- Kết quả: `"Hello World!"`

---

## 5. Hiểu mô hình MVC

| Thành phần | Vai trò               | Ghi chú             |
|------------|------------------------|----------------------|
| Model      | Dữ liệu, Entity, DTO   | Chưa dùng (tuần 2)   |
| View       | Giao diện HTML         | Không dùng           |
| Controller | Nhận request, trả response JSON | Đã dùng (`HelloController`) |

---

## 6. Kết quả đạt được

- ✅ Cài xong môi trường Java + Spring Boot
- ✅ Tạo ứng dụng Spring Boot đầu tiên
- ✅ Viết được REST API đơn giản
- ✅ Hiểu mô hình MVC cơ bản
- ✅ Làm quen cấu trúc thư mục Spring Boot

---

## 7. Ghi chú thêm

**Cấu hình file `application.yml`:**
```yaml
server:
  port: 8080
  servlet:
    context-path: /
```

**Lệnh chạy ứng dụng bằng Maven:**
```bash
mvn spring-boot:run
```

**Build file .jar để chạy:**
```bash
mvn clean package
java -jar target/task-management-internship-0.0.1-SNAPSHOT.jar
```

---

## ✅ Checklist Tuần 1

- [x] Ôn lại OOP, Collections, Exception
- [x] Cài đặt công cụ
- [x] Tạo project Spring Boot
- [x] Viết REST API Hello World
- [x] Hiểu mô hình MVC

---
