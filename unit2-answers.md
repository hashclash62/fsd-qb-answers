# Unit 2 Answers: RESTful APIs and Database Connectivity

Subject: Full Stack Development  
Unit: RESTful APIs and Database Connectivity

---

## 1. Illustrate how layered architecture improves maintainability in applications.

Layered architecture is a design approach where an application is divided into separate layers, and each layer has a specific responsibility. In Spring Boot REST applications, the common layers are Controller, Service, Repository, and Database.

```text
Client / Frontend
       |
       v
Controller Layer
       |
       v
Service Layer
       |
       v
Repository Layer
       |
       v
Database
```

### Controller Layer

The Controller handles HTTP requests and responses. It uses annotations such as `@RestController`, `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping`.

### Service Layer

The Service layer contains business logic. It decides what should happen in the application.

### Repository Layer

The Repository layer communicates with the database. In Spring Boot, it is commonly created using Spring Data JPA interfaces.

### Database Layer

The Database stores actual data such as student, employee, product, or order records.

Maintainability benefits:

- Each layer has a clear responsibility.
- Code becomes easier to understand.
- Changes in one layer do not heavily affect other layers.
- Testing becomes easier because each layer can be tested separately.
- Business logic is not mixed with request handling or database logic.
- Team members can work on different layers independently.

Mental model:

```text
Controller = Reception desk
Service    = Manager who applies rules
Repository = Clerk who accesses records
Database   = Record room
```

For example, if database logic changes from MySQL to MongoDB, most changes should happen in the Repository layer, while the Controller and Service layers can remain mostly unchanged.

---

## 2. Apply MongoDB with Spring Boot to store simple JSON documents.

MongoDB is a NoSQL database that stores data in document format. A MongoDB document is similar to JSON. Spring Boot can connect to MongoDB using Spring Data MongoDB.

Dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

`application.properties`:

```properties
spring.data.mongodb.uri=mongodb://localhost:27017/studentdb
```

Document class:

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document(collection = "students")
public class Student {

    @Id
    private String id;
    private String name;
    private String email;

    // getters and setters
}
```

Repository:

```java
import org.springframework.data.mongodb.repository.MongoRepository;

public interface StudentRepository
        extends MongoRepository<Student, String> {
}
```

Controller:

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    private final StudentRepository studentRepository;

    public StudentController(StudentRepository studentRepository) {
        this.studentRepository = studentRepository;
    }

    @PostMapping
    public Student saveStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }
}
```

JSON request:

```json
{
  "name": "Amit",
  "email": "amit@example.com"
}
```

Flow:

```text
Frontend sends JSON
       |
       v
Spring Boot @RequestBody
       |
       v
Student document object
       |
       v
MongoRepository.save()
       |
       v
MongoDB collection
```

MongoDB is useful when data is document-based, flexible, and does not require strict table relationships.

---

## 3. Use application.properties in configuring Spring Boot apps.

`application.properties` is a configuration file used in Spring Boot applications. It is placed inside the `src/main/resources` folder. It stores configuration values such as server port, database URL, username, password, logging level, and custom application properties.

Location:

```text
src/main/resources/application.properties
```

Example:

```properties
server.port=8081

spring.datasource.url=jdbc:mysql://localhost:3306/studentdb
spring.datasource.username=root
spring.datasource.password=password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

app.name=Student Management System
```

Uses:

- Configure server port.
- Configure database connection.
- Configure JPA and Hibernate.
- Configure logging.
- Store custom application values.
- Change behavior without changing Java code.

Reading custom property:

```java
@RestController
public class AppController {

    @Value("${app.name}")
    private String appName;

    @GetMapping("/app-name")
    public String getAppName() {
        return appName;
    }
}
```

Configuration flow:

```text
application.properties
        |
        v
Spring Boot reads values at startup
        |
        v
Auto-configuration uses values
        |
        v
Application runs with configured settings
```

Thus, `application.properties` separates configuration from source code and makes Spring Boot applications easier to manage.

---

## 4. Apply Spring Boot annotations to build a simple Employee CRUD system.

A simple Employee CRUD system can be built using Spring Boot annotations for Controller, Service, Repository, Entity, and mapping methods.

Layered structure:

```text
EmployeeController
        |
        v
EmployeeService
        |
        v
EmployeeRepository
        |
        v
Database
```

Entity:

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String department;
    private double salary;

    // getters and setters
}
```

Repository:

```java
@Repository
public interface EmployeeRepository
        extends JpaRepository<Employee, Long> {
}
```

Service:

```java
@Service
public class EmployeeService {

    private final EmployeeRepository employeeRepository;

    public EmployeeService(EmployeeRepository employeeRepository) {
        this.employeeRepository = employeeRepository;
    }

    public List<Employee> getAllEmployees() {
        return employeeRepository.findAll();
    }

