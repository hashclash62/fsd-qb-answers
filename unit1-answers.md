# Unit 1 Answers: Introduction to Full Stack Development

Subject: Full Stack Development  
Unit: Introduction to Full Stack Development

---

## 1. Describe the MVC architecture with roles of Model, View, and Controller.

MVC stands for Model-View-Controller. It is an architectural pattern used to separate an application into three main parts: Model, View, and Controller. This separation makes the application easier to understand, develop, test, and maintain.

```text
User
 |
 v
Controller
 |
 +----> Model
 |
 v
View
 |
 v
Response to user
```

### Model

The Model represents the data and business logic of the application. It contains classes that store data and may interact with the database.

Example:

```java
public class Student {
    private Long id;
    private String name;
    private String email;
}
```

### View

The View is responsible for displaying data to the user. In traditional web applications, views may be HTML, JSP, Thymeleaf, or frontend pages. In REST APIs, the view is often replaced by JSON response data.

### Controller

The Controller handles incoming user requests. It accepts input, calls the service or model layer, and returns a response.

Example:

```java
@RestController
public class StudentController {

    @GetMapping("/students")
    public List<Student> getStudents() {
        return studentService.getAllStudents();
    }
}
```

Benefits of MVC:

- Separates responsibilities clearly.
- Makes code easier to maintain.
- Supports parallel development.
- Improves testing because each layer can be tested separately.
- Reduces mixing of UI, business logic, and request handling.

---

## 2. Explain key features of Spring Boot.

Spring Boot is a framework built on top of the Spring Framework. It simplifies the development of Java applications by reducing configuration and providing ready-to-use defaults.

Key features of Spring Boot:

### Auto-configuration

Spring Boot automatically configures application components based on the dependencies present in the project. For example, if `spring-boot-starter-web` is added, Spring Boot configures web-related components automatically.

### Starter dependencies

Spring Boot provides starter dependencies such as:

- `spring-boot-starter-web`
- `spring-boot-starter-data-jpa`
- `spring-boot-starter-test`

These starters group commonly used dependencies together.

### Embedded server

Spring Boot includes embedded servers such as Tomcat, Jetty, or Undertow. Because of this, developers do not need to deploy applications manually on an external server.

```text
Spring Boot app
      |
      v
Embedded Tomcat
      |
      v
Runs as standalone application
```

### Production-ready features

Spring Boot supports logging, health checks, metrics, and monitoring through Spring Boot Actuator.

### Less boilerplate configuration

Spring Boot reduces XML configuration and uses annotations and sensible defaults.

### Easy REST API development

Using annotations such as `@RestController`, `@GetMapping`, and `@PostMapping`, REST APIs can be created quickly.

Overall, Spring Boot makes Java backend development faster, simpler, and more suitable for REST APIs and microservices.

---

## 3. Describe the structure of a Spring Boot project.

A Spring Boot project follows a standard structure. This structure helps organize source code, resources, configuration files, and tests.

Typical project structure:

```text
student-app/
 |
 +-- pom.xml
 |
 +-- src/
     |
     +-- main/
     |   |
     |   +-- java/
     |   |   |
     |   |   +-- com/example/studentapp/
     |   |       |
     |   |       +-- StudentAppApplication.java
     |   |       +-- controller/
     |   |       +-- service/
     |   |       +-- repository/
     |   |       +-- model/
     |   |
     |   +-- resources/
     |       |
     |       +-- application.properties
     |
     +-- test/
         |
         +-- java/
```

Important parts:

### `pom.xml`

This file contains project information, dependencies, plugins, and build configuration for Maven.

### `src/main/java`

This folder contains Java source code. Controllers, services, repositories, and model classes are placed here.

### Main application class

This class starts the Spring Boot application.

```java
@SpringBootApplication
public class StudentAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(StudentAppApplication.class, args);
    }
}
```

### `src/main/resources`

This folder contains configuration files such as `application.properties`.

### `src/test/java`

This folder contains test classes.

Good project structure improves readability and separates responsibilities in a full stack backend application.

---

## 4. Explain REST principles.

REST stands for Representational State Transfer. It is an architectural style used to design web services. REST APIs allow frontend applications, mobile apps, and other clients to communicate with backend servers using HTTP.

Main REST principles:

### Client-server architecture

The frontend and backend are separated. The client sends requests, and the server sends responses.

```text
Frontend Client
      |
      v
REST API Request
      |
      v
Spring Boot Backend
```

### Stateless communication

Each request should contain all information needed to process it. The server should not depend on previous requests to understand the current one.

### Resource-based URLs

REST APIs treat data as resources. Each resource is identified by a URL.

Examples:

```text
/students
/students/1
/products
/orders/10
```

### Use of HTTP methods

