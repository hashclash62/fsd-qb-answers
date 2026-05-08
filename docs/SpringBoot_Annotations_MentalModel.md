# Spring Boot Annotations — Complete Mental Model
### For T.Y. B.Tech IT | Full Stack Development (Units 1–3)

---

## 🧠 The Big Picture — How to Think About Annotations

> **Pattern to remember:** Annotations in Spring Boot are like **job titles + instructions** given to classes and methods. Spring Boot reads them at startup and *wires everything together automatically*.

Think of your application as a **company**:

```
┌─────────────────────────────────────────────────────────┐
│                   SPRING BOOT APP = COMPANY             │
│                                                         │
│  CLIENT (Browser/Postman)                               │
│       │                                                 │
│       ▼                                                 │
│  @RestController  ◄── "Receptionist" (handles requests) │
│       │                                                 │
│       ▼                                                 │
│  @Service         ◄── "Manager" (business logic)        │
│       │                                                 │
│       ▼                                                 │
│  @Repository      ◄── "Archivist" (talks to database)   │
│       │                                                 │
│       ▼                                                 │
│  DATABASE (MySQL / MongoDB)                             │
└─────────────────────────────────────────────────────────┘
```

---

## 📦 GROUP 1: Stereotype Annotations (WHO are you?)

> **Pattern:** These tell Spring *what role* a class plays. Spring then creates and manages them as **beans** (managed objects).

| Annotation | Role | File |
|---|---|---|
| `@Component` | Generic managed bean | Any class |
| `@Controller` | Handles HTTP, returns HTML views | XxxController.java |
| `@RestController` | Handles HTTP, returns JSON | XxxController.java |
| `@Service` | Business logic layer | XxxService.java |
| `@Repository` | Database access layer | XxxRepository.java |

---

### 1.1 `@Component`

**1) Theory:**
The base/generic annotation. When Spring scans your project, any class marked `@Component` gets picked up and added to the **Spring Application Context** (the container that holds all beans).

**2) Function:**
Marks a class as a Spring-managed bean. Spring creates one instance (singleton by default) and manages its lifecycle.

**3) Used in Service:**
Any utility/helper class that doesn't fit Controller, Service, or Repository categories.

**4) Used in Files:**
`EmailUtils.java`, `FileHelper.java`, `DateConverter.java` — any helper/utility class.

**5) Example:**
```java
@Component
public class EmailValidator {
    public boolean isValid(String email) {
        return email.contains("@");
    }
}
```

---

### 1.2 `@Controller`

**1) Theory:**
A specialization of `@Component` for the **web layer**. Returns view names (HTML pages via Thymeleaf or JSP). Part of MVC — the "C" in MVC.

**2) Function:**
Handles incoming HTTP requests and returns a **view** (HTML template name), not raw data.

**3) Used in Service:**
Traditional MVC web applications with server-side rendering.

**4) Used in Files:**
`UserController.java`, `HomeController.java`

**5) Example:**
```java
@Controller
public class HomeController {
    @GetMapping("/home")
    public String showHome(Model model) {
        model.addAttribute("msg", "Welcome!");
        return "home"; // returns home.html
    }
}
```

---

### 1.3 `@RestController` ⭐ (Most Important)

**1) Theory:**
`@RestController` = `@Controller` + `@ResponseBody` combined. Every method directly returns **data (JSON/XML)**, not a view name. This is the backbone of REST API development.

**2) Function:**
Converts return values to JSON automatically (via Jackson library) and sends them as HTTP responses.

**3) Used in Service:**
REST API backends, microservices, any frontend-backend separated architecture.

**4) Used in Files:**
`UserController.java`, `ProductController.java`