    public Employee saveEmployee(Employee employee) {
        return employeeRepository.save(employee);
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    private final EmployeeService employeeService;

    public EmployeeController(EmployeeService employeeService) {
        this.employeeService = employeeService;
    }

    @GetMapping
    public List<Employee> getEmployees() {
        return employeeService.getAllEmployees();
    }

    @PostMapping
    public Employee createEmployee(@RequestBody Employee employee) {
        return employeeService.saveEmployee(employee);
    }
}
```

Important annotations:

- `@Entity`: marks class as database table.
- `@Id`: marks primary key.
- `@Repository`: marks database access layer.
- `@Service`: marks business logic layer.
- `@RestController`: marks REST API controller.
- `@GetMapping`, `@PostMapping`: map HTTP requests.

---

## 5. Design a Customer Feedback API with proper HTTP methods.

A Customer Feedback API should use proper REST endpoints and HTTP methods. Feedback is treated as a resource.

Resource:

```text
Feedback
  id
  customerName
  email
  message
  rating
```

API design:

| Operation | HTTP Method | Endpoint | Purpose |
|---|---|---|---|
| Create feedback | `POST` | `/api/feedbacks` | Submit new feedback |
| Get all feedback | `GET` | `/api/feedbacks` | View all feedback |
| Get feedback by id | `GET` | `/api/feedbacks/{id}` | View one feedback |
| Update feedback | `PUT` | `/api/feedbacks/{id}` | Update feedback |
| Delete feedback | `DELETE` | `/api/feedbacks/{id}` | Remove feedback |

Controller example:

```java
@RestController
@RequestMapping("/api/feedbacks")
public class FeedbackController {

    @PostMapping
    public Feedback createFeedback(@RequestBody Feedback feedback) {
        return feedbackService.save(feedback);
    }

    @GetMapping
    public List<Feedback> getAllFeedbacks() {
        return feedbackService.findAll();
    }

    @GetMapping("/{id}")
    public Feedback getFeedback(@PathVariable Long id) {
        return feedbackService.findById(id);
    }

    @PutMapping("/{id}")
    public Feedback updateFeedback(@PathVariable Long id,
                                   @RequestBody Feedback feedback) {
        return feedbackService.update(id, feedback);
    }

    @DeleteMapping("/{id}")
    public void deleteFeedback(@PathVariable Long id) {
        feedbackService.delete(id);
    }
}
```

Design mental model:

```text
Resource name in URL: /feedbacks
Action in HTTP method: GET, POST, PUT, DELETE
```

This is better than using URLs like `/addFeedback` or `/deleteFeedback` because REST APIs should use nouns in URLs and verbs in HTTP methods.

---

## 6. Apply validation annotations (e.g., @Valid) in request handling.

Validation is used to check whether incoming request data is correct before processing it. In Spring Boot, validation annotations can be applied on model fields, and `@Valid` can be used in controller methods.

Dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

Request DTO:

```java
public class UserRequest {

    @NotBlank(message = "Name is required")
    private String name;

    @Email(message = "Email must be valid")
    private String email;

    @Min(value = 18, message = "Age must be at least 18")
    private int age;