REST uses HTTP methods to perform actions:

- `GET` to read data.
- `POST` to create data.
- `PUT` to update data.
- `DELETE` to remove data.

### Representation using JSON

REST APIs commonly send and receive data in JSON format.

```json
{
  "id": 1,
  "name": "Amit"
}
```

### Uniform interface

REST APIs should follow consistent naming, status codes, and request-response formats.

REST principles make APIs simple, scalable, and easy to consume from frontend applications.

---

## 5. Describe CRUD operations using HTTP methods.

CRUD stands for Create, Read, Update, and Delete. These are the four basic operations performed on data in most applications. In REST APIs, CRUD operations are mapped to HTTP methods.

| CRUD Operation | HTTP Method | Example URL | Purpose |
|---|---|---|---|
| Create | `POST` | `/students` | Add new student |
| Read | `GET` | `/students` or `/students/1` | Fetch student data |
| Update | `PUT` | `/students/1` | Update existing student |
| Delete | `DELETE` | `/students/1` | Remove student |

Example Spring Boot controller:

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentService.save(student);
    }

    @GetMapping
    public List<Student> getStudents() {
        return studentService.findAll();
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

Flow:

```text
Frontend action
      |
      v
HTTP method
      |
      v
Spring Boot REST endpoint
      |
      v
Database operation
```

Using proper HTTP methods makes REST API design clear and standard.

---

## 6. Describe Microservices architecture.

Microservices architecture is a software design approach where a large application is divided into small, independent services. Each service handles one specific business function and can be developed, deployed, and scaled separately.

Example e-commerce microservices:

```text
E-commerce Application
      |
      +-- User Service
      +-- Product Service
      +-- Cart Service
      +-- Order Service
      +-- Payment Service
```

Each microservice usually has:

- Its own business logic.
- Its own database or data storage.
- Its own REST APIs.
- Independent deployment.

Communication flow:

```text
Frontend
   |
   v
API Gateway
   |
   +-- User Service
   +-- Product Service
   +-- Order Service
```

Advantages:

- Services can be developed independently.
- Failure in one service does not always stop the whole system.
- Each service can be scaled separately.
- Different teams can work on different services.
- Suitable for large and complex applications.

Disadvantages:

- More complex deployment.
- Network communication is required between services.
- Testing and debugging become harder.
- Data consistency can be challenging.

In Spring Boot, microservices are commonly built as separate REST API applications.

---

## 7. Explain steps to create project using Spring Initializer and structure of POM.xml.

Spring Initializr is an online tool used to quickly generate a Spring Boot project with selected dependencies.

Steps to create a project using Spring Initializr:

1. Open `https://start.spring.io`.
2. Select project type as Maven.
3. Select language as Java.
4. Choose Spring Boot version.
5. Enter project metadata such as group, artifact, name, and package name.
6. Select packaging as JAR.
7. Select Java version.
8. Add dependencies such as Spring Web, Spring Data JPA, or MySQL Driver.
9. Click Generate.
10. Extract the downloaded project and open it in an IDE.
11. Run the main Spring Boot application class.

Project creation flow:

```text
Spring Initializr
      |
      v
Select metadata and dependencies
      |
      v
Generate ZIP
      |
      v
Open project in IDE
      |
      v
Run Spring Boot application
```

Basic `pom.xml` structure:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>student-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

Important parts of `pom.xml`:

- `groupId`: organization or package name.
- `artifactId`: project name.
- `version`: project version.
- `parent`: Spring Boot parent configuration.
- `dependencies`: required libraries.
- `plugins`: build and execution tools.

---

## 8. Describe Maven repositories and Explain the phases of Maven build lifecycle.

Maven repositories are locations where Maven stores and downloads project dependencies. When a dependency is added in `pom.xml`, Maven searches repositories to find that dependency.

Types of Maven repositories:

### Local repository

The local repository is present on the developer's machine. It stores downloaded dependencies.

Example location:

```text
~/.m2/repository
```

### Central repository

The central repository is an online repository maintained by the Maven community. Maven downloads most public dependencies from here.

### Remote repository

A remote repository is a custom repository provided by an organization or third party.

Repository flow:

```text
Maven checks local repository
        |
        v
If not found, checks central/remote repository
        |
        v
Downloads dependency
        |
        v
Stores it in local repository
```

Maven build lifecycle phases:

The default Maven lifecycle contains important phases such as:

- `validate`: checks whether the project is correct.
- `compile`: compiles source code.
- `test`: runs unit tests.
- `package`: packages code into JAR or WAR.
- `verify`: verifies the package.
- `install`: installs package into local repository.
- `deploy`: copies package to remote repository.

Example command:

```bash
mvn clean install
```

This cleans old build files and builds the project, runs tests, packages it, and installs it into the local repository.

---

## 9. Illustrate the different types of spring context used in springboot applications.

Spring context is the container that manages beans in a Spring or Spring Boot application. It creates objects, wires dependencies, and manages the lifecycle of beans.

Common Spring contexts used in Spring Boot:

### ApplicationContext

`ApplicationContext` is the main Spring container. It manages beans, dependency injection, configuration, and application events.

### ConfigurableApplicationContext

This is a sub-interface of `ApplicationContext`. It allows the context to be refreshed and closed. Spring Boot's `SpringApplication.run()` returns a `ConfigurableApplicationContext`.

```java
ConfigurableApplicationContext context =
        SpringApplication.run(DemoApplication.class, args);
```

### WebApplicationContext

This context is used in web applications. It contains web-related beans such as controllers, filters, and web configurations.

### ServletWebServerApplicationContext

In Spring Boot web applications, this context starts and manages the embedded servlet web server such as Tomcat.

Context flow:

```text
SpringApplication.run()
        |
        v
Creates ApplicationContext
        |
        v
Scans components and creates beans
        |
        v
Starts embedded server for web app
```

Importance of Spring context:

- Creates and manages beans.
- Performs dependency injection.
- Reads configuration.
- Manages application lifecycle.
- Supports web application startup.

Thus, Spring context is the core container behind a Spring Boot application.

---

## 10. Illustrate the steps to create a bean in springboot application.

A bean is an object managed by the Spring container. In Spring Boot, beans can be created using annotations such as `@Component`, `@Service`, `@Repository`, `@Controller`, or using `@Bean` inside a configuration class.

### Method 1: Using component annotation

Step 1: Create a class.

```java
import org.springframework.stereotype.Service;

@Service
public class GreetingService {

    public String getMessage() {
        return "Welcome to Spring Boot";
    }
}
```

Step 2: Inject the bean into another class.

```java
@RestController
public class GreetingController {

    private final GreetingService greetingService;

    public GreetingController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    @GetMapping("/welcome")
    public String welcome() {
        return greetingService.getMessage();
    }
}
```

Bean creation flow:

```text
Class marked with @Service
        |
        v
Component scanning finds class
        |
        v
Spring creates bean
        |
        v
Bean is injected where required
```

### Method 2: Using @Bean

```java
@Configuration
public class AppConfig {

    @Bean
    public GreetingService greetingService() {
        return new GreetingService();
    }
}
```

Important points:

- `@Component` is a generic stereotype annotation.
- `@Service` is used for service layer classes.
- `@Repository` is used for data access classes.
- `@Controller` and `@RestController` are used for web controllers.
- `@Bean` is used when bean creation logic is written manually.

---

## 11. Explain use of @RestController and @GetMapping.

`@RestController` and `@GetMapping` are commonly used annotations in Spring Boot REST API development.

### @RestController

`@RestController` marks a class as a REST controller. It tells Spring that the class will handle HTTP requests and return data directly as the response body.

It is a combination of:

```text
@Controller + @ResponseBody
```

Example:

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello Spring Boot";
    }
}
```

### @GetMapping

`@GetMapping` maps HTTP GET requests to a specific method. It is used when the client wants to read or retrieve data.

Example:

```java
@GetMapping("/students")
public List<Student> getStudents() {
    return studentService.getAllStudents();
}
```

Request flow:

```text
GET /hello
   |
   v