**5) Example:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return new User(id, "Rahul"); // Auto-converted to JSON: {"id":1,"name":"Rahul"}
    }
}
```

> **Memory tip:** `@Controller` → gives HTML page. `@RestController` → gives JSON data. REST = JSON.

---

### 1.4 `@Service`

**1) Theory:**
A specialization of `@Component` for the **business logic layer**. Sits between the Controller and Repository. Contains the "rules" of your application.

**2) Function:**
Holds business logic. Controllers call Service methods; Services call Repository methods. Makes code clean and testable (you can mock services in tests).

**3) Used in Service:**
Every Spring Boot application — UserService, OrderService, etc.

**4) Used in Files:**
`UserService.java`, `UserServiceImpl.java`

**5) Example:**
```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User getUserById(Long id) {
        return userRepository.findById(id)
               .orElseThrow(() -> new RuntimeException("User not found"));
    }
}
```

---

### 1.5 `@Repository`

**1) Theory:**
A specialization of `@Component` for the **data access layer**. Wraps database operations. Spring also adds automatic **exception translation** (converts database-specific exceptions to Spring's unified `DataAccessException`).

**2) Function:**
Marks a class/interface as a data repository. With Spring Data JPA, you just extend `JpaRepository` — no need to write implementation.

**3) Used in Service:**
Database operations, called only by the Service layer.

**4) Used in Files:**
`UserRepository.java`

**5) Example:**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Spring Data JPA auto-implements: findById, save, delete, findAll...
    List<User> findByEmail(String email); // custom query — auto-implemented!
}
```

---

### 🧠 Stereotype Pattern Summary

```
REQUEST FLOW (top to bottom):

  HTTP Request
       │
       ▼
@RestController  ──► receives request, calls service
       │
       ▼
@Service         ──► applies business logic, calls repo
       │
       ▼
@Repository      ──► talks to DB, returns data
       │
       ▼
  DATABASE
```

---

## 📦 GROUP 2: Dependency Injection Annotations (HOW to connect?)

> **Pattern:** These tell Spring *how to wire* beans together. Think of it as plugging in cables between components.

| Annotation | Purpose |
|---|---|
| `@Autowired` | Auto-inject a dependency |
| `@Qualifier` | Specify which bean when multiple exist |
| `@Value` | Inject a value from config file |
| `@Bean` | Manually create and register a bean |
| `@Configuration` | Mark a class as config provider |
| `@ComponentScan` | Tell Spring where to look for beans |

---

### 2.1 `@Autowired`

**1) Theory:**
Tells Spring to **automatically inject** a matching bean into this field/constructor/method. Spring looks in its container for a bean of the matching type and injects it.

**2) Function:**
Eliminates `new` keyword for managed objects. Spring creates the object and injects it.

**3) Used in Service:**
Controller, Service, and any class needing a dependency.

**4) Used in Files:**
`UserController.java`, `UserService.java`

**5) Example:**
```java
@RestController
public class UserController {

    @Autowired  // Spring injects UserService here automatically
    private UserService userService;

    // No need to write: UserService userService = new UserService();
}
```

> **Memory tip:** `@Autowired` = "Hey Spring, fill this in for me!"

---

### 2.2 `@Qualifier`

**1) Theory:**
When Spring finds **multiple beans of the same type**, it gets confused. `@Qualifier` tells Spring *exactly which one* to inject.

**2) Function:**
Disambiguates when multiple beans of the same type exist.

**3) Used in Service:**
Anywhere you have multiple implementations of the same interface.

**4) Used in Files:**
`UserController.java`, `UserService.java`

**5) Example:**
```java
// Two implementations of NotificationService:
@Service("emailNotification")
public class EmailNotificationService implements NotificationService { ... }

@Service("smsNotification")
public class SmsNotificationService implements NotificationService { ... }

// Inject specifically:
@Autowired
@Qualifier("emailNotification")
private NotificationService notificationService;
```

---

### 2.3 `@Value`

**1) Theory:**
Injects a value from `application.properties` or `application.yml` directly into a field. Keeps configuration outside code.

**2) Function:**
Reads config properties and injects them. Supports default values with `${key:default}`.

**3) Used in Service:**
Service and Configuration classes.

**4) Used in Files:**
`application.properties` + any Spring-managed class.

**5) Example:**
```properties
# application.properties
app.name=MyApp
app.max-users=100
```

