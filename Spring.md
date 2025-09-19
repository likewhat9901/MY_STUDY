1. Controller 기본 구조
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<String> getUser(@PathVariable Long id) {
        return ResponseEntity.ok("User id = " + id);
    }
}


REST API 작성의 기본 구조