Spring finds @GetMapping("/hello")
   |
   v
Method executes
   |
   v
Response returned to client
```

Together, `@RestController` and `@GetMapping` are used to create simple REST endpoints in Spring Boot.

---

## 12. Illustrate the role of @componentscan in springboot configuration.

`@ComponentScan` tells Spring where to search for classes marked with annotations such as `@Component`, `@Service`, `@Repository`, and `@Controller`. These classes are then registered as Spring beans.

In Spring Boot, `@SpringBootApplication` includes `@ComponentScan` internally.

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

This is equivalent to using:

```text
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

Component scanning flow:

```text
Application starts
       |
       v
@ComponentScan scans package
       |
       v
Finds @Controller, @Service, @Repository
       |
       v
Creates Spring beans
       |
       v
Beans become available for dependency injection
```

Example:

```java
@Service
public class StudentService {
}
```

If `StudentService` is inside the scanned package, Spring automatically creates its bean.

Important point: By default, Spring Boot scans the package where the main application class is present and its sub-packages. Therefore, controller, service, and repository classes should usually be placed under the main package.

---

## 13. Explain the request-response flow in MVC architecture.

In MVC architecture, a request moves through the Controller, Model, and View layers before a response is sent back to the client.

Basic MVC flow:

```text
Client sends request
       |
       v
Controller receives request
       |
       v
Controller calls Model/Service
       |
       v
Model processes data
       |
       v
View or response is prepared
       |
       v
Client receives response
```