    // getters and setters
}
```

Controller:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping
    public String createUser(@Valid @RequestBody UserRequest userRequest) {
        return "User created successfully";
    }
}
```

Validation flow:

```text
JSON request received
        |
        v
@RequestBody maps JSON to object
        |
        v
@Valid triggers validation
        |
   +----+----+
   |         |
Valid     Invalid
   |         |
Process   Return validation error
```

Common validation annotations:

- `@NotNull`: value cannot be null.
- `@NotBlank`: string cannot be null or empty.
- `@Email`: validates email format.
- `@Min`: minimum numeric value.
- `@Max`: maximum numeric value.
- `@Size`: length or collection size.

Validation improves API reliability by preventing invalid data from entering the application.

---

## 7. Demonstrate how Controller communicates with Service layer using example.

In layered architecture, the Controller should not contain business logic directly. It should receive HTTP requests and call the Service layer. The Service layer performs business operations and returns data to the Controller.

Flow:

```text
Client request
      |
      v
Controller
      |
      v
Service
      |
      v
Controller returns response
```

Service:

```java
@Service
public class StudentService {

    public String getStudentName() {
        return "Amit";
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    private final StudentService studentService;

    public StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    @GetMapping("/name")
    public String getStudentName() {
        return studentService.getStudentName();
    }
}
```

Explanation:

- `StudentController` receives the request.
- `StudentService` contains business logic.
- Constructor injection provides the service object to the controller.
- The controller calls `studentService.getStudentName()`.

This separation improves maintainability because the Controller handles web logic and the Service handles business logic.

---

## 8. Design a RESTful API for a Student Management System. Include endpoints for CRUD operations and specify which Spring Boot annotations will be used and justify your choices.

A Student Management System should expose REST endpoints for creating, reading, updating, and deleting student records.

Student resource:

```text
Student
  id
  name
  email
  course
```

REST API endpoint design:

| Operation | HTTP Method | Endpoint | Annotation | Justification |
|---|---|---|---|---|
| Create student | `POST` | `/api/students` | `@PostMapping` | Creates new data |
| Get all students | `GET` | `/api/students` | `@GetMapping` | Reads all records |
| Get student by id | `GET` | `/api/students/{id}` | `@GetMapping` | Reads one record |
| Update student | `PUT` | `/api/students/{id}` | `@PutMapping` | Updates existing record |
| Delete student | `DELETE` | `/api/students/{id}` | `@DeleteMapping` | Deletes record |

Controller:

```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentService.save(student);
    }

    @GetMapping
    public List<Student> getStudents() {
        return studentService.findAll();
    }

    @GetMapping("/{id}")
    public Student getStudent(@PathVariable Long id) {
        return studentService.findById(id);
    }

    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id,
                                 @RequestBody Student student) {
        return studentService.update(id, student);
    }

    @DeleteMapping("/{id}")
    public void deleteStudent(@PathVariable Long id) {
        studentService.delete(id);
    }
}
```

Mental model:

```text
URL identifies resource
HTTP method identifies action
Controller annotation maps the request
```

Data flow:

```text
Frontend/Postman
      |
      v
StudentController
      |
      v
StudentService
      |
      v
StudentRepository
      |
      v
Database
```

This design follows REST principles and keeps API operations clear.

---

## 9. Apply it to a real scenario where a Controller depends on a Service and Repository layer. Illustrate how loose coupling is achieved.

Consider an order management system. The Controller handles API requests, the Service applies business rules, and the Repository performs database operations.

Layer structure:

```text
OrderController
      |
      v
OrderService interface
      |
      v
OrderServiceImpl
      |
      v
OrderRepository
      |
      v
Database
```

Repository:

```java
@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {
}
```

Service interface:

```java
public interface OrderService {
    Order placeOrder(Order order);
}
```

Service implementation:

```java
@Service
public class OrderServiceImpl implements OrderService {

    private final OrderRepository orderRepository;

    public OrderServiceImpl(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public Order placeOrder(Order order) {
        return orderRepository.save(order);
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/orders")
public class OrderController {

    private final OrderService orderService;

    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @PostMapping
    public Order placeOrder(@RequestBody Order order) {
        return orderService.placeOrder(order);
    }
}
```

Loose coupling is achieved because:

- Controller depends on `OrderService`, not directly on `OrderRepository`.
- Business logic is hidden behind the service interface.
- Repository logic can change without changing the Controller.
- In tests, a mock service can be injected into the Controller.

Mental model:

```text
Controller knows "what service to call"
Service knows "what business rules to apply"
Repository knows "how to talk to database"
```

This separation makes the application easier to modify and test.

---

## 10. Given a requirement to manage employee records, design Controller-Service-Repository layers. Explain the role of each layer and how data flows between them.

For managing employee records, the application can be divided into Entity, Repository, Service, and Controller layers.

Layer diagram:

```text
HTTP Request
    |
    v
EmployeeController
    |
    v
EmployeeService
    |
    v
EmployeeRepository
    |
    v
Database
```

Entity:

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String department;
    private double salary;
}
```

Repository:

```java
@Repository
public interface EmployeeRepository
        extends JpaRepository<Employee, Long> {
}
```

Service:

```java
@Service
public class EmployeeService {

    private final EmployeeRepository employeeRepository;

    public EmployeeService(EmployeeRepository employeeRepository) {
        this.employeeRepository = employeeRepository;
    }

    public List<Employee> getEmployees() {
        return employeeRepository.findAll();
    }