```java
@Component
public class AppConfig {

    @Value("${app.name}")
    private String appName;

    @Value("${app.max-users:50}") // default = 50 if not found
    private int maxUsers;
}
```

---

### 2.4 `@Bean` + `@Configuration`

**1) Theory:**
When you need to create a bean from a **third-party class** (that you can't annotate directly), you use `@Bean` inside a `@Configuration` class. This is *manual* bean registration.

**2) Function:**
`@Configuration` marks the class as a source of bean definitions. `@Bean` marks a method whose return value should be registered as a Spring bean.

**3) Used in Service:**
Security config, datasource config, third-party library integration (e.g., BCryptPasswordEncoder).

**4) Used in Files:**
`SecurityConfig.java`, `AppConfig.java`, `DataSourceConfig.java`

**5) Example:**
```java
@Configuration
public class AppConfig {

    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(); // Spring manages this object
    }

    @Bean
    public ModelMapper modelMapper() {
        return new ModelMapper();
    }
}
```

> **Memory tip:** `@Bean` inside `@Configuration` = factory method pattern for Spring.

---

## 📦 GROUP 3: Web/REST Mapping Annotations (WHAT URL?)

> **Pattern:** These control the **routing** of HTTP requests to methods. Think of them as URL sign posts.

| Annotation | HTTP Method | Use |
|---|---|---|
| `@RequestMapping` | Any | Base URL mapping |
| `@GetMapping` | GET | Read/Fetch data |
| `@PostMapping` | POST | Create data |
| `@PutMapping` | PUT | Update (full) |
| `@PatchMapping` | PATCH | Update (partial) |
| `@DeleteMapping` | DELETE | Delete data |

---

### 3.1 `@RequestMapping`

**1) Theory:**
The parent annotation for all HTTP mapping. Can be placed at class level (sets base URL) or method level (sets specific endpoint).

**2) Function:**
Maps HTTP requests to handler methods. When at class level, all methods inherit its path as a prefix.

**3) Used in Service:**
Controllers — placed on the class to define a base path.

**4) Used in Files:**
`UserController.java`, `ProductController.java`

**5) Example:**
```java
@RestController
@RequestMapping("/api/users")  // all endpoints start with /api/users
public class UserController {

    @GetMapping("/all")         // full path: GET /api/users/all
    public List<User> getAll() { ... }

    @PostMapping("/create")     // full path: POST /api/users/create
    public User create() { ... }
}
```

---

### 3.2 CRUD Mapping Annotations ⭐

**Pattern to remember:**

```
CRUD Operation   →  HTTP Method  →  Spring Annotation
─────────────────────────────────────────────────────
Create           →  POST         →  @PostMapping
Read (one/all)   →  GET          →  @GetMapping
Update (full)    →  PUT          →  @PutMapping
Update (partial) →  PATCH        →  @PatchMapping
Delete           →  DELETE       →  @DeleteMapping
```

**Example — Full CRUD Controller:**
```java
@RestController
@RequestMapping("/api/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    // READ ALL
    @GetMapping
    public List<Product> getAllProducts() {
        return productService.findAll();
    }

    // READ ONE
    @GetMapping("/{id}")
    public Product getProduct(@PathVariable Long id) {
        return productService.findById(id);
    }

    // CREATE
    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.save(product);
    }

    // UPDATE
    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.update(id, product);
    }

    // DELETE
    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productService.delete(id);
    }
}
```

---

## 📦 GROUP 4: Request Parameter Annotations (HOW to extract input?)

> **Pattern:** These extract different parts of an HTTP request into method parameters.

| Annotation | Extracts From | Example URL |
|---|---|---|
| `@PathVariable` | URL path | `/users/42` |
| `@RequestParam` | Query string | `/users?name=Rahul` |
| `@RequestBody` | Request body (JSON) | POST with JSON body |
| `@RequestHeader` | HTTP headers | `Authorization: Bearer ...` |

---

### 4.1 `@PathVariable`

