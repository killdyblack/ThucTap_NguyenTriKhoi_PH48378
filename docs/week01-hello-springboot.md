# Tuần 1 – Ôn tập Java & Làm quen Spring Boot

Mục tiêu:
- Ôn tập kiến thức nền tảng Java: OOP, Collections, và Exception Handling.
- Cài đặt đầy đủ môi trường lập trình.
- Tạo ứng dụng Spring Boot cơ bản với cấu trúc chuẩn.
- Viết REST API đầu tiên: "Hello World".
- Hiểu mô hình MVC trong Spring Boot.

1. Ôn tập Java

OOP – Lập trình hướng đối tượng:
- Sử dụng class, object, kế thừa, đa hình, đóng gói.
- Ví dụ: Tạo class User với thuộc tính id, name, email.

Java Collections:
- Dùng List, Map, Set để lưu trữ dữ liệu linh hoạt.
- Áp dụng ArrayList<User> để quản lý danh sách người dùng.

Exception Handling:
- Sử dụng try-catch-finally để xử lý ngoại lệ.
- Ví dụ: chia cho 0, truy cập mảng sai chỉ số.
- Tạo custom exception đơn giản: UserNotFoundException.

2. Cài đặt công cụ

- Java JDK 17+: Biên dịch và chạy Spring Boot
- Spring Initializr: Khởi tạo project nhanh chóng
- Maven: Quản lý dependency
- IntelliJ IDEA: IDE viết code
- Git: Quản lý phiên bản
- Postman: Kiểm thử REST API

3. Tạo project Spring Boot cơ bản

Dùng https://start.spring.io/

Group: com.sakurahino
Artifact: task-management-internship

Dependencies: Spring Web

4. Viết REST API đầu tiên – “Hello World”

File HelloController.java:

```java
package com.sakurahino.taskmanagementinternship.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello-world")
    public String sayHello() {
        return "Hello World";
    }
}


Kiểm thử API:

Method: GET
URL: http://localhost:8080/hello-world
Response:
"Hello World"

Ví dụ mở rộng (trả về JSON):
@GetMapping("/hello-object")
public Map<String, String> helloObject() {
    return Map.of("message", "Hello World", "status", "success");
}
Response:
{
  "message": "Hello World",
  "status": "success"
}
Mô hình MVC trong Spring Boot:

Model: chứa dữ liệu, entity, DTO (chưa dùng – sẽ dùng từ tuần 2)

View: giao diện, HTML, Thymeleaf (chưa dùng)

Controller: xử lý request/response – HelloController.java

Kết quả đạt được

Cài đặt thành công môi trường Java + Spring Boot

Tạo được project cơ bản

Viết được REST API Hello World

Làm quen mô hình MVC

Biết dùng Postman kiểm thử API

Ghi chú thêm

File application.yml:
server:
  port: 8080
  servlet:
    context-path: /

mvn spring-boot:run
mvn clean package
java -jar target/task-management-internship-0.0.1-SNAPSHOT.jar

Tự đánh giá Tuần 1:

Ôn lại OOP, Collection, Exception: Đã hoàn thành

Cài đặt môi trường: Đã hoàn thành

Tạo project Spring Boot: Đã hoàn thành

Viết API Hello World: Đã hoàn thành

Hiểu mô hình MVC: Đã hoàn thành