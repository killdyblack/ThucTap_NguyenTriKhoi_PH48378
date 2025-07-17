# 🧪 Tuần 8 – Viết Unit Test & Integration Test

## 🎯 Mục tiêu

- Viết test cho `Service`, `Controller`.
- Dùng thư viện `JUnit` + `Mockito`.
- Cấu hình môi trường `test` với `H2 Database`.
- Đảm bảo test logic nghiệp vụ, xác thực dữ liệu và phản hồi API đúng.

---

## ⚙️ 1. Cấu hình `application-test.yml`

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    driver-class-name: org.h2.Driver
    username: sa
    password:
  h2:
    console:
      enabled: true
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: true
    properties:
      hibernate:
        format_sql: true
  main:
    allow-bean-definition-overriding: true

logging:
  level:
    root: WARN
    org.springframework.web: DEBUG
    org.hibernate.SQL: DEBUG

springdoc:
  api-docs:
    enabled: false
  swagger-ui:
    enabled: false
  show-actuator: false
```

---

## 🧪 2. Unit Test – `AuthServiceImplTest.java`
📁 [AuthServiceImplTest.java](../src/test/java/com/taskmanagement/service/impl/AuthServiceImplTest.java)
```java
class AuthServiceImplTest {

    @InjectMocks
    private AuthServiceImpl authService;

    @Mock private UserRepository userRepository;
    @Mock private PasswordEncoder passwordEncoder;
    @Mock private UserMapper userMapper;
    @Mock private JwtTokenProvider jwtTokenProvider;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void testRegister_Success() {
        RegisterRequest request = new RegisterRequest("khoint3", "123456", "Nguyen Tri Khoi", "USER");
        when(userRepository.findByUsername("hung123")).thenReturn(Optional.empty());
        when(passwordEncoder.encode("123456")).thenReturn("encoded-password");

        UUID userId = UUID.fromString("550e8400-e29b-41d4-a716-446655440000");
        User savedUser = new User();
        savedUser.setId(userId.toString());
        savedUser.setUsername("khoint3");
        savedUser.setFullName("Nguyen Tri Khoi");
        savedUser.setPassword("encoded-password");
        savedUser.setCreated(Instant.parse("2024-01-01T00:00:00Z"));
        savedUser.setRole(Role.USER);

        when(userRepository.save(any(User.class))).thenReturn(savedUser);
        when(userMapper.toUserResponseDTO(any(User.class)))
            .thenReturn(new UserResponseDTO(userId.toString(), "khoint3", "Nguyen Tri Khoi", null, Instant.parse("2024-01-01T00:00:00Z")));

        UserResponseDTO result = authService.register(request);
        assertEquals("khoint3", result.getUsername());
    }

    @Test
    void testRegister_UsernameExists() {
        when(userRepository.findByUsername("khoint3")).thenReturn(Optional.of(new User()));
        AppException ex = assertThrows(AppException.class, () -> authService.register(
                new RegisterRequest("khoint3", "123456", "Khoi", "USER")));
        assertEquals(ExceptionCode.USER_FOUND, ex.getCode());
    }

    @Test
    void testLogin_Success() {
        User user = new User();
        user.setUsername("khoint3");
        user.setPassword("encoded-password");

        when(userRepository.findByUsername("khoint3")).thenReturn(Optional.of(user));
        when(passwordEncoder.matches("mypassword", "encoded-password")).thenReturn(true);
        when(jwtTokenProvider.generateToken(any())).thenReturn("mocked-jwt");

        LoginResponse response = authService.login(new LoginRequest("khoint3", "mypassword"));
        assertEquals("mocked-jwt", response.getToken());
    }

    @Test
    void testLogin_UserNotFound() {
        when(userRepository.findByUsername("notfound")).thenReturn(Optional.empty());

        AppException ex = assertThrows(AppException.class, () ->
                authService.login(new LoginRequest("notfound", "pass")));
        assertEquals(ExceptionCode.USER_NOT_FOUND, ex.getCode());
    }

