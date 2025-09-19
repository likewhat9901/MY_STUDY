# Spring 실무 필수 코드 모음

## 1. Controller 기본 구조
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<String> getUser(@PathVariable Long id) {
        return ResponseEntity.ok("User id = " + id);
    }
}
