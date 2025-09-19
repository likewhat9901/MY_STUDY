## Spring 실무 필수 코드 모음

### 1. Controller 기본 구조
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<String> getUser(@PathVariable Long id) {
        return ResponseEntity.ok("User id = " + id);
    }
}
```

* **이유**: REST API 작성의 기본. `@RestController` + `@RequestMapping` 조합은 필수.

---

### 2. DTO (Data Transfer Object) 사용

```java
public record UserDto(Long id, String name, String email) {}
```

* **이유**: Entity를 그대로 노출하지 않고, API 응답/요청 포맷을 관리.

---

### 3. Service Layer 분리

```java
@Service
public class UserService {
    public String findUser(Long id) {
        return "User_" + id;
    }
}
```

* **이유**: Controller에서 비즈니스 로직을 분리 → 유지보수성 향상.

---

### 4. Repository + JPA 기본

```java
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
}
```

* **이유**: Spring Data JPA의 핵심. CRUD 자동화.

---

### 5. Entity 매핑

```java
@Entity
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String email;
}
```

* **이유**: DB와 객체 매핑의 기본. `@Id`, `@GeneratedValue`는 반드시 숙지.

---

### 6. 예외 처리 (Global Exception Handler)

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<String> handleException(RuntimeException ex) {
        return ResponseEntity.badRequest().body(ex.getMessage());
    }
}
```

* **이유**: 서비스 전체의 에러 응답 통일.

---

### 7. Validation (유효성 검증)

```java
public record UserRequest(
    @NotBlank String name,
    @Email String email
) {}
```

```java
@PostMapping
public ResponseEntity<String> createUser(@Valid @RequestBody UserRequest req) {
    return ResponseEntity.ok("Created: " + req.email());
}
```

* **이유**: 사용자 입력값 검증 필수.

---

### 8. ResponseEntity 활용

```java
return ResponseEntity.status(HttpStatus.CREATED).body(userDto);
```

* **이유**: HTTP 상태코드 + body 동시 관리.

---

### 9. 로그 출력 (Slf4j)

```java
@Slf4j
@Service
public class UserService {
    public void doSomething() {
        log.info("UserService 실행");
    }
}
```

* **이유**: 실무에서 로그는 필수. `System.out.println` 대신 `log`.

---

### 10. Lombok 활용

```java
@Getter @Setter
@NoArgsConstructor @AllArgsConstructor
@Builder
public class User {
    private Long id;
    private String name;
}
```

* **이유**: 반복 코드 줄이기. 실무 필수 라이브러리.

---

### 11. @Transactional

```java
@Transactional
public void updateUser(Long id, String name) {
    User user = userRepository.findById(id).orElseThrow();
    user.setName(name);
}
```

* **이유**: DB 작업 시 원자성 보장.

---

### 12. QueryDSL / JPQL 기본

```java
@Query("select u from User u where u.email = :email")
Optional<User> findByEmail(@Param("email") String email);
```

* **이유**: 복잡한 쿼리 작성.

---

### 13. FeignClient (외부 API 호출)

```java
@FeignClient(name = "other-service", url = "http://localhost:8081")
public interface OtherServiceClient {
    @GetMapping("/api/data")
    String getData();
}
```

* **이유**: 마이크로서비스 구조에서 다른 서버 호출.

---

### 14. RestTemplate (간단 HTTP 호출)

```java
RestTemplate restTemplate = new RestTemplate();
String res = restTemplate.getForObject("https://api.example.com/data", String.class);
```

* **이유**: 외부 API 연동 시 기본.

---

### 15. WebClient (비동기 호출)

```java
WebClient webClient = WebClient.create("https://api.example.com");
String res = webClient.get().uri("/data").retrieve().bodyToMono(String.class).block();
```

* **이유**: 비동기/리액티브 환경에서 필수.

---

### 16. CORS 설정

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**").allowedOrigins("*");
    }
}
```

* **이유**: 프론트엔드-백엔드 통신 허용.

---

### 17. Scheduler (배치 작업)

```java
@EnableScheduling
@Configuration
public class SchedulerConfig {
    @Scheduled(cron = "0 0 * * * ?")
    public void runTask() {
        System.out.println("매시간 실행");
    }
}
```

* **이유**: 자동화 작업 필수.

---

### 18. AOP (로깅, 트랜잭션)

```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore() {
        System.out.println("메서드 실행 전");
    }
}
```

* **이유**: 횡단 관심사 처리.

---

### 19. 캐시 적용

```java
@Cacheable("users")
public User getUser(Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

* **이유**: 성능 최적화.

---

### 20. 파일 업로드

```java
@PostMapping("/upload")
public String upload(@RequestParam MultipartFile file) throws IOException {
    file.transferTo(new File("uploads/" + file.getOriginalFilename()));
    return "ok";
}
```

* **이유**: 실무에서 파일 업로드는 매우 흔함.

---

### 21. Security 기본 설정

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests()
            .anyRequest().authenticated()
            .and().httpBasic();
    }
}
```

* **이유**: 보안 기본기.

---

# ✅ 정리

* **Controller/Service/Repository 기본기** (1\~5)
* **예외, 검증, 응답 통일** (6\~8)
* **로그, Lombok, 트랜잭션** (9\~11)
* **DB 쿼리, API 호출** (12\~15)
* **CORS, Scheduler, AOP** (16\~18)
* **캐시, 파일 업로드, 보안** (19\~21)

그 방식 원하시나요? (think about it step-by-step)