In a Spring MVC or Spring Boot REST application, the flow is:

1. Client sends an HTTP request.
2. `DispatcherServlet` receives the request.
3. The request is mapped to the correct controller method.
4. Controller calls service layer for business logic.
5. Service may call repository layer for database access.
6. Data is returned to the controller.
7. Controller returns a view or JSON response.
8. Client receives the response.

REST API example:

```java
@GetMapping("/students/{id}")
public Student getStudent(@PathVariable Long id) {
    return studentService.getStudentById(id);
}
```

REST response flow:

```text
GET /students/1
      |
      v
Controller
      |
      v
Service
      |
      v
Repository
      |
      v
JSON response
```

This structure separates request handling, business logic, and data access.

---

## 14. Illustrate the role of DispatcherServlet in Spring MVC.

`DispatcherServlet` is the front controller in Spring MVC. It receives all incoming HTTP requests and dispatches them to the correct controller method.

Role of `DispatcherServlet`:

- Receives incoming requests.
- Finds the correct controller using handler mappings.
- Calls the controller method.
- Processes the returned response.
- Sends the final response back to the client.

Flow:

```text
Client request
      |
      v
DispatcherServlet
      |
      v
HandlerMapping finds controller
      |
      v
Controller method executes
      |
      v
Model/View or JSON response returned
      |
      v
DispatcherServlet sends response
```

Example:

```java
@RestController
public class StudentController {

    @GetMapping("/students")
    public List<Student> getStudents() {
        return studentService.getAllStudents();
    }
}
```

When a client sends `GET /students`, the `DispatcherServlet` receives the request and routes it to the `getStudents()` method.

In Spring Boot, `DispatcherServlet` is automatically configured when `spring-boot-starter-web` is added. Developers normally do not need to configure it manually.

It is important because it centralizes request handling and makes Spring MVC work in an organized way.

---

## 15. Discuss auto-configuration in Spring Boot.

Auto-configuration is one of the most important features of Spring Boot. It automatically configures Spring application components based on the dependencies and settings available in the project.

For example, if `spring-boot-starter-web` is added, Spring Boot automatically configures:

- Embedded Tomcat server.
- Spring MVC.
- `DispatcherServlet`.
- JSON conversion.
- REST controller support.

Auto-configuration flow:

```text
Application starts
      |
      v
Spring Boot checks classpath dependencies
      |
      v
Checks configuration properties
      |
      v
Applies suitable auto-configuration
      |
      v
Application runs with default setup
```

Main annotation:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

`@SpringBootApplication` internally includes `@EnableAutoConfiguration`.

Benefits:

- Reduces manual configuration.
- Speeds up project setup.
- Provides sensible defaults.
- Makes REST API development easier.
- Reduces XML configuration.

Example: If a database dependency is added, Spring Boot tries to configure a datasource using values from `application.properties`.

Auto-configuration can also be customized or overridden by defining your own beans or properties.

---

## 16. Differentiate between Spring and Spring Boot.

Spring is a powerful Java framework used for building enterprise applications. Spring Boot is built on top of Spring and simplifies Spring application development by providing auto-configuration, starter dependencies, and embedded servers.

| Point | Spring | Spring Boot |
|---|---|---|
| Meaning | Java framework for enterprise applications | Framework that simplifies Spring development |
| Configuration | Requires more manual configuration | Provides auto-configuration |
| Server | Usually needs external server setup | Provides embedded server |
| Dependencies | Dependencies are added individually | Starter dependencies group common libraries |
| Setup time | More setup required | Faster project setup |
| XML configuration | Often used in older Spring apps | Mostly annotation and property based |
| Main purpose | Provides core features like DI, MVC, AOP | Makes production-ready Spring apps quickly |

Spring example may need more configuration for MVC, server deployment, and dependencies.

Spring Boot example:

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

Comparison:

```text
Spring
  |
  +-- Flexible but more configuration

Spring Boot
  |
  +-- Spring + auto-configuration + embedded server + starters
```

Spring Boot does not replace Spring. It makes Spring easier and faster to use.

---

## 17. Explain idempotent HTTP methods with examples.

An HTTP method is called idempotent if making the same request multiple times has the same effect on the server as making it once.

Example:

```text
Same request repeated many times
        |
        v
Final server state remains same
```

### GET

`GET` is idempotent because it only reads data and does not change server state.

```text
GET /students/1
```

Calling this many times returns the same student data, assuming data is not changed by another operation.

### PUT

`PUT` is idempotent because it updates a resource to a specific state.

```text
PUT /students/1
Body: {"name": "Amit"}
```