    public Employee saveEmployee(Employee employee) {
        return employeeRepository.save(employee);
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/employees")
public class EmployeeController {

    private final EmployeeService employeeService;

    public EmployeeController(EmployeeService employeeService) {
        this.employeeService = employeeService;
    }

    @GetMapping
    public List<Employee> getEmployees() {
        return employeeService.getEmployees();
    }

    @PostMapping
    public Employee saveEmployee(@RequestBody Employee employee) {
        return employeeService.saveEmployee(employee);
    }
}
```

Role of each layer:

- Controller: handles REST API requests and responses.
- Service: contains business logic such as validation or calculations.
- Repository: performs database operations.
- Entity: represents database table structure.

This layered structure improves maintainability because each class has one clear responsibility.

---

## 11. Apply Spring Data JPA to implement CRUD operations. Describe how repository interfaces are created and explain how query methods are derived automatically.

Spring Data JPA simplifies database operations by providing repository interfaces. Developers do not need to write basic SQL queries for common CRUD operations.

Entity:

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String course;
    private int marks;
}
```

Repository interface:

```java
@Repository
public interface StudentRepository
        extends JpaRepository<Student, Long> {

    List<Student> findByCourse(String course);

    List<Student> findByMarksGreaterThan(int marks);
}
```

`JpaRepository<Student, Long>` means:

- `Student` is the entity type.
- `Long` is the primary key type.

Built-in CRUD methods:

- `save(entity)`
- `findAll()`
- `findById(id)`
- `deleteById(id)`
- `existsById(id)`
- `count()`

Derived query method examples:

```java
List<Student> findByCourse(String course);
List<Student> findByNameContaining(String keyword);
List<Student> findByMarksGreaterThan(int marks);
```

Spring Data JPA reads the method name and creates the query automatically.

Flow:

```text
Repository method name
        |
        v
Spring Data JPA interprets method
        |
        v
Query generated automatically
        |
        v
Database returns result
```

This reduces boilerplate code and makes CRUD operations faster to implement.

---

## 12. Design a REST API for payment transaction records.

A Payment Transaction API should allow clients to create, view, update, and track payment transactions. It should use proper HTTP methods and clear resource names.

Payment transaction resource:

```text
PaymentTransaction
  id
  orderId
  amount
  paymentMode
  status
  transactionDate
```

Endpoint design:

| Operation | HTTP Method | Endpoint | Purpose |
|---|---|---|---|
| Create transaction | `POST` | `/api/payments` | Create payment record |
| Get all transactions | `GET` | `/api/payments` | Fetch all payments |
| Get transaction by id | `GET` | `/api/payments/{id}` | Fetch one payment |
| Get payments by status | `GET` | `/api/payments/status/{status}` | Filter by status |
| Update payment status | `PUT` | `/api/payments/{id}/status` | Update transaction status |
| Delete transaction | `DELETE` | `/api/payments/{id}` | Remove transaction record |

Controller example:

```java
@RestController
@RequestMapping("/api/payments")
public class PaymentController {

    @PostMapping
    public Payment createPayment(@RequestBody Payment payment) {
        return paymentService.createPayment(payment);
    }

    @GetMapping("/{id}")
    public Payment getPayment(@PathVariable Long id) {
        return paymentService.getPayment(id);
    }

    @GetMapping("/status/{status}")
    public List<Payment> getPaymentsByStatus(@PathVariable String status) {
        return paymentService.getByStatus(status);
    }

    @PutMapping("/{id}/status")
    public Payment updateStatus(@PathVariable Long id,
                                @RequestBody Payment payment) {
        return paymentService.updateStatus(id, payment.getStatus());
    }
}
```

Mental model:

```text
Payment created -> PENDING
Payment verified -> SUCCESS or FAILED
Payment status updated through PUT
```

This API design is suitable for tracking payment records in an order or billing system.

---

## 13. Design REST endpoints for a Product API using appropriate mapping annotations.

A Product API should provide endpoints for adding, reading, updating, deleting, and searching products.

Product resource:

```text
Product
  id
  name
  category
  price
  quantity
```

Endpoint design:

| Operation | Method | Endpoint | Annotation |
|---|---|---|---|
| Add product | `POST` | `/api/products` | `@PostMapping` |
| Get all products | `GET` | `/api/products` | `@GetMapping` |
| Get product by id | `GET` | `/api/products/{id}` | `@GetMapping("/{id}")` |
| Update product | `PUT` | `/api/products/{id}` | `@PutMapping("/{id}")` |
| Delete product | `DELETE` | `/api/products/{id}` | `@DeleteMapping("/{id}")` |
| Get by category | `GET` | `/api/products/category/{category}` | `@GetMapping` |

Controller:

```java
@RestController
@RequestMapping("/api/products")
public class ProductController {

    @PostMapping
    public Product addProduct(@RequestBody Product product) {
        return productService.save(product);
    }

    @GetMapping
    public List<Product> getProducts() {
        return productService.findAll();
    }

    @GetMapping("/{id}")
    public Product getProduct(@PathVariable Long id) {
        return productService.findById(id);
    }

    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id,
                                 @RequestBody Product product) {
        return productService.update(id, product);
    }

    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productService.delete(id);
    }
}
```

Design rule:

```text
Use nouns in endpoint URLs.
Use HTTP methods for actions.
```

This makes the Product API RESTful, readable, and easy to test using Postman.

---

## 14. Apply @RequestBody and @ResponseBody in handling user registration data.

`@RequestBody` is used to read JSON data from the HTTP request body and convert it into a Java object. `@ResponseBody` is used to send method return data directly in the HTTP response body.

In `@RestController`, `@ResponseBody` is applied automatically to all methods.

Request DTO:

```java
public class UserRegistrationRequest {
    private String username;
    private String email;
    private String password;
}
```

Response DTO:

```java
public class UserRegistrationResponse {
    private String message;
    private String username;

    public UserRegistrationResponse(String message, String username) {
        this.message = message;
        this.username = username;
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping("/register")
    public UserRegistrationResponse registerUser(
            @RequestBody UserRegistrationRequest request) {

        return new UserRegistrationResponse(
                "User registered successfully",
                request.getUsername()
        );
    }
}
```

JSON request:

```json
{
  "username": "john",
  "email": "john@example.com",
  "password": "123456"
}
```

JSON response:

```json
{
  "message": "User registered successfully",
  "username": "john"
}
```

Flow:

```text
JSON request body
      |
      v
@RequestBody
      |
      v
Java request object
      |
      v
Controller returns Java response object
      |
      v
@ResponseBody converts it to JSON
```

This is the standard way to handle JSON request and response data in Spring Boot REST APIs.

---

## 15. Illustrate how @Service and @Repository interact in a banking application.

In a banking application, `@Service` contains business logic such as deposit, withdrawal, and balance checking. `@Repository` performs database operations such as saving account details and fetching account records.

Layer diagram:

```text
BankController
      |
      v
BankService  (@Service)
      |
      v
AccountRepository  (@Repository)
      |
      v
Database
```

Entity:

```java
@Entity
public class Account {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String accountHolder;
    private double balance;
}
```

Repository:

```java
@Repository
public interface AccountRepository
        extends JpaRepository<Account, Long> {
}
```

Service:

```java
@Service
public class BankService {

    private final AccountRepository accountRepository;

    public BankService(AccountRepository accountRepository) {
        this.accountRepository = accountRepository;
    }

    public Account deposit(Long id, double amount) {
        Account account = accountRepository.findById(id).orElseThrow();
        account.setBalance(account.getBalance() + amount);
        return accountRepository.save(account);
    }
}
```

Interaction flow:

```text
Deposit request
      |
      v
BankService checks business rule
      |
      v
AccountRepository fetches account
      |
      v
Service updates balance
      |
      v
Repository saves account
```

The Service layer decides what should happen, while the Repository layer decides how to interact with the database.

---

## 16. Design a CRUD API for Course Management using Spring Boot annotations.

A Course Management API can be designed using Spring Boot REST annotations and layered architecture.

Course entity:

```java
@Entity
public class Course {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String instructor;
    private int durationInWeeks;
}
```

Repository:

```java
@Repository
public interface CourseRepository
        extends JpaRepository<Course, Long> {
}
```

Controller endpoints:

| Operation | Method | Endpoint | Annotation |
|---|---|---|---|
| Add course | `POST` | `/api/courses` | `@PostMapping` |
| Get all courses | `GET` | `/api/courses` | `@GetMapping` |
| Get course by id | `GET` | `/api/courses/{id}` | `@GetMapping("/{id}")` |
| Update course | `PUT` | `/api/courses/{id}` | `@PutMapping("/{id}")` |
| Delete course | `DELETE` | `/api/courses/{id}` | `@DeleteMapping("/{id}")` |

Controller:

```java
@RestController
@RequestMapping("/api/courses")
public class CourseController {

    private final CourseService courseService;

    public CourseController(CourseService courseService) {
        this.courseService = courseService;
    }

    @PostMapping
    public Course createCourse(@RequestBody Course course) {
        return courseService.save(course);
    }

    @GetMapping
    public List<Course> getCourses() {
        return courseService.findAll();
    }

    @PutMapping("/{id}")
    public Course updateCourse(@PathVariable Long id,
                               @RequestBody Course course) {
        return courseService.update(id, course);
    }

    @DeleteMapping("/{id}")
    public void deleteCourse(@PathVariable Long id) {
        courseService.delete(id);
    }
}
```

Mental model:

```text
Course is the resource.
HTTP method decides action.
Path variable identifies one course.
Request body carries new or updated data.
```

---

## 17. Apply Spring Boot properties configuration to connect an application to a database.

Spring Boot uses `application.properties` to configure database connectivity. These properties tell Spring Boot which database to connect to and how Hibernate should manage tables.

MySQL dependency:

```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
```

JPA dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

`application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/studentdb
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

Configuration flow:

```text
Spring Boot starts
        |
        v
Reads application.properties
        |
        v
Creates DataSource
        |
        v
Configures JPA and Hibernate
        |
        v
Repository can access database
```

Meaning of important properties:

- `spring.datasource.url`: database connection URL.
- `spring.datasource.username`: database username.
- `spring.datasource.password`: database password.
- `spring.jpa.hibernate.ddl-auto=update`: updates tables based on entities.
- `spring.jpa.show-sql=true`: prints generated SQL queries.

These properties connect the Spring Boot application to a relational database.

---

## 18. Explain how Dependency Injection simplifies testing in a real-world application.

Dependency Injection, or DI, means that objects are provided to a class from outside instead of being created inside the class. Spring Boot uses DI to inject beans such as services and repositories.

Without DI:

```java
public class OrderService {
    private OrderRepository repository = new OrderRepository();
}
```

This is tightly coupled because `OrderService` directly creates its dependency.

With DI:

```java
@Service
public class OrderService {

    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }
}
```

Testing benefit:

```text
Real application:
OrderService -> Real OrderRepository

Unit test:
OrderService -> Mock OrderRepository
```

Example test idea:

```java
OrderRepository mockRepository = mock(OrderRepository.class);
OrderService orderService = new OrderService(mockRepository);
```

How DI simplifies testing:

- Dependencies can be replaced with mocks.
- Tests do not need real databases or external services.
- Business logic can be tested separately.
- Classes become loosely coupled.
- Test setup becomes easier.

Mental model:

```text
Class says: "I need a dependency"
Spring says: "I will provide it"
Test says: "I will provide a fake one"
```

Thus, DI improves testability and maintainability in real-world applications.

---

## 19. Demonstrate handling of HTTP GET and POST requests in a student system.

In a student system, `GET` is used to fetch student data, and `POST` is used to create a new student record.

Entity:

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

Controller:

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    private final StudentService studentService;

    public StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    @GetMapping
    public List<Student> getStudents() {
        return studentService.findAll();
    }

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentService.save(student);
    }
}
```

GET request:

```text
GET /students
```

Purpose: Fetch all students.

POST request:

```text
POST /students
Content-Type: application/json
```

Body:

```json
{
  "name": "Amit",
  "email": "amit@example.com"
}
```

Flow:

```text
GET request  -> Controller -> Service -> Repository -> Data returned
POST request -> Controller -> Service -> Repository -> Data saved
```

This demonstrates basic JSON request and response handling in Spring Boot.

---

## 20. Apply Spring Data JPA repository methods to fetch records based on conditions.

Spring Data JPA allows developers to create query methods based on method names. These methods fetch records according to conditions without writing explicit SQL.

Entity:

```java
@Entity
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String category;
    private double price;
}
```

Repository:

```java
@Repository
public interface ProductRepository
        extends JpaRepository<Product, Long> {

    List<Product> findByCategory(String category);

    List<Product> findByPriceLessThan(double price);

    List<Product> findByNameContaining(String keyword);

    List<Product> findByCategoryAndPriceLessThan(
            String category,
            double price
    );
}
```

Controller example:

```java
@GetMapping("/category/{category}")
public List<Product> getByCategory(@PathVariable String category) {
    return productRepository.findByCategory(category);
}
```

Derived query mental model:

```text
findByCategory
   |
   v
SELECT * FROM product WHERE category = ?
```

Examples:

- `findByCategory("Electronics")`: returns electronics products.
- `findByPriceLessThan(1000)`: returns products below 1000.
- `findByNameContaining("phone")`: returns products with phone in name.

Spring Data JPA improves productivity by generating common queries automatically from method names.

---

## 21. Design a REST API for login functionality and explain request-response flow.

A login API accepts user credentials and returns a response indicating whether login was successful. In a basic system, the frontend sends username and password as JSON.

Endpoint design:

```text
POST /api/login
```

Request JSON:

```json
{
  "username": "john",
  "password": "123456"
}
```

Response JSON:

```json
{
  "message": "Login successful",
  "username": "john"
}
```

Request DTO:

```java
public class LoginRequest {
    private String username;
    private String password;
}
```

Controller:

```java
@RestController
@RequestMapping("/api")
public class LoginController {

    private final LoginService loginService;

    public LoginController(LoginService loginService) {
        this.loginService = loginService;
    }

    @PostMapping("/login")
    public String login(@RequestBody LoginRequest request) {
        boolean valid = loginService.validateLogin(
                request.getUsername(),
                request.getPassword()
        );

        if (valid) {
            return "Login successful";
        }

        return "Invalid username or password";
    }
}
```

Flow:

```text
Frontend sends username/password
        |
        v
LoginController receives @RequestBody
        |
        v
LoginService validates credentials
        |
        v
Repository checks database
        |
   +----+----+
   |         |
Valid     Invalid
   |         |
Success   Error response
```

In production, passwords should be securely encoded, and successful login may return a token such as JWT. For Unit 2, the focus is on REST request-response flow and layered handling.

---

## 22. Explain how Hibernate maps Java objects to database tables with an example.

Hibernate is an ORM tool. ORM means Object Relational Mapping. It maps Java classes to database tables and Java objects to table rows.

Mental model:

```text
Java class  -> Database table
Field       -> Table column
Object      -> Table row
```

Entity class:

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

Database table:

```text
student
--------------------------------
id        name        email
1         Amit        amit@example.com
```

Mapping explanation:

- `@Entity` tells Hibernate that the class should be mapped to a table.
- `@Id` marks the primary key.
- `@GeneratedValue` automatically generates id values.
- Class fields become table columns.
- An object of `Student` becomes a row in the `student` table.

Repository:

```java
public interface StudentRepository
        extends JpaRepository<Student, Long> {
}
```

Save operation:

```java
studentRepository.save(student);
```

Hibernate flow:

```text
Java Student object
        |
        v
Hibernate converts object to SQL
        |
        v
INSERT or UPDATE query runs
        |
        v
Data stored in database table
```

Hibernate reduces manual SQL writing and helps developers work with Java objects directly.

---

## 23. Design REST endpoints for a Product API using appropriate mapping annotations.

A Product API should be designed around the product resource. Proper mapping annotations make the API readable and RESTful.

API endpoints:

```text
POST   /products                 -> create product
GET    /products                 -> get all products
GET    /products/{id}            -> get product by id
GET    /products/search?name=pen -> search product by name
PUT    /products/{id}            -> update product
DELETE /products/{id}            -> delete product
```

Controller:

```java
@RestController
@RequestMapping("/products")
public class ProductController {

    @PostMapping
    public Product create(@RequestBody Product product) {
        return productService.save(product);
    }

    @GetMapping
    public List<Product> getAll() {
        return productService.findAll();
    }

    @GetMapping("/{id}")
    public Product getById(@PathVariable Long id) {
        return productService.findById(id);
    }

    @GetMapping("/search")
    public List<Product> search(@RequestParam String name) {
        return productService.searchByName(name);
    }

    @PutMapping("/{id}")
    public Product update(@PathVariable Long id,
                          @RequestBody Product product) {
        return productService.update(id, product);
    }

    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        productService.delete(id);
    }
}
```

Annotation justification:

- `@PostMapping`: creates a new product.
- `@GetMapping`: retrieves products.
- `@PutMapping`: updates an existing product.
- `@DeleteMapping`: deletes a product.
- `@PathVariable`: reads id from URL.
- `@RequestParam`: reads search query value.

This design keeps product operations clear and follows REST standards.

---

## 24. Apply @DeleteMapping in a real-world use case like removing cart items.

`@DeleteMapping` is used to map HTTP DELETE requests. A real-world example is removing an item from a shopping cart.

Endpoint:

```text
DELETE /api/cart/items/{itemId}
```

Controller:

```java
@RestController
@RequestMapping("/api/cart")
public class CartController {

    private final CartService cartService;

    public CartController(CartService cartService) {
        this.cartService = cartService;
    }

    @DeleteMapping("/items/{itemId}")
    public String removeCartItem(@PathVariable Long itemId) {
        cartService.removeItem(itemId);
        return "Item removed from cart";
    }
}
```

Service:

```java
@Service
public class CartService {

    private final CartItemRepository cartItemRepository;

    public CartService(CartItemRepository cartItemRepository) {
        this.cartItemRepository = cartItemRepository;
    }

    public void removeItem(Long itemId) {
        cartItemRepository.deleteById(itemId);
    }
}
```

Flow:

```text
User clicks Remove
       |
       v
DELETE /api/cart/items/5
       |
       v
CartController reads itemId
       |
       v
CartService applies remove logic
       |
       v
Repository deletes item
```

`@DeleteMapping` is suitable here because the client wants to remove an existing resource from the backend.

---

## 25. Design a REST API for order processing system using proper HTTP methods.

An order processing system manages order creation, viewing, updating status, and cancellation.

Order resource:

```text
Order
  id
  customerId
  items
  totalAmount
  status
```

Endpoint design:

| Operation | Method | Endpoint | Purpose |
|---|---|---|---|
| Place order | `POST` | `/api/orders` | Create new order |
| Get all orders | `GET` | `/api/orders` | Fetch all orders |
| Get order by id | `GET` | `/api/orders/{id}` | Fetch one order |
| Get customer orders | `GET` | `/api/customers/{customerId}/orders` | Fetch orders of customer |
| Update order status | `PUT` | `/api/orders/{id}/status` | Update status |
| Cancel order | `DELETE` | `/api/orders/{id}` | Cancel/delete order |

Controller:

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {

    @PostMapping
    public Order placeOrder(@RequestBody Order order) {
        return orderService.placeOrder(order);
    }

    @GetMapping("/{id}")
    public Order getOrder(@PathVariable Long id) {
        return orderService.findById(id);
    }

    @PutMapping("/{id}/status")
    public Order updateStatus(@PathVariable Long id,
                              @RequestBody Order order) {
        return orderService.updateStatus(id, order.getStatus());
    }

    @DeleteMapping("/{id}")
    public void cancelOrder(@PathVariable Long id) {
        orderService.cancelOrder(id);
    }
}
```

Order processing flow:

```text
Cart checkout
     |
     v
POST /orders
     |
     v
Order status = PLACED
     |
     v
Payment/shipping updates status
     |
     v
PUT /orders/{id}/status
```

This design uses HTTP methods correctly and represents orders as REST resources.

---

## 26. Compare and justify when to use SQL-JPA vs MongoDB for E-commerce vs Social Media application.

SQL-JPA and MongoDB are used for different data modeling needs. SQL databases are relational and structured, while MongoDB is document-oriented and flexible.

| Point | SQL with JPA/Hibernate | MongoDB |
|---|---|---|
| Data model | Tables and rows | Documents and collections |
| Schema | Fixed schema | Flexible schema |
| Relationships | Strong support using joins | Embedded or referenced documents |
| Transactions | Strong transaction support | Supports transactions, but document model is primary |
| Best for | Structured relational data | Flexible, changing, nested data |
| Query style | SQL/JPQL/JPA methods | Mongo queries/document queries |

### E-commerce application

An e-commerce system usually has structured data:

- Users
- Products
- Orders
- Payments
- Inventory

These entities have strong relationships. For example, an order belongs to a user and contains products. Payment records must be consistent.

Recommended choice: SQL with JPA/Hibernate.

Reason:

- Strong relationships.
- Transaction safety.
- Consistent order and payment records.
- Structured reporting.
- Better for inventory and billing accuracy.

### Social media application

A social media application may have flexible data:

- Posts
- Comments
- Likes
- User profiles
- Activity feeds

Post content can vary, and documents may contain nested comments, tags, images, or metadata.

Recommended choice: MongoDB for flexible content-heavy parts.

Reason:

- Flexible document structure.
- Good for nested and changing data.
- Easy storage of JSON-like posts and comments.
- Scales well for large document-based data.

Decision mental model:

```text
Need strict relationships and transactions?
        |
     Yes -> SQL + JPA
        |
     No, need flexible JSON documents?
        |
     Yes -> MongoDB
```

Final justification:

- Use SQL-JPA for e-commerce core order, payment, and inventory data.
- Use MongoDB for social media posts, comments, feeds, and flexible user-generated content.

---

## 27. Design a RESTful API for an Online Bookstore with layered architecture. Design Entity, repository interface, service logic, and controller endpoints.

An Online Bookstore API should use layered architecture to separate request handling, business logic, and database access.

Architecture:

```text
BookController
      |
      v
BookService
      |
      v
BookRepository
      |
      v
Database
```

Book entity:

```java
@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String author;
    private double price;
    private int stock;
}
```

Repository:

```java
@Repository
public interface BookRepository
        extends JpaRepository<Book, Long> {

    List<Book> findByAuthor(String author);

    List<Book> findByTitleContaining(String keyword);
}
```

Service:

```java
@Service
public class BookService {

