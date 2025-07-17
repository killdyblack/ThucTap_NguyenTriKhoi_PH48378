# Tuần 2: Làm việc với Database (MySQL) & JPA

## Mục tiêu:
- Cài đặt và kết nối cơ sở dữ liệu MySQL.
- Tạo entity User và Task bằng JPA.
- Cấu hình kết nối database trong application.yml.
- Tạo repository, service, controller cho CRUD.
- Kiểm thử API bằng Postman.

---

## 1. Cài đặt & cấu hình MySQL

Tạo database:
```sql
CREATE DATABASE task_management;

Cấu hình trong application.yml:
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
