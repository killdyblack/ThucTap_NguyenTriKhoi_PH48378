# 🗃️ Tuần 2 – Làm việc với Database (MySQL) & JPA

## 🎯 Mục tiêu
- Cài đặt và kết nối cơ sở dữ liệu MySQL.
- Tạo entity `User` và `Task` bằng JPA.
- Cấu hình kết nối database trong `application.yml`.
- Tạo repository, service, controller cho chức năng CRUD.
- Kiểm thử API bằng Postman.

---

## 1. Cài đặt & cấu hình MySQL

**Tạo database trong MySQL:**
```sql
CREATE DATABASE task_management;
```

**Cấu hình `application.yml`:**
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/task_management
    username: root
    password: your_password
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
```

---

## 2. Entity `User` với JPA

```java
@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class User {

    @Id
    @Column(name = "id", columnDefinition = "VARCHAR(36)")
    private String id;

    @Column(name = "username")
    private String username;

    @Column(name = "password")
    private String password;

    @Column(name = "full_name")
    private String fullName;

    @Column(name = "created")
    private Instant created;

    @Enumerated(EnumType.STRING)
    @Column(name = "role")
    private Role role;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Task> tasks;

    @PrePersist
    private void prePersist() {
        if (id == null) {
            id = UUID.randomUUID().toString();
        }
    }
}
```

---

📌 **Gợi ý tiếp theo:**
- Tạo `Task` entity tương tự và thiết lập liên kết với `User`.
- Tạo `UserRepository`, `UserService`, `UserController`.
- Viết các API: GET, POST, PUT, DELETE cho người dùng.
- Kiểm thử các API bằng Postman.

---

✅ **Checklist Tuần 2**
- [x] Tạo database MySQL
- [x] Cấu hình kết nối trong Spring Boot
- [x] Tạo entity `User`
- [x] Kết nối thành công với JPA
- [ ] Tạo full CRUD cho `User`
- [ ] Test bằng Postman

---