Sending this request multiple times keeps the student's name as `Amit`.

### DELETE

`DELETE` is idempotent because deleting the same resource multiple times results in the resource being deleted.

```text
DELETE /students/1
```

After the first request, the student is deleted. Repeating the request does not create a new change.

### POST

`POST` is generally not idempotent because repeating it may create multiple records.

```text
POST /students
Body: {"name": "Amit"}
```

Calling this multiple times may create multiple students.

Summary:

| Method | Idempotent? | Reason |
|---|---|---|
| GET | Yes | Reads data only |
| PUT | Yes | Sets resource to same state |
| DELETE | Yes | Resource remains deleted |
| POST | No | May create duplicate data |

---

## 18. Design basic REST API endpoints for Student system.

A Student system can expose REST API endpoints for performing CRUD operations on student data.

Basic resource:

```text
Student
  id
  name
  email
  course
```

REST API design:

| Operation | HTTP Method | Endpoint | Purpose |
|---|---|---|---|
| Create student | `POST` | `/api/students` | Add a new student |
| Get all students | `GET` | `/api/students` | Fetch all students |
| Get student by id | `GET` | `/api/students/{id}` | Fetch one student |
| Update student | `PUT` | `/api/students/{id}` | Update existing student |
| Delete student | `DELETE` | `/api/students/{id}` | Remove student |

Controller design:

```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentService.save(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return studentService.findAll();
    }

    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) {
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

This endpoint design follows REST principles by using nouns in URLs and HTTP methods for actions.

---

## 19. Differentiate between Monolithic and Microservices architecture.

Monolithic and microservices are two different application architecture styles.

In monolithic architecture, the entire application is built as one single unit. All modules such as user, product, order, and payment are part of the same codebase and deployment.

In microservices architecture, the application is divided into small independent services. Each service handles a specific business function.

| Point | Monolithic Architecture | Microservices Architecture |
|---|---|---|
| Structure | Single application | Multiple small services |
| Deployment | Deployed as one unit | Each service deployed separately |
| Database | Usually one shared database | Each service may have its own database |
| Scaling | Entire app is scaled | Individual services can be scaled |
| Development | Simple for small apps | Better for large complex apps |
| Failure impact | One failure can affect whole app | Failure may be limited to one service |
| Complexity | Easier initially | More complex due to network and deployment |

Diagram:

```text
Monolithic:

Single Application
  +-- User
  +-- Product
  +-- Order
  +-- Payment

Microservices:

User Service      Product Service
Order Service     Payment Service
```

Monolithic architecture is suitable for small applications or simple projects. Microservices are suitable for large systems where independent development, deployment, and scaling are needed.

---

## 20. Differentiate between Controller and RestController

`@Controller` and `@RestController` are Spring annotations used to create web controllers, but they are used for different types of responses.

| Point | @Controller | @RestController |
|---|---|---|
| Purpose | Used for MVC web applications | Used for REST APIs |
| Response | Usually returns view name | Returns data directly |
| View technology | JSP, Thymeleaf, HTML | JSON, XML, plain text |
| @ResponseBody | Needed to return data | Included by default |
| Common use | Web pages | Backend API endpoints |

`@Controller` example:

```java
@Controller
public class PageController {

    @GetMapping("/home")
    public String home() {
        return "home";
    }
}
```

Here, `"home"` may refer to a view page.

`@RestController` example:

```java
@RestController
public class ApiController {

    @GetMapping("/message")
    public String message() {
        return "Hello API";
    }
}
```

Here, `"Hello API"` is returned directly in the HTTP response body.

Relationship:

```text
@RestController = @Controller + @ResponseBody
```

For REST API development in Spring Boot, `@RestController` is commonly preferred.

---

## 21. Explain dependency management in Maven.

Dependency management in Maven is the process of declaring, downloading, and controlling external libraries required by a project. Dependencies are defined in the `pom.xml` file.

Example:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

When Maven sees this dependency, it downloads the required library and its related transitive dependencies from Maven repositories.

Dependency resolution flow:

```text
Dependency added in pom.xml
        |
        v
Maven checks local repository
        |
        v
If missing, downloads from central repository
        |
        v
Adds dependency to project classpath
```

Important concepts:

### Direct dependency

A dependency directly added in `pom.xml`.

### Transitive dependency

A dependency required by another dependency. Maven downloads it automatically.

### Dependency version

The version decides which release of a library is used.

### Dependency management section

`<dependencyManagement>` is used to control versions centrally, especially in multi-module projects.

Spring Boot simplifies dependency management using `spring-boot-starter-parent`, which provides compatible dependency versions.

Benefits:

- Reduces manual JAR handling.
- Automatically downloads dependencies.
- Manages transitive dependencies.
- Helps avoid version mismatch.
- Makes builds consistent.

---

## 22. Explain role of plugins in Maven.

Maven plugins are used to perform build-related tasks. Maven itself is based on plugins. Tasks such as compiling code, running tests, packaging JAR files, and running Spring Boot applications are handled by plugins.

Common Maven plugins:

- `maven-compiler-plugin`: compiles Java source code.
- `maven-surefire-plugin`: runs unit tests.
- `maven-jar-plugin`: creates JAR files.
- `spring-boot-maven-plugin`: packages and runs Spring Boot applications.

Example:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

Plugin flow:

```text
Maven command
     |
     v