    private final BookRepository bookRepository;

    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public Book addBook(Book book) {
        return bookRepository.save(book);
    }

    public List<Book> getBooks() {
        return bookRepository.findAll();
    }

    public Book getBook(Long id) {
        return bookRepository.findById(id).orElseThrow();
    }

    public void deleteBook(Long id) {
        bookRepository.deleteById(id);
    }
}
```

Controller:

```java
@RestController
@RequestMapping("/api/books")
public class BookController {

    private final BookService bookService;

    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @PostMapping
    public Book addBook(@RequestBody Book book) {
        return bookService.addBook(book);
    }

    @GetMapping
    public List<Book> getBooks() {
        return bookService.getBooks();
    }

    @GetMapping("/{id}")
    public Book getBook(@PathVariable Long id) {
        return bookService.getBook(id);
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        bookService.deleteBook(id);
    }
}
```

Endpoint design:

```text
POST   /api/books      -> add book
GET    /api/books      -> list books
GET    /api/books/{id} -> get book by id
PUT    /api/books/{id} -> update book
DELETE /api/books/{id} -> delete book
```

Mental model:

```text
Controller = API door
Service    = Bookstore rules
Repository = Database access
Entity     = Book table structure
```

This layered design makes the Online Bookstore API clean, testable, and maintainable.

---

## 28. A system is facing performance issues due to improper database queries. Analyze how Spring Data JPA can optimize query handling and suggest improvements using custom queries or indexing.

Improper database queries can slow down an application. Common causes include fetching too much data, missing indexes, repeated queries, inefficient joins, and not filtering data at the database level.

Problem flow:

```text
API request
    |
    v