**1) Theory:**
Extracts a value embedded in the **URL path itself** (a path segment). Defined with `{variableName}` in the mapping.

**2) Function:**
Binds URL path segments to method parameters.

**3) Used in Service:**
REST APIs for resource identification (GET/PUT/DELETE by ID).

**4) Used in Files:**
`UserController.java`

**5) Example:**
```java
@GetMapping("/users/{id}")         // {id} is a path variable
public User getUser(@PathVariable Long id) {
    return userService.findById(id);
}
// Call: GET /users/42  →  id = 42
```

---

### 4.2 `@RequestParam`

**1) Theory:**
Extracts values from the **query string** (the `?key=value` part of a URL). Optional by default (can set `required=false` with a default value).

**2) Function:**
Binds query parameters to method parameters.

**3) Used in Service:**
Search, filter, pagination endpoints.

**4) Used in Files:**
`UserController.java`, `SearchController.java`

**5) Example:**
```java
@GetMapping("/users/search")
public List<User> searchUsers(
    @RequestParam String name,
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(required = false) String city
) {
    return userService.search(name, page, city);
}
// Call: GET /users/search?name=Rahul&page=1
```

---

### 4.3 `@RequestBody`

**1) Theory:**
Extracts and **deserializes the HTTP request body** (usually JSON) into a Java object. Uses Jackson library under the hood to convert JSON → Java object.

**2) Function:**
Binds the entire request body to a method parameter. Used in POST and PUT requests.

**3) Used in Service:**
All create (POST) and update (PUT) endpoints.

**4) Used in Files:**
`UserController.java`

**5) Example:**
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    // JSON body: {"name":"Rahul","email":"rahul@mail.com"}
    // Spring converts it to a User object automatically
    return userService.save(user);
}
```

---

### 4.4 Parameter Extraction — Visual Summary

```
HTTP Request:
  POST /api/users/42/orders?status=pending
  Headers: { Authorization: "Bearer xyz" }
  Body: { "product": "Laptop", "qty": 2 }

         ┌─────────────────────────────────┐
         │   @PathVariable    → "42"        │  ← from URL path
         │   @RequestParam    → "pending"   │  ← from ?status=pending
         │   @RequestBody     → {product...}│  ← from body JSON
         │   @RequestHeader   → "Bearer xyz"│  ← from header
         └─────────────────────────────────┘
```

---

## 📦 GROUP 5: JPA / Entity Annotations (DATABASE MAPPING)

> **Pattern:** These map Java objects to database tables and columns. Think of each annotation as a "bridge" instruction between Java and SQL.

| Annotation | Maps To |
|---|---|
| `@Entity` | A database table |
| `@Table` | Specifies table name |
| `@Id` | Primary key column |
| `@GeneratedValue` | Auto-increment strategy |
| `@Column` | Column definition |
| `@OneToMany` | One-to-many relationship |
| `@ManyToOne` | Many-to-one relationship |
| `@JoinColumn` | Foreign key column |

---

### 5.1 Core Entity Annotations

**1) Theory:**
`@Entity` tells JPA (Hibernate) that this Java class maps to a database table. Each instance = one row.

**2) Full Example with all core annotations:**
```java
@Entity                          // This class = a DB table
@Table(name = "users")           // Table name: "users"
public class User {

    @Id                           // This field = Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto increment
    private Long id;

    @Column(name = "full_name", nullable = false, length = 100)
    private String name;

    @Column(unique = true)
    private String email;

    @Column(name = "created_at")
    private LocalDateTime createdAt;
}
```

**3) Used in Service:** Entity/Domain layer.

**4) Used in Files:** `User.java`, `Product.java`, `Order.java` (model classes)

---

### 5.2 Relationship Annotations

```
RELATIONSHIP CHEAT SHEET:

One User → Many Orders:
  User class:   @OneToMany(mappedBy = "user")  List<Order> orders
  Order class:  @ManyToOne  User user;
                @JoinColumn(name = "user_id")   ← FK column in orders table