Lifecycle phase
     |
     v
Plugin goal executes
     |
     v
Build task completed
```

Example command:

```bash
mvn spring-boot:run
```

This uses the Spring Boot Maven plugin to run the application.

Roles of plugins:

- Compile source code.
- Run test cases.
- Create packages.
- Generate reports.
- Run the application.
- Clean build directories.

Plugins make Maven flexible and allow the build process to be customized.

---

## 23. Differentiate between clean, default, and site lifecycle.

Maven has three main built-in lifecycles: clean, default, and site. Each lifecycle contains phases that perform different tasks.

| Lifecycle | Purpose | Common Phases |
|---|---|---|
| clean | Removes previous build output | `pre-clean`, `clean`, `post-clean` |
| default | Builds and deploys the project | `validate`, `compile`, `test`, `package`, `install`, `deploy` |
| site | Generates project documentation | `pre-site`, `site`, `post-site`, `site-deploy` |

### Clean lifecycle

The clean lifecycle removes files generated by previous builds.

Example:

```bash
mvn clean
```

This deletes the `target` folder.

### Default lifecycle

The default lifecycle is the main build lifecycle. It compiles code, runs tests, packages the application, and installs or deploys it.

Example:

```bash
mvn package
```

This compiles, tests, and creates a JAR or WAR file.

### Site lifecycle

The site lifecycle generates project documentation and reports.

Example:

```bash
mvn site
```

Lifecycle relationship:

```text
clean   -> remove old build files
default -> build and package project
site    -> generate documentation
```

These lifecycles help standardize project building and reporting in Maven.

---

## 24. Describe the components of the Postman interface (request URL, headers, body, response)

Postman is a tool used to test APIs. It allows developers to send HTTP requests and inspect backend responses without writing frontend code.

Main components of the Postman interface:

### Request URL

The request URL is the API endpoint to be tested.

Example:

```text
http://localhost:8080/api/students
```

### HTTP method

Postman allows selecting methods such as:

- `GET`
- `POST`
- `PUT`
- `DELETE`

### Headers

Headers provide extra information about the request. For JSON requests, a common header is:

```text
Content-Type: application/json
```

### Body

The body contains data sent with requests such as `POST` or `PUT`.

Example JSON body:

```json
{
  "name": "Amit",
  "email": "amit@example.com"
}
```

### Response

The response section shows:

- HTTP status code.
- Response body.
- Response headers.
- Response time.

Postman request-response flow:

```text
Enter URL and method
      |
      v
Add headers/body if needed
      |
      v
Click Send
      |
      v
View response status and body
```

Postman is useful for testing Spring Boot REST APIs before connecting them to a frontend.

---

## 25. Given a scenario where multiple dependencies use different versions of the same library, explain how Maven resolves the conflict

When multiple dependencies use different versions of the same library, Maven must decide which version should be used. This situation is called dependency version conflict.

Example:

```text
Project
 |
 +-- Dependency A -> Library X version 1.0
 |
 +-- Dependency B -> Library X version 2.0
```

Maven resolves conflicts using dependency mediation.

### Nearest definition rule

Maven chooses the version that is nearest to the project in the dependency tree.

Example:

```text
Project -> A -> X 1.0
Project -> B -> C -> X 2.0
```

Here, `X 1.0` is nearer, so Maven selects `X 1.0`.

### First declaration rule

If two versions are at the same depth, Maven chooses the version that appears first in the `pom.xml`.

### dependencyManagement

Developers can force a specific version using `<dependencyManagement>`.

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>library-x</artifactId>
            <version>2.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Conflict resolution flow:

```text
Multiple versions found
        |
        v
Maven checks nearest dependency
        |
        v
If same depth, first declaration wins
        |
        v