Repository query
    |
    v
Slow database response
    |
    v
Slow API response
```

Spring Data JPA can optimize query handling in several ways.

### 1. Use derived query methods

Instead of fetching all records and filtering in Java, filter directly in the database.

Wrong approach:

```java
List<Product> products = productRepository.findAll();
// filtering manually in Java
```

Better approach:

```java
List<Product> findByCategory(String category);
```

### 2. Use custom queries

For complex requirements, use `@Query`.

```java
@Query("SELECT p FROM Product p WHERE p.category = :category AND p.price < :price")
List<Product> findAffordableProducts(String category, double price);
```

### 3. Use pagination

Large result sets should be loaded page by page.

```java
Page<Product> findByCategory(String category, Pageable pageable);
```

Usage:

```java
PageRequest pageRequest = PageRequest.of(0, 10);
```

### 4. Use indexing

If searches frequently happen on fields like `email`, `category`, or `status`, indexes should be added in the database.

Example:

```sql
CREATE INDEX idx_product_category ON product(category);
```

### 5. Select only required data

Use DTO projections when the API does not need the full entity.

```java
@Query("SELECT p.name FROM Product p WHERE p.category = :category")
List<String> findProductNamesByCategory(String category);
```

Optimization mental model:

```text
Do not bring all data to Java.
Ask the database for exactly what is needed.
```

Suggested improvements:

- Replace `findAll()` with conditional query methods.
- Add indexes on frequently searched columns.
- Use pagination for large tables.
- Use custom JPQL queries for complex conditions.
- Avoid unnecessary eager loading.
- Check generated SQL using `spring.jpa.show-sql=true`.

These improvements reduce database load and improve API response time.

---

## 29. Use REST APIs Error Handling to a real-world scenario like payment failure or invalid input.

REST API error handling is used to return clear and consistent responses when something goes wrong. Real-world examples include payment failure, invalid input, missing records, and server errors.

Scenario: Payment failure due to invalid amount.

Request:

```json
{
  "orderId": 101,
  "amount": -500
}
```

This input is invalid because amount cannot be negative.

Validation DTO:

```java
public class PaymentRequest {

    @NotNull(message = "Order id is required")
    private Long orderId;

    @Min(value = 1, message = "Amount must be greater than zero")
    private double amount;
}
```

Controller:

```java
@RestController
@RequestMapping("/api/payments")
public class PaymentController {

    @PostMapping
    public String makePayment(@Valid @RequestBody PaymentRequest request) {
        return paymentService.processPayment(request);
    }
}
```

Global error handler:

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<String> handleValidationError(
            MethodArgumentNotValidException ex) {

        return ResponseEntity
                .badRequest()
                .body("Invalid payment input");
    }

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<String> handleRuntimeError(RuntimeException ex) {
        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("Payment processing failed");
    }
}
```

Error flow:

```text
Invalid payment request
        |
        v
@Valid checks request
        |
        v
Validation fails
        |
        v
@ControllerAdvice handles exception
        |
        v
400 Bad Request response
```

Common status codes:

- `400 Bad Request`: invalid input.
- `404 Not Found`: order or payment record not found.
- `500 Internal Server Error`: unexpected backend failure.

Good error response example:

```json
{
  "status": 400,
  "message": "Amount must be greater than zero",
  "path": "/api/payments"
}
```

Proper error handling improves user experience, debugging, and API reliability.