One-to-Many means: 1 User can place N Orders
```

**Example:**
```java
// User.java
@Entity
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Order> orders;
}

// Order.java
@Entity
public class Order {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    @JoinColumn(name = "user_id")  // FK column in 'orders' table
    private User user;
}
```

---

## 📦 GROUP 6: Security Annotations (Unit 3)

> **Pattern:** These control WHO can access WHAT. Think of them as security guards at different doors.

| Annotation | Purpose |
|---|---|
| `@EnableWebSecurity` | Activates Spring Security |
| `@PreAuthorize` | Checks permission before method runs |
| `@Secured` | Role-based access (older style) |
| `@EnableGlobalMethodSecurity` | Enables method-level security |

---

### 6.1 `@EnableWebSecurity` + Security Config

**1) Theory:**
Placed on a `@Configuration` class. Tells Spring Security to take over HTTP security. You then extend `WebSecurityConfigurerAdapter` (or use component-based config in newer versions) to define rules.

**2) Function:**
Activates Spring Security's full suite — authentication, authorization, CSRF, session management.

**3) Used in Service:**
Authentication & Authorization service.

**4) Used in Files:**
`SecurityConfig.java`

**5) Example:**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests()
                .requestMatchers("/api/auth/**").permitAll()   // public
                .requestMatchers("/api/admin/**").hasRole("ADMIN")  // admin only
                .anyRequest().authenticated()
            .and()
            .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS); // JWT mode
        return http.build();
    }
}
```

---

### 6.2 `@PreAuthorize`

**1) Theory:**
Method-level security annotation. Evaluated **before** the method runs. Uses Spring Expression Language (SpEL).

**2) Function:**
Restricts method access based on roles or expressions.

**3) Used in Service:**
Service and Controller methods.

**4) Used in Files:**
`UserController.java`, `UserService.java`

**5) Example:**
```java
@PreAuthorize("hasRole('ADMIN')")
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable Long id) {
    userService.delete(id);
}

@PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
@GetMapping("/profile")
public UserProfile getProfile() { ... }
```

---

## 📦 GROUP 7: Testing Annotations (Unit 3)

> **Pattern:** These set up a test environment so you can test components in isolation.

| Annotation | Purpose |
|---|---|
| `@SpringBootTest` | Full application context test |
| `@WebMvcTest` | Test only web layer (controllers) |
| `@DataJpaTest` | Test only JPA/DB layer |
| `@MockBean` | Create a mock bean in Spring context |
| `@Mock` (Mockito) | Create a mock object |
| `@InjectMocks` (Mockito) | Inject mocks into the target |
| `@Test` | Mark a method as a test |
| `@ExtendWith` | Extend JUnit with Mockito/Spring |

---

### 7.1 Testing Flow

```
TEST TYPES:

Unit Test (no Spring context):
  @ExtendWith(MockitoExtension.class)
  @Mock → @InjectMocks
  Tests: Service logic in isolation

Web Layer Test (mock web layer):
  @WebMvcTest(UserController.class)
  @MockBean → inject mock services
  Tests: Controller endpoints

Integration Test (full app):
  @SpringBootTest
  Tests: Everything end-to-end
```