    @Test
    void testLogin_IncorrectPassword() {
        User user = new User();
        user.setUsername("khoint3");
        user.setPassword("encoded");

        when(userRepository.findByUsername("khoint3")).thenReturn(Optional.of(user));
        when(passwordEncoder.matches("wrongpass", "encoded")).thenReturn(false);

        AppException ex = assertThrows(AppException.class, () ->
                authService.login(new LoginRequest("khoint3", "wrongpass")));
        assertEquals(ExceptionCode.PASSWORD_INCORRECT, ex.getCode());
    }
}
```

---

## 🧪 3. Unit Test – `UserServiceImplTest.java`
📁 [UserServiceImplTest.java](../src/test/java/com/taskmanagement/service/impl/UserServiceImplTest.java)
```java
@Test
void testCreate_Success() {
    UserRequestDTO request = new UserRequestDTO("user", "Password1", "Full Name", "USER");
    when(userRepository.findByUsername("user")).thenReturn(Optional.empty());
    when(passwordEncoder.encode("Password1")).thenReturn("encoded");
    when(userRepository.save(any())).thenReturn(new User());
    when(userMapper.toUserResponseDTO(any()))
        .thenReturn(new UserResponseDTO("uuid", "user", "Full Name", Role.USER, Instant.now()));

    UserResponseDTO result = userService.create(request);
    assertEquals("user", result.getUsername());
}
```

---

## 🧪 4. Integration Test – `AdminUserControllerTest.java`
📁 [AdminUserControllerTest.java](../src/test/java/com/taskmanagement/controller/admin/AdminUserControllerTest.java)
```java
@SpringBootTest
@AutoConfigureMockMvc
class AdminUserControllerTest {

    @Autowired private MockMvc mockMvc;
    @MockBean private UserService userService;

    @Test
    void testGetAllUsers() throws Exception {
        List<UserResponseDTO> users = List.of(new UserResponseDTO("1", "admin", "Admin", Role.ADMIN, Instant.now()));
        when(userService.findAll(anyInt(), anyInt())).thenReturn(new PaginatedResponse<>(users, 1, 1));

        mockMvc.perform(get("/api/admin/users")
                .header("Authorization", "Bearer mocked-token"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.data.content[0].username").value("admin"));
    }
}
```

---

## 🧪 5. Integration Test – `AuthControllerTest.java`
📁 [AuthControllerTest.java](../src/test/java/com/taskmanagement/controller/AuthControllerTest.java)
```java
@SpringBootTest
@AutoConfigureMockMvc
class AuthControllerTest {

    @Autowired private MockMvc mockMvc;
    @MockBean private AuthService authService;

    @Test
    void testLoginSuccess() throws Exception {
        when(authService.login(any()))
                .thenReturn(new LoginResponse("jwt-token"));

        mockMvc.perform(post("/api/auth/login")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content("{\"username\": \"khoint3\", \"password\": \"password\"}"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.data.token").value("jwt-token"));
    }
}
```

---

## 🧪 6. Integration Test – `TaskAdminControllerTest.java`
📁 [TaskAdminControllerTest.java](../src/test/java/com/taskmanagement/controller/admin/TaskAdminControllerTest.java)
```java
@SpringBootTest
@AutoConfigureMockMvc
class TaskAdminControllerTest {

    @Autowired private MockMvc mockMvc;
    @MockBean private TaskService taskService;

    @Test
    void testGetAllTasksByAdmin() throws Exception {
        when(taskService.findAll(anyInt(), anyInt()))
            .thenReturn(new PaginatedResponse<>(new ArrayList<>(), 0, 1));

        mockMvc.perform(get("/api/admin/tasks")
                .header("Authorization", "Bearer mocked-token"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.data.content").isArray());
    }
}
```

---

## ✅ Tổng kết

| Thành phần            | Trạng thái          |
|----------------------|---------------------|
| AuthServiceImpl      | ✅ Unit test         |
| UserServiceImpl      | ✅ Unit test         |
| AuthController       | ✅ Integration test  |
| AdminUserController  | ✅ Integration test  |
| TaskAdminController  | ✅ Integration test  |
| application-test.yml | ✅ Cấu hình H2, MockMvc |