dependencyManagement can override version
```

Command to inspect dependency tree:

```bash
mvn dependency:tree
```

This helps identify which dependency version is finally used.

---

## 26. Differentiate between GET, POST, PUT, and DELETE methods in Postman

In Postman, `GET`, `POST`, `PUT`, and `DELETE` are HTTP methods used to test REST API operations.

| Method | Purpose | Request Body | Example |
|---|---|---|---|
| GET | Retrieve data | Usually no body | Get all students |
| POST | Create new data | Has body | Add new student |
| PUT | Update existing data | Has body | Update student by id |
| DELETE | Remove data | Usually no body | Delete student by id |

### GET

Used to fetch data from the backend.

```text
GET http://localhost:8080/api/students
```

### POST

Used to create a new resource.

```text
POST http://localhost:8080/api/students
```

Body:

```json
{
  "name": "Amit",
  "email": "amit@example.com"
}
```

### PUT

Used to update an existing resource.

```text
PUT http://localhost:8080/api/students/1
```

Body:

```json
{
  "name": "Amit Sharma",
  "email": "amit@example.com"
}
```

### DELETE

Used to delete a resource.

```text
DELETE http://localhost:8080/api/students/1
```

Postman testing flow:

```text
Select method
      |
      v
Enter URL
      |
      v
Add headers/body if needed
      |
      v
Click Send
      |
      v
Check response
```

Using the correct HTTP method is important for proper REST API testing.

---

## 27. After running the Spring Boot application, API responses are not visible in Postman. Break down the steps to identify where the failure might be occurring.

If API responses are not visible in Postman after running a Spring Boot application, the failure can occur at different points such as application startup, URL, port, controller mapping, HTTP method, or backend exception.

Debugging steps:

### 1. Check whether the application started successfully

Look at the console logs and confirm that the application started without errors.

```text
Started DemoApplication in ...
Tomcat started on port 8080
```

### 2. Check the port number

Spring Boot usually runs on port `8080`. If the port is changed in `application.properties`, use the correct port in Postman.

```properties
server.port=8081
```

Then the URL should be:

```text
http://localhost:8081/api/students
```

### 3. Check the request URL

Verify that the Postman URL matches the controller mapping exactly.

```java
@RequestMapping("/api/students")
```

### 4. Check HTTP method

If the controller uses `@GetMapping`, Postman should use `GET`. If it uses `@PostMapping`, Postman should use `POST`.

### 5. Check request body and headers

For `POST` and `PUT`, add:

```text
Content-Type: application/json
```

### 6. Check controller package location

Controller classes should be inside the main application package or its sub-packages so that component scanning can detect them.

### 7. Check console errors

Backend exceptions are usually printed in the console. These errors help identify mapping, database, or code issues.

Debugging flow:

```text
No response in Postman
        |
        v
Check app running
        |
        v
Check port and URL
        |
        v
Check HTTP method
        |
        v
Check controller mapping
        |
        v
Check headers/body
        |
        v
Check backend logs
```

This step-by-step approach helps locate the exact failure point.

---

## 28. Create a basic Spring Boot project using Spring Initializer and print a welcome message using @RestController.

To create a basic Spring Boot project, use Spring Initializr and add the Spring Web dependency.

Steps:

1. Open `https://start.spring.io`.
2. Select Maven project and Java language.
3. Enter group id such as `com.example`.
4. Enter artifact id such as `demo`.
5. Add dependency: Spring Web.
6. Generate and download the project.
7. Extract and open the project in an IDE.
8. Create a REST controller.
9. Run the main application class.

Controller code:

```java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class WelcomeController {

    @GetMapping("/welcome")
    public String welcome() {
        return "Welcome to Spring Boot";
    }
}
```

Main class:

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

Test in browser or Postman:

```text
GET http://localhost:8080/welcome
```

Expected response:

```text
Welcome to Spring Boot
```

Flow:

```text
GET /welcome
      |
      v
WelcomeController
      |
      v
welcome() method
      |
      v
Text response
```

---

## 29. Write code to create a REST endpoint using @GetMapping to return student details.

`@GetMapping` is used to create a REST endpoint that handles HTTP GET requests. The following example returns student details as JSON.

Student class:

```java
public class Student {
    private Long id;
    private String name;
    private String email;

    public Student(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }
}
```

Controller:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class StudentController {

    @GetMapping("/student")
    public Student getStudent() {
        return new Student(1L, "Amit", "amit@example.com");
    }
}
```

Test URL:

```text
GET http://localhost:8080/student
```

Expected JSON response:

```json
{
  "id": 1,
  "name": "Amit",
  "email": "amit@example.com"
}
```

Flow:

```text
Client sends GET /student
        |
        v
StudentController method executes
        |
        v
Student object returned
        |
        v