**Example — Service Unit Test with Mockito:**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;  // fake repo

    @InjectMocks
    private UserService userService;        // real service with fake repo injected

    @Test
    void testGetUserById_Found() {
        User mockUser = new User(1L, "Rahul", "rahul@test.com");
        Mockito.when(userRepository.findById(1L))
               .thenReturn(Optional.of(mockUser));

        User result = userService.getUserById(1L);

        assertEquals("Rahul", result.getName());
    }
}
```

**Example — Controller Test with MockMvc:**
```java
@WebMvcTest(UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    void testGetUser() throws Exception {
        Mockito.when(userService.getUserById(1L))
               .thenReturn(new User(1L, "Rahul", "r@test.com"));

        mockMvc.perform(get("/api/users/1"))
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.name").value("Rahul"));
    }
}
```

---

## 📦 GROUP 8: Exception Handling Annotations

> **Pattern:** These intercept exceptions globally and return proper error responses instead of stack traces.

| Annotation | Purpose |
|---|---|
| `@ControllerAdvice` | Global exception handler class |
| `@ExceptionHandler` | Handle a specific exception |
| `@ResponseStatus` | Set HTTP status code |

---

### 8.1 `@ControllerAdvice` + `@ExceptionHandler`

**1) Theory:**
`@ControllerAdvice` creates a **global exception handler** — a single place to catch exceptions thrown anywhere in your controllers. `@ExceptionHandler` specifies which exception type each method handles.

**2) Function:**
Intercepts exceptions and returns structured error responses (JSON) instead of raw stack traces.

**3) Used in Service:**
Exception handling layer — applies to entire application.

**4) Used in Files:**
`GlobalExceptionHandler.java`

**5) Example:**
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse("NOT_FOUND", ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        ErrorResponse error = new ErrorResponse("INTERNAL_ERROR", "Something went wrong");
        return ResponseEntity.status(500).body(error);
    }
}
```

---

## 📦 GROUP 9: Spring Boot Core Annotations

> **Pattern:** These are the "entry point" and "auto-configuration" annotations.

| Annotation | Purpose |
|---|---|
| `@SpringBootApplication` | Main entry point |
| `@EnableAutoConfiguration` | Auto-configures Spring |
| `@ComponentScan` | Scans for beans |
| `@Transactional` | Wraps method in DB transaction |
| `@CrossOrigin` | Allow CORS for frontend connection |

---

### 9.1 `@SpringBootApplication`

**1) Theory:**
The all-in-one annotation on your main class. It's actually a shortcut for 3 annotations combined:
- `@Configuration`
- `@EnableAutoConfiguration`
- `@ComponentScan`

**2) Function:**
Starts the Spring application — bootstraps context, auto-configures, scans for beans.

**3) Used in Files:** `Application.java` / `MainApplication.java`

**5) Example:**
```java
@SpringBootApplication  // = @Configuration + @EnableAutoConfiguration + @ComponentScan
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

---

### 9.2 `@Transactional`

**1) Theory:**
Wraps a method (or entire class) in a **database transaction**. If the method fails, all DB changes are rolled back — guaranteeing data consistency.

**2) Function:**
Ensures atomicity — all DB operations in the method succeed together or fail together.

**3) Used in Service:**
Service methods that perform multiple DB operations.

**4) Used in Files:**
`UserService.java`, `OrderService.java`

**5) Example:**
```java
@Service
public class OrderService {

    @Transactional  // if any DB call fails → entire transaction rolls back
    public void placeOrder(Order order) {
        orderRepository.save(order);
        inventoryRepository.reduceStock(order.getProductId());
        paymentRepository.processPayment(order.getAmount());
        // If payment fails → order is NOT saved, stock is NOT reduced
    }
}
```

---

### 9.3 `@CrossOrigin`

**1) Theory:**
Browsers block requests from one origin (e.g., React on `localhost:3000`) to another origin (Spring Boot on `localhost:8080`) by default — this is called the **Same-Origin Policy**. `@CrossOrigin` tells Spring Boot to allow these cross-origin requests.

**2) Function:**
Adds CORS headers to responses, allowing specified frontend origins to make API calls.

**3) Used in Service:**
Full-stack development — connecting React/Angular frontend to Spring Boot backend (Unit 5).

**4) Used in Files:**
`UserController.java` or global `WebMvcConfigurer`

**5) Example:**
```java
// On specific controller:
@CrossOrigin(origins = "http://localhost:3000")
@RestController
@RequestMapping("/api/users")
public class UserController { ... }

// OR globally in config:
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000")
                .allowedMethods("GET", "POST", "PUT", "DELETE");
    }
}
```

---

## 🗺️ MASTER FLOWCHART — Where Each Annotation Lives

```
project/
├── src/main/java/com/example/
│   │
│   ├── Application.java                 ← @SpringBootApplication
│   │
│   ├── config/
│   │   ├── SecurityConfig.java          ← @Configuration, @EnableWebSecurity, @Bean
│   │   ├── AppConfig.java               ← @Configuration, @Bean
│   │   └── CorsConfig.java              ← @Configuration, @CrossOrigin
│   │
│   ├── controller/
│   │   └── UserController.java          ← @RestController, @RequestMapping,
│   │                                       @GetMapping, @PostMapping, @PutMapping,
│   │                                       @DeleteMapping, @PathVariable,
│   │                                       @RequestParam, @RequestBody, @CrossOrigin,
│   │                                       @PreAuthorize
│   │
│   ├── service/
│   │   └── UserService.java             ← @Service, @Autowired, @Transactional
│   │
│   ├── repository/
│   │   └── UserRepository.java          ← @Repository (extends JpaRepository)
│   │
│   ├── model/
│   │   └── User.java                    ← @Entity, @Table, @Id,
│   │                                       @GeneratedValue, @Column,
│   │                                       @OneToMany, @ManyToOne, @JoinColumn
│   │
│   └── exception/
│       └── GlobalExceptionHandler.java  ← @ControllerAdvice, @ExceptionHandler,
│                                           @ResponseStatus
│
└── src/main/resources/
    └── application.properties           ← values read by @Value
```

---

## 🧠 QUICK-RECALL CHEAT SHEET

```
STEREOTYPE (who am I?):
  @Component       → generic bean
  @Controller      → web, returns HTML
  @RestController  → web, returns JSON ✦
  @Service         → business logic ✦
  @Repository      → database access ✦

INJECTION (how do I connect?):
  @Autowired       → auto-inject bean ✦
  @Qualifier       → pick specific bean
  @Value           → inject from config
  @Bean            → manual bean definition
  @Configuration   → config class

HTTP MAPPING (which URL?):
  @RequestMapping  → base path
  @GetMapping      → fetch ✦
  @PostMapping     → create ✦
  @PutMapping      → update ✦
  @DeleteMapping   → delete ✦

INPUT EXTRACTION (where from?):
  @PathVariable    → /resource/{id} ✦
  @RequestParam    → ?key=value
  @RequestBody     → JSON body ✦
  @RequestHeader   → HTTP headers

JPA/DB:
  @Entity          → maps to DB table ✦
  @Id              → primary key ✦
  @GeneratedValue  → auto-increment ✦
  @Column          → column definition
  @OneToMany       → relationship
  @ManyToOne       → relationship ✦

SECURITY:
  @EnableWebSecurity  → activate security
  @PreAuthorize       → method-level auth ✦

TESTING:
  @SpringBootTest     → full app test
  @WebMvcTest         → web layer only
  @MockBean           → mock in Spring context
  @Mock               → Mockito mock
  @InjectMocks        → inject mocks

EXCEPTION HANDLING:
  @ControllerAdvice   → global handler ✦
  @ExceptionHandler   → handle specific error ✦

CORE:
  @SpringBootApplication → entry point ✦
  @Transactional         → DB transaction ✦
  @CrossOrigin           → allow CORS ✦

✦ = High priority, learn these first
```

---

## 🔁 PATTERN MATCHING TRICKS

1. **All `@___Mapping` annotations** → they map URLs to methods. HTTP verb is in the name.
2. **`@___Controller`** → handles web requests. `Rest` prefix = JSON output.
3. **`@___Test`** → testing annotations. Suffix tells you what layer it tests.
4. **`@Enable___`** → enables a Spring feature (WebSecurity, GlobalMethodSecurity).
5. **`@___Advice`** → applies globally to controllers.
6. **`@Pre___`** → executes before something (PreAuthorize = before method).
7. **`@___Bean` / `@Configuration`** → always go together.
8. **`@___Body` / `@___Variable` / `@___Param`** → extract input from different parts of HTTP request.

---

*Generated for T.Y. B.Tech IT — PCCoE Pune | Full Stack Development Syllabus*