Spring converts object to JSON
```

Spring Boot automatically converts Java objects into JSON using Jackson.

---

## 30. You are given two REST API designs: one using proper HTTP methods (GET, POST) and another using only GET. Evaluate which design is better and why.

The REST API design using proper HTTP methods such as `GET`, `POST`, `PUT`, and `DELETE` is better than a design that uses only `GET`.

Good REST design:

```text
GET    /students       -> fetch students
POST   /students       -> create student
PUT    /students/1     -> update student
DELETE /students/1     -> delete student
```

Poor REST design:

```text
GET /getStudents
GET /createStudent?name=Amit
GET /updateStudent?id=1
GET /deleteStudent?id=1
```

Reasons proper HTTP methods are better:

### Clear meaning

Each method has a standard purpose. `GET` reads data, while `POST` creates data.

### Safety

`GET` should not change server data. If `GET` is used for create or delete operations, accidental clicks or browser refreshes may modify data.

### REST standard compliance

REST APIs should use HTTP methods correctly. This makes APIs easier for developers to understand and use.

### Better testing in Postman

Postman clearly supports different methods, headers, and request bodies.

### Better caching behavior

`GET` requests may be cached by browsers or proxies. Using `GET` for data-changing operations can cause unexpected behavior.

Comparison:

```text
Proper design:
Action is represented by HTTP method

Only GET design:
Action is hidden in URL name
```

Therefore, the design using proper HTTP methods is better because it is clearer, safer, standard, and easier to maintain.

---

## 31. Write a REST API to accept a name as input using @RequestParam and return greeting.

`@RequestParam` is used to read query parameters from the request URL. It is useful when small input values are passed in the URL.

Example URL:

```text
http://localhost:8080/greet?name=Amit
```

Controller code:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    @GetMapping("/greet")
    public String greet(@RequestParam String name) {
        return "Hello, " + name + "!";
    }
}
```

Request:

```text
GET /greet?name=Amit
```

Response:

```text
Hello, Amit!
```

Flow:

```text
Client sends /greet?name=Amit
        |
        v
@RequestParam reads name
        |
        v
Controller prepares greeting
        |
        v
Response returned
```

Optional default value:

```java
@GetMapping("/greet")
public String greet(@RequestParam(defaultValue = "Guest") String name) {
    return "Hello, " + name + "!";
}
```

Here, if the name is not passed, the response will be `Hello, Guest!`.

---

## 32. Write code to read value from application.properties file.

In Spring Boot, `application.properties` is used to store configuration values such as application name, server port, database URL, or custom messages. These values can be read using the `@Value` annotation.

`application.properties`:

```properties
app.message=Welcome to Full Stack Development
app.college=PCCOE
```

Controller code:

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ConfigController {

    @Value("${app.message}")
    private String message;

    @Value("${app.college}")
    private String college;

    @GetMapping("/config")
    public String getConfig() {
        return message + " - " + college;
    }
}
```

Test URL:

```text
GET http://localhost:8080/config
```

Expected response:

```text
Welcome to Full Stack Development - PCCOE
```

Flow:

```text
application.properties
        |
        v
@Value reads property
        |
        v
Value injected into field
        |
        v
Controller returns value
```

Another common approach for multiple related properties is `@ConfigurationProperties`, but for simple values, `@Value` is easy and suitable.

---

## 33. Write a Maven pom.xml with multiple dependencies and explain their usage.

The `pom.xml` file is the Project Object Model file used by Maven. It contains project metadata, dependencies, plugins, and build configuration.

Example `pom.xml`:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>student-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>student-app</name>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

Dependency usage:

- `spring-boot-starter-web`: used to create REST APIs and web applications.
- `spring-boot-starter-data-jpa`: used for database operations using JPA.
- `mysql-connector-j`: connects Spring Boot application to MySQL database.
- `spring-boot-starter-test`: provides testing tools such as JUnit and Mockito.
- `spring-boot-maven-plugin`: helps package and run the Spring Boot application.

Maven flow:

```text
pom.xml dependencies
        |
        v
Maven downloads libraries
        |
        v
Libraries added to classpath
        |
        v
Spring Boot application builds and runs
```

This `pom.xml` is suitable for a basic Spring Boot REST API project with database support and testing.

---

## 34. Create a REST API using @PathVariable to fetch data based on ID.

`@PathVariable` is used to read values from the URL path. It is commonly used when fetching a resource by id.

Example URL:

```text
GET http://localhost:8080/students/1
```

Student class:

```java
public class Student {
    private Long id;
    private String name;

    public Student(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Controller:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class StudentController {

    @GetMapping("/students/{id}")
    public Student getStudentById(@PathVariable Long id) {
        return new Student(id, "Amit");
    }
}
```

Request:

```text
GET /students/1
```

Response:

```json
{
  "id": 1,
  "name": "Amit"
}
```

Flow:

```text
Client sends /students/1
        |
        v
@PathVariable reads id = 1
        |
        v
Controller fetches student by id
        |
        v
Student returned as JSON
```

`@PathVariable` is useful for RESTful URLs because it clearly identifies the resource being requested.

