# Unit 5 Answers: Connecting Frontend with Spring Boot

Subject: Full Stack Development  
Unit: Connecting Frontend with Spring Boot

---

## 1. Apply Fetch API to send a POST request from frontend to backend.

The Fetch API is used in frontend JavaScript to send HTTP requests to a backend server. A `POST` request is used when the frontend wants to create new data in the Spring Boot backend, such as adding a student, creating a user, or placing an order.

Example frontend code:

```js
async function addStudent() {
  const student = {
    name: "John",
    email: "john@example.com",
    marks: 85
  };

  const response = await fetch("http://localhost:8080/api/students", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(student)
  });

  const data = await response.json();
  console.log("Saved student:", data);
}
```

Spring Boot backend example:

```java
@PostMapping("/api/students")
public Student addStudent(@RequestBody Student student) {
    return studentService.saveStudent(student);
}
```

Flow:

```text
Frontend object
      |
      v
JSON.stringify()
      |
      v
POST request with JSON body
      |
      v
Spring Boot @RequestBody
      |
      v
Data saved in backend
```

Important points:

- Use `method: "POST"`.
- Use `Content-Type: application/json`.
- Convert JavaScript object to JSON using `JSON.stringify()`.
- Backend receives JSON using `@RequestBody`.

---

## 2. Apply GET request using Fetch API to retrieve data from Spring Boot backend.

A `GET` request is used to retrieve data from the backend. In a Spring Boot application, a frontend can use Fetch API to call a REST endpoint and display the received data.

Example frontend code:

```js
async function getStudents() {
  const response = await fetch("http://localhost:8080/api/students");
  const students = await response.json();

  console.log(students);
}
```

Example using React:

```jsx
import { useEffect, useState } from "react";

function StudentList() {
  const [students, setStudents] = useState([]);

  useEffect(() => {
    fetch("http://localhost:8080/api/students")
      .then((response) => response.json())
      .then((data) => setStudents(data));
  }, []);

  return (
    <ul>
      {students.map((student) => (
        <li key={student.id}>{student.name}</li>
      ))}
    </ul>
  );
}

export default StudentList;
```

Spring Boot backend:

```java
@GetMapping("/api/students")
public List<Student> getStudents() {
    return studentService.getAllStudents();
}
```

Flow:

```text
Frontend sends GET request
          |
          v
Spring Boot controller receives request
          |
          v
Backend returns JSON response
          |
          v
Frontend parses JSON
          |
          v
UI displays data
```

GET requests should not send a request body. They are mainly used for reading data.

---

## 3. Demonstrate how JSON data is sent from frontend to backend.

JSON stands for JavaScript Object Notation. It is the most common format used for sending data between frontend and backend in REST APIs.

In the frontend, data usually starts as a JavaScript object:

```js
const user = {
  username: "john",
  email: "john@example.com"
};
```

Before sending it to the backend, it is converted into a JSON string using `JSON.stringify()`.

```js
fetch("http://localhost:8080/api/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(user)
});
```

Spring Boot receives this JSON using `@RequestBody`:

```java
@PostMapping("/api/users")
public User createUser(@RequestBody User user) {
    return userService.saveUser(user);
}
```

Flow:

```text
JavaScript object
      |
      v
JSON.stringify()
      |
      v
JSON request body
      |
      v
Spring Boot @RequestBody
      |
      v
Java object
```

Example JSON sent in request:

```json
{
  "username": "john",
  "email": "john@example.com"
}
```

Important points:

- Frontend must set `Content-Type` as `application/json`.
- Frontend must use `JSON.stringify()`.
- Backend field names should match JSON property names.
- Spring Boot automatically maps JSON to a Java object using Jackson.

---

## 4. Apply PUT request to update data using Fetch API.

A `PUT` request is used to update existing data in the backend. Usually, the id of the record is passed in the URL, and the updated data is passed in the request body.

Example frontend code:

```js
async function updateStudent(id) {
  const updatedStudent = {
    name: "John Smith",
    email: "johnsmith@example.com",
    marks: 90
  };

  const response = await fetch(`http://localhost:8080/api/students/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(updatedStudent)
  });

  const data = await response.json();
  console.log("Updated student:", data);
}

updateStudent(1);
```

Spring Boot backend:

```java
@PutMapping("/api/students/{id}")
public Student updateStudent(
        @PathVariable Long id,
        @RequestBody Student student) {
    return studentService.updateStudent(id, student);
}
```

Flow:

```text
Frontend has updated data
        |
        v
PUT /api/students/1
        |
        v
JSON body sent
        |
        v
Backend finds record by ID
        |
        v
Backend updates record
```

Important points:

- Use `method: "PUT"`.
- Send the id in the URL.
- Send updated data in JSON body.
- Backend should use `@PathVariable` for id and `@RequestBody` for data.

---

## 5. Apply DELETE request to remove data from backend.

A `DELETE` request is used to remove an existing record from the backend. The id of the record to delete is usually passed in the URL.

Frontend code:

```js
async function deleteStudent(id) {
  const response = await fetch(`http://localhost:8080/api/students/${id}`, {
    method: "DELETE"
  });

  if (response.ok) {
    console.log("Student deleted successfully");
  } else {
    console.log("Failed to delete student");
  }
}

deleteStudent(1);
```

Spring Boot backend:

```java
@DeleteMapping("/api/students/{id}")
public ResponseEntity<String> deleteStudent(@PathVariable Long id) {
    studentService.deleteStudent(id);
    return ResponseEntity.ok("Student deleted successfully");
}
```

Flow:

```text
User clicks delete
      |
      v
Frontend sends DELETE request
      |
      v
Backend receives ID from URL
      |
      v
Record is deleted
      |
      v
Frontend updates UI
```

After successful deletion, the frontend should also update the displayed list. This can be done by removing the item from state or by fetching the list again.

---

## 6. Demonstrate handling of API response in frontend using Fetch API.

When the frontend calls a backend API, it must handle the response properly. The response may contain data, success status, or error status. Fetch API returns a `Response` object, which can be checked using `response.ok` and parsed using `response.json()`.

Example:

```js
async function loadStudent(id) {
  const response = await fetch(`http://localhost:8080/api/students/${id}`);

  if (!response.ok) {
    console.log("Request failed with status:", response.status);
    return;
  }

  const student = await response.json();
  console.log("Student:", student);
}
```

Response handling flow:

```text
Fetch request sent
      |
      v
Response received
      |
      v
Check response.ok
      |
   +--+--+
   |     |
  true  false
   |     |
Parse  Show error
JSON
```

Common response properties:

- `response.ok`: true for status code 200 to 299.
- `response.status`: HTTP status code such as 200, 404, or 500.
- `response.json()`: parses JSON response body.
- `response.text()`: parses plain text response.

Good response handling helps display correct data and meaningful error messages in the UI.

---

## 7. Apply CORS configuration in Spring Boot to allow frontend requests.

CORS stands for Cross-Origin Resource Sharing. It is a browser security mechanism that blocks requests from one origin to another unless the backend allows it. For example, a React app running on `http://localhost:3000` may be blocked from calling a Spring Boot backend running on `http://localhost:8080`.

Controller-level CORS configuration:

```java
@CrossOrigin(origins = "http://localhost:3000")
@RestController
@RequestMapping("/api/students")
public class StudentController {

    @GetMapping
    public List<Student> getStudents() {
        return studentService.getAllStudents();
    }
}
```

Global CORS configuration:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("http://localhost:3000")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*");
            }
        };
    }
}
```

Flow:

```text
React app on localhost:3000
        |
        v
Calls Spring Boot localhost:8080
        |
        v
Browser checks CORS policy
        |
        v
Backend allows origin
        |
        v
Request succeeds
```

CORS should be configured carefully. During development, `localhost:3000` can be allowed. In production, only the actual frontend domain should be allowed.

---

## 8. Show how to parse JSON response in frontend.

When a backend sends JSON data, the frontend must parse it before using it as a JavaScript object. In Fetch API, JSON parsing is done using `response.json()`.

Example:

```js
async function getUser() {
  const response = await fetch("http://localhost:8080/api/users/1");
  const user = await response.json();

  console.log(user.name);
  console.log(user.email);
}
```

If the backend response is:

```json
{
  "id": 1,
  "name": "John",
  "email": "john@example.com"
}
```

After parsing, it can be accessed like this:

```js
user.name
user.email
```

Parsing flow:

```text
Backend JSON response
        |
        v
response.json()
        |
        v
JavaScript object/array
        |
        v
Display in UI
```

React example:

```jsx
const [user, setUser] = useState(null);

useEffect(() => {
  async function loadUser() {
    const response = await fetch("http://localhost:8080/api/users/1");
    const data = await response.json();
    setUser(data);
  }

  loadUser();
}, []);
```

Important point: `response.json()` is asynchronous, so it must be used with `await` or `.then()`.

---

## 9. Apply basic error handling using try-catch in API calls.

API calls can fail due to network problems, wrong URLs, server errors, validation errors, or CORS issues. `try-catch` is used with `async/await` to handle such errors in a clean way.

Example:

```js
async function getStudents() {
  try {
    const response = await fetch("http://localhost:8080/api/students");

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const students = await response.json();
    console.log(students);
  } catch (error) {
    console.log("API call failed:", error.message);
  }
}
```

Flow:

```text
try block
   |
   v
Send API request
   |
   v
Check response.ok
   |
   +-- success -> parse data
   |
   +-- failure -> throw error
                  |
                  v
              catch block
```

Important points:

- Network errors are caught by `catch`.
- HTTP error status codes like 400 or 500 should be checked using `response.ok`.
- Error messages can be shown to the user.
- Errors should also be logged for debugging.

This makes the frontend more reliable and user-friendly.

---

## 10. Demonstrate connecting frontend form with backend API.

A frontend form can be connected to a Spring Boot backend by collecting form values, converting them to JSON, and sending them using Fetch API.

React form example:

```jsx
import { useState } from "react";

function StudentForm() {
  const [student, setStudent] = useState({
    name: "",
    email: ""
  });

  const handleChange = (event) => {
    setStudent({
      ...student,
      [event.target.name]: event.target.value
    });
  };

  const handleSubmit = async (event) => {
    event.preventDefault();

    const response = await fetch("http://localhost:8080/api/students", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(student)
    });

    const data = await response.json();
    console.log("Saved:", data);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={student.name}
        onChange={handleChange}
        placeholder="Name"
      />

      <input
        name="email"
        value={student.email}
        onChange={handleChange}
        placeholder="Email"
      />

      <button type="submit">Save</button>
    </form>
  );
}

export default StudentForm;
```

Spring Boot endpoint:

```java
@PostMapping("/api/students")
public Student saveStudent(@RequestBody Student student) {
    return studentService.saveStudent(student);
}
```

Flow:

```text
User fills form
      |
      v
React state stores input
      |
      v
Submit button clicked
      |
      v
Fetch POST request sent
      |
      v
Spring Boot saves data
```

This connects the frontend form with the backend API for full stack data submission.

---

## 11. Apply Fetch API to send a POST request from frontend to backend.

A `POST` request is used to send new data from the frontend to the backend. In a Spring Boot REST API, `POST` commonly maps to a controller method with `@PostMapping`.

Frontend example:

```js
async function createProduct() {
  const product = {
    name: "Keyboard",
    price: 1200
  };

  try {
    const response = await fetch("http://localhost:8080/api/products", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(product)
    });

    if (!response.ok) {
      throw new Error("Product creation failed");
    }

    const savedProduct = await response.json();
    console.log(savedProduct);
  } catch (error) {
    console.log(error.message);
  }
}
```

Backend:

```java
@PostMapping("/api/products")
public Product createProduct(@RequestBody Product product) {
    return productRepository.save(product);
}
```

Request structure:

```text
URL: http://localhost:8080/api/products
Method: POST
Headers: Content-Type: application/json
Body: {"name":"Keyboard","price":1200}
```

This format ensures that the frontend sends JSON data and the Spring Boot backend can map it to a Java object.

---

## 12. Apply GET request using Fetch API to retrieve data from Spring Boot backend.

A `GET` request retrieves data from the backend without modifying it. Fetch API can call a Spring Boot `@GetMapping` endpoint and convert the response into JSON.

Frontend example:

```js
async function loadProducts() {
  try {
    const response = await fetch("http://localhost:8080/api/products");

    if (!response.ok) {
      throw new Error("Unable to fetch products");
    }

    const products = await response.json();
    console.log(products);
  } catch (error) {
    console.log(error.message);
  }
}
```

Backend:

```java
@GetMapping("/api/products")
public List<Product> getProducts() {
    return productService.getAllProducts();
}
```

Data flow:

```text
GET /api/products
      |
      v
Spring Boot returns product list
      |
      v
Frontend parses JSON array
      |
      v
Products displayed in UI
```

In React, the GET request is usually called inside `useEffect()` so that data loads when the component opens.

---

## 13. A student management frontend needs CRUD operations with a Spring Boot backend. Apply Fetch API for POST, GET, PUT, DELETE requests.

CRUD means Create, Read, Update, and Delete. In a student management system, the frontend uses Fetch API to perform CRUD operations on the Spring Boot backend.

API design:

```text
POST   /api/students       -> create student
GET    /api/students       -> get all students
GET    /api/students/{id}  -> get one student
PUT    /api/students/{id}  -> update student
DELETE /api/students/{id}  -> delete student
```

Create student:

```js
async function createStudent(student) {
  const response = await fetch("http://localhost:8080/api/students", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(student)
  });

  return await response.json();
}
```

Read students:

```js
async function getStudents() {
  const response = await fetch("http://localhost:8080/api/students");
  return await response.json();
}
```

Update student:

```js
async function updateStudent(id, student) {
  const response = await fetch(`http://localhost:8080/api/students/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(student)
  });

  return await response.json();
}
```

Delete student:

```js
async function deleteStudent(id) {
  const response = await fetch(`http://localhost:8080/api/students/${id}`, {
    method: "DELETE"
  });

  return response.ok;
}
```

CRUD flow:

```text
Frontend UI
   |
   +-- Add form    -> POST
   +-- List page   -> GET
   +-- Edit form   -> PUT
   +-- Delete btn  -> DELETE
   |
   v
Spring Boot REST Controller
```

This structure connects the student management frontend with backend REST APIs.

---

## 14. An e-commerce app sends and receives JSON data between frontend and backend. Design the API call structure for request and response.

In an e-commerce application, JSON is used to exchange product, cart, order, and user data between frontend and Spring Boot backend.

Example: Add product to cart.

Frontend request:

```js
async function addToCart(productId, quantity) {
  const requestBody = {
    productId: productId,
    quantity: quantity
  };

  const response = await fetch("http://localhost:8080/api/cart/items", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(requestBody)
  });

  return await response.json();
}
```

Request JSON:

```json
{
  "productId": 101,
  "quantity": 2
}
```

Backend response JSON:

```json
{
  "cartId": 10,
  "items": [
    {
      "productId": 101,
      "name": "Keyboard",
      "quantity": 2,
      "price": 1200
    }
  ],
  "totalAmount": 2400
}
```

API call structure:

```text
Frontend component
      |
      v
API function
      |
      v
Fetch request with JSON body
      |
      v
Spring Boot REST controller
      |
      v
JSON response
      |
      v
Frontend updates UI
```

Good API design should include:

- Correct HTTP method.
- Clear endpoint URL.
- JSON request body.
- `Content-Type: application/json`.
- Proper response status.
- Consistent response structure.
- Error response for failed operations.

This makes frontend-backend interaction predictable and easier to debug.

---

## 15. A React app on localhost:3000 cannot access backend on localhost:8080. Apply CORS configuration to resolve the issue.

This issue occurs because the frontend and backend are running on different origins. `http://localhost:3000` and `http://localhost:8080` have different ports, so the browser treats them as different origins. The backend must allow the frontend origin using CORS configuration.

Simple controller-level solution:

```java
@CrossOrigin(origins = "http://localhost:3000")
@RestController
@RequestMapping("/api")
public class ProductController {

    @GetMapping("/products")
    public List<Product> getProducts() {
        return productService.getProducts();
    }
}
```

Global solution:

```java
@Configuration
public class CorsConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("http://localhost:3000")
                        .allowedMethods("GET", "POST", "PUT", "DELETE")
                        .allowedHeaders("*");
            }
        };
    }
}
```

Flow:

```text
React localhost:3000
        |
        v
Request to Spring Boot localhost:8080
        |
        v
Browser asks: Is this origin allowed?
        |
        v
Spring Boot CORS config allows it
        |
        v
Request succeeds
```

If Spring Security is used, CORS must also be enabled in the security configuration. The allowed origin should match the frontend URL exactly.

---

## 16. An API sometimes returns 404/500 errors during payment processing. Demonstrate handling errors using try-catch and status codes

During payment processing, API failures must be handled carefully because payment is a critical operation. A `404` may mean the payment endpoint or order was not found. A `500` means an internal server error occurred.

Frontend error handling:

```js
async function processPayment(paymentData) {
  try {
    const response = await fetch("http://localhost:8080/api/payments", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(paymentData)
    });

    if (response.status === 404) {
      throw new Error("Payment service or order not found");
    }

    if (response.status === 500) {
      throw new Error("Server error during payment processing");
    }

    if (!response.ok) {
      throw new Error(`Payment failed with status ${response.status}`);
    }

    const result = await response.json();
    console.log("Payment successful:", result);
  } catch (error) {
    console.log("Payment error:", error.message);
  }
}
```

Flow:

```text
Payment request
      |
      v
Response received
      |
      v
Check status code
      |
   +-- 200 -> success
   +-- 404 -> not found message
   +-- 500 -> server error message
   +-- other error -> generic failure
```

Good error handling should:

- Check `response.ok`.
- Check important status codes separately.
- Show user-friendly messages.
- Avoid duplicate payment attempts.
- Log error details for debugging.

This prevents the UI from failing silently during payment operations.

---

## 17. A frontend sends a POST request with body {name: "John"} but backend expects {username: "John"}. Identify the issue and apply the correct request format.

The issue is a mismatch between the JSON property name sent by the frontend and the field expected by the backend. The frontend sends `name`, but the backend expects `username`.

Incorrect request body:

```json
{
  "name": "John"
}
```

Expected request body:

```json
{
  "username": "John"
}
```

Correct Fetch API request:

```js
async function createUser() {
  const user = {
    username: "John"
  };

  const response = await fetch("http://localhost:8080/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(user)
  });

  const data = await response.json();
  console.log(data);
}
```

Spring Boot model:

```java
public class User {
    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

Mismatch flow:

```text
Frontend sends name
        |
        v
Backend expects username
        |
        v
Mapping fails or username becomes null
```

To fix this, frontend and backend must agree on the same JSON structure. API documentation should clearly define expected request fields.

---

## 18. A GET API returns a JSON array of users, but UI shows nothing. The response is received correctly. Identify where the issue might be in frontend handling.

If the response is received correctly but the UI shows nothing, the issue is likely in frontend state handling, JSON parsing, or rendering logic.

Possible causes:

- The response was not converted using `response.json()`.
- The parsed data was not stored in state.
- The component is rendering the wrong variable.
- The code assumes an object but the API returns an array.
- The `map()` function is missing or incorrect.
- The state is initialized incorrectly.
- Property names used in JSX do not match backend response fields.

Correct example:

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function loadUsers() {
      const response = await fetch("http://localhost:8080/api/users");
      const data = await response.json();
      setUsers(data);
    }

    loadUsers();
  }, []);

  return (
    <div>
      <h2>Users</h2>
      {users.map((user) => (
        <p key={user.id}>{user.username}</p>
      ))}
    </div>
  );
}
```

Debugging flow:

```text
Response received correctly
        |
        v
Check response.json()
        |
        v
Check setUsers(data)
        |
        v
Check users.map()
        |
        v
Check property names in JSX
```

For example, if the backend returns `username` but UI uses `user.name`, nothing meaningful may display. The frontend must use the correct property names.

---

## 19. A PUT request is made without sending an ID in the URL. Backend fails to update. Apply the correct API structure for update operation.

In REST APIs, update operations usually require the id of the record to be updated. If the frontend sends a `PUT` request without the id, the backend cannot identify which record should be updated.

Incorrect request:

```text
PUT /api/users
```

Correct request:

```text
PUT /api/users/1
```

Correct frontend code:

```js
async function updateUser(id, user) {
  const response = await fetch(`http://localhost:8080/api/users/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(user)
  });

  return await response.json();
}

updateUser(1, {
  username: "john_updated",
  email: "john@example.com"
});
```

Spring Boot backend:

```java
@PutMapping("/api/users/{id}")
public User updateUser(
        @PathVariable Long id,
        @RequestBody User user) {
    return userService.updateUser(id, user);
}
```

Flow:

```text
PUT /api/users/1
      |
      v
@PathVariable Long id
      |
      v
Backend finds user with id 1
      |
      v
Updates using request body
```

The correct update API structure includes the id in the URL and the updated data in the request body.

---

## 20. A CORS error occurs when frontend runs on localhost:3000 and backend on localhost:8080. Apply correct CORS configuration.

A CORS error occurs because the browser blocks cross-origin requests when the backend does not allow the frontend origin. Since the frontend runs on `localhost:3000` and backend on `localhost:8080`, CORS must be configured in Spring Boot.

Global CORS configuration:

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("http://localhost:3000")
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                        .allowedHeaders("*");
            }
        };
    }
}
```

Controller-level alternative:

```java
@CrossOrigin(origins = "http://localhost:3000")
@RestController
public class UserController {
    // API methods
}
```

Request flow:

```text
Browser sends request from localhost:3000
        |
        v
Spring Boot checks CORS policy
        |
        v
Origin is allowed
        |
        v
API response is given to frontend
```

Important checks:

- The frontend URL must exactly match the allowed origin.
- Include required methods such as `GET`, `POST`, `PUT`, and `DELETE`.
- If Spring Security is used, enable CORS in security configuration too.

---

## 21. A Fetch API call uses wrong HTTP method (GET instead of POST) causing failure. Identify and correct the method.

The issue is that the frontend is using the wrong HTTP method. `GET` is used to retrieve data, while `POST` is used to create new data. If the backend endpoint is defined with `@PostMapping`, a `GET` request will not match it and may return `405 Method Not Allowed`.

Incorrect code:

```js
fetch("http://localhost:8080/api/users", {
  method: "GET",
  body: JSON.stringify({ username: "John" })
});
```

Correct code:

```js
fetch("http://localhost:8080/api/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    username: "John"
  })
});
```

Backend:

```java
@PostMapping("/api/users")
public User createUser(@RequestBody User user) {
    return userService.saveUser(user);
}
```

Method matching:

```text
Frontend method       Backend mapping
     POST       --->  @PostMapping
     GET        --->  @GetMapping
     PUT        --->  @PutMapping
     DELETE     --->  @DeleteMapping
```

To fix this type of error, check the backend controller mapping and use the same HTTP method in the frontend Fetch API call.

---

## 22. A backend API expects path variable /user/1, but frontend sends /user?id=1. Identify mismatch and apply correct request

The mismatch is between a path variable and a query parameter. If the backend expects `/user/1`, then the id must be part of the URL path. The frontend is incorrectly sending the id as a query parameter using `/user?id=1`.

Backend expecting path variable:

```java
@GetMapping("/user/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

Correct frontend request:

```js
const id = 1;

const response = await fetch(`http://localhost:8080/user/${id}`);
const user = await response.json();
```

Incorrect request:

```js
fetch("http://localhost:8080/user?id=1");
```

Correct request:

```js
fetch("http://localhost:8080/user/1");
```

Difference:

```text
Path variable:
/user/1
Backend: @PathVariable Long id

Query parameter:
/user?id=1
Backend: @RequestParam Long id
```

Flow:

```text
Frontend URL /user/1
      |
      v
Spring Boot matches /user/{id}
      |
      v
id = 1 is received
```

The frontend URL structure must match the backend endpoint design.

---

## 23. An API call fails due to incorrect base URL configuration. Identify and correct the URL setup.

An incorrect base URL means the frontend is calling the wrong server, port, path, or protocol. For example, the Spring Boot backend may be running at `http://localhost:8080`, but the frontend may call `http://localhost:3000/api/users`, which points to the frontend server instead of the backend.

Incorrect setup:

```js
const BASE_URL = "http://localhost:3000";

fetch(`${BASE_URL}/api/users`);
```

Correct setup:

```js
const BASE_URL = "http://localhost:8080";

fetch(`${BASE_URL}/api/users`);
```

Better structure:

```js
const API_BASE_URL = "http://localhost:8080/api";

export async function getUsers() {
  const response = await fetch(`${API_BASE_URL}/users`);
  return await response.json();
}
```

Debugging checklist:

```text
Check protocol: http or https
Check host: localhost or domain
Check port: 8080 for Spring Boot
Check base path: /api
Check endpoint path: /users
```

Flow:

```text
Frontend API service
      |
      v
Uses API_BASE_URL
      |
      v
Builds full endpoint URL
      |
      v
Calls Spring Boot backend
```

Centralizing the base URL in one file reduces mistakes and makes it easier to change URLs for development and production.

---

## 24. A Fetch API call does not handle error status codes (400/500). Apply proper error handling logic.

Fetch API does not automatically throw an error for HTTP status codes like `400` or `500`. It only rejects the promise for network-level failures. Therefore, the frontend must manually check `response.ok` or `response.status`.

Correct error handling:

```js
async function saveUser(user) {
  try {
    const response = await fetch("http://localhost:8080/api/users", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(user)
    });

    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.message || `Error ${response.status}`);
    }

    const data = await response.json();
    console.log("User saved:", data);
  } catch (error) {
    console.log("Request failed:", error.message);
  }
}
```

Status handling flow:

```text
Fetch response received
        |
        v
Check response.ok
        |
   +----+----+
   |         |
 true      false
   |         |
Parse      Parse error
success    response
data       and show message
```

Common status codes:

- `400 Bad Request`: invalid request data.
- `401 Unauthorized`: login required.
- `403 Forbidden`: access denied.
- `404 Not Found`: resource not found.
- `500 Internal Server Error`: backend failure.

Proper error handling prevents silent failures and helps users understand what went wrong.

---

## 25. A web application displays product data, but repeated API calls are triggered on every user interaction, causing slow performance. Analyze the issue and apply suitable optimization techniques.

Repeated API calls usually happen because the API call is placed in the wrong location or the dependency array of `useEffect()` is incorrect. In React, if an API call runs on every render, then any user interaction that changes state can trigger another request.

Problem example:

```jsx
function Products() {
  const [products, setProducts] = useState([]);

  fetch("http://localhost:8080/api/products")
    .then((response) => response.json())
    .then((data) => setProducts(data));

  return <div>{products.length}</div>;
}
```

This is wrong because `fetch()` runs during every render.

Correct approach:

```jsx
useEffect(() => {
  async function loadProducts() {
    const response = await fetch("http://localhost:8080/api/products");
    const data = await response.json();
    setProducts(data);
  }

  loadProducts();
}, []);
```

Optimization flow:

```text
Component renders
      |
      v
useEffect with [] runs once
      |
      v
Products loaded
      |
      v
User interactions do not refetch unnecessarily
```

Suitable optimization techniques:

- Put API calls inside `useEffect()`.
- Use correct dependency arrays.
- Avoid API calls directly inside render logic.
- Cache data when suitable.
- Use pagination for large product lists.
- Debounce search/filter API calls.
- Avoid updating state unnecessarily.
- Use loading states to avoid repeated button clicks.

For example, search input should not call API on every keystroke without debounce. It should wait until the user stops typing for a short time.

---

## 26. In an online shopping system, deleting an item works correctly in backend testing tools but fails when triggered from the frontend. Analyze the request configuration and apply necessary fixes.

If deletion works in backend tools like Postman but fails from the frontend, the backend logic is probably correct. The issue is likely in frontend request configuration, URL, method, CORS, or state update.

Possible causes:

- Frontend uses wrong HTTP method.
- Item id is missing from the URL.
- URL is incorrect.
- CORS does not allow `DELETE`.
- Authentication token is missing.
- Frontend does not update UI after deletion.
- Backend expects `/cart/items/{id}` but frontend calls another path.

Correct frontend request:

```js
async function deleteCartItem(itemId) {
  try {
    const response = await fetch(`http://localhost:8080/api/cart/items/${itemId}`, {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json"
      }
    });

    if (!response.ok) {
      throw new Error(`Delete failed with status ${response.status}`);
    }

    console.log("Item deleted");
  } catch (error) {
    console.log(error.message);
  }
}
```

Spring Boot endpoint:

```java
@DeleteMapping("/api/cart/items/{id}")
public ResponseEntity<Void> deleteItem(@PathVariable Long id) {
    cartService.deleteItem(id);
    return ResponseEntity.noContent().build();
}
```

CORS should allow DELETE:

```java
.allowedMethods("GET", "POST", "PUT", "DELETE")
```

Debugging flow:

```text
Delete works in backend tool
        |
        v
Check frontend URL
        |
        v
Check method is DELETE
        |
        v
Check id in URL
        |
        v
Check CORS allows DELETE
        |
        v
Update frontend state after success
```

After successful deletion, remove the item from the UI:

```js
setItems(items.filter((item) => item.id !== itemId));
```

---

## 27. In a booking application, API calls take time to complete and the UI becomes unresponsive. Apply asynchronous handling and introduce a loading indicator to improve user experience.

API calls are asynchronous operations. If the UI does not show any loading state, users may think the application is stuck. A loading indicator improves user experience by showing that the request is in progress.

React example:

```jsx
import { useState } from "react";

function BookingForm() {
  const [loading, setLoading] = useState(false);
  const [message, setMessage] = useState("");

  const bookTicket = async () => {
    setLoading(true);
    setMessage("");

    try {
      const response = await fetch("http://localhost:8080/api/bookings", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          userId: 1,
          seatNo: "A1"
        })
      });

      if (!response.ok) {
        throw new Error("Booking failed");
      }

      setMessage("Booking successful");
    } catch (error) {
      setMessage(error.message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div>
      <button onClick={bookTicket} disabled={loading}>
        {loading ? "Booking..." : "Book Ticket"}
      </button>

      {loading && <p>Please wait...</p>}
      {message && <p>{message}</p>}
    </div>
  );
}
```

Flow:

```text
User clicks Book
      |
      v
Set loading = true
      |
      v
Send async API request
      |
      v
Success or failure response
      |
      v
Set loading = false
      |
      v
Show result message
```

Improvements:

- Disable button while request is running.
- Show loading message or spinner.
- Use `try-catch-finally`.
- Prevent duplicate booking requests.
- Show clear success or error messages.

This keeps the UI responsive and understandable during slow API calls.

---

## 28. In a dashboard application, data received from the backend is shown as undefined on the UI. Analyze the response handling and apply correct parsing logic.

If backend data appears as `undefined` in the UI, the frontend is probably accessing the wrong property, rendering before data is loaded, or not parsing the JSON response correctly.

Example backend response:

```json
{
  "totalUsers": 120,
  "totalOrders": 45
}
```

Incorrect frontend code:

```jsx
<p>Total Users: {dashboard.users}</p>
```

Here, `dashboard.users` is undefined because the backend sends `totalUsers`, not `users`.

Correct code:

```jsx
import { useEffect, useState } from "react";

function Dashboard() {
  const [dashboard, setDashboard] = useState(null);

  useEffect(() => {
    async function loadDashboard() {
      const response = await fetch("http://localhost:8080/api/dashboard");
      const data = await response.json();
      setDashboard(data);
    }

    loadDashboard();
  }, []);

  if (!dashboard) {
    return <p>Loading dashboard...</p>;
  }

  return (
    <div>
      <p>Total Users: {dashboard.totalUsers}</p>
      <p>Total Orders: {dashboard.totalOrders}</p>
    </div>
  );
}
```

Debugging flow:

```text
Data shows undefined
        |
        v
Check actual backend JSON
        |
        v
Check response.json()
        |
        v
Check state value
        |
        v
Check property names in JSX
        |
        v
Add loading check before rendering
```

Common fixes:

- Use `await response.json()`.
- Store parsed data in state.
- Match property names exactly.
- Use conditional rendering before data loads.
- Inspect response using browser developer tools.

---

## 29. In a registration system, a request to create a new user fails due to improper request configuration. Identify the issue and apply correct request headers.

In a registration system, a user creation request may fail if the frontend does not send proper headers or JSON body. The most common issue is missing `Content-Type: application/json`. Without this header, Spring Boot may not correctly parse the request body.

Incorrect request:

```js
fetch("http://localhost:8080/api/register", {
  method: "POST",
  body: {
    username: "john",
    email: "john@example.com",
    password: "123456"
  }
});
```

Problems:

- The body is a JavaScript object, not JSON string.
- `Content-Type` header is missing.
- Backend may receive empty or invalid data.

Correct request:

```js
async function registerUser() {
  const user = {
    username: "john",
    email: "john@example.com",
    password: "123456"
  };

  const response = await fetch("http://localhost:8080/api/register", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json"
    },
    body: JSON.stringify(user)
  });

  if (!response.ok) {
    throw new Error(`Registration failed: ${response.status}`);
  }

  return await response.json();
}
```

Spring Boot endpoint:

```java
@PostMapping("/api/register")
public User register(@RequestBody User user) {
    return userService.register(user);
}
```

Correct request structure:

```text
Method: POST
URL: http://localhost:8080/api/register
Headers:
  Content-Type: application/json
  Accept: application/json
Body:
  JSON.stringify(user)
```

This ensures the backend can read JSON using `@RequestBody`.

---

## 30. In a multi-module application, different frontend components interact inconsistently with backend APIs. Analyze the issue and design a structured API interaction flow.

In a multi-module frontend application, inconsistent API interaction happens when each component writes its own Fetch API logic differently. This can cause wrong URLs, missing headers, inconsistent error handling, duplicate code, and difficult debugging.

Problem structure:

```text
UserComponent -> fetch directly
ProductComponent -> different fetch format
OrderComponent -> wrong base URL
PaymentComponent -> no error handling
```

Better approach: create a centralized API layer.

Suggested structure:

```text
src/
  api/
    apiClient.js
    userApi.js
    productApi.js
    orderApi.js
  components/
  pages/
```

Base API client:

```js
const API_BASE_URL = "http://localhost:8080/api";

export async function apiRequest(endpoint, options = {}) {
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    headers: {
      "Content-Type": "application/json",
      ...options.headers
    },
    ...options
  });

  if (!response.ok) {
    throw new Error(`API error: ${response.status}`);
  }

  return await response.json();
}
```

Module-specific API:

```js
import { apiRequest } from "./apiClient";

export function getUsers() {
  return apiRequest("/users");
}

export function createUser(user) {
  return apiRequest("/users", {
    method: "POST",
    body: JSON.stringify(user)
  });
}
```

Structured flow:

```text
Component
   |
   v
Module API function
   |
   v
Common apiClient
   |
   v
Spring Boot REST API
   |
   v
Common response/error handling
```

Benefits:

- One base URL for all modules.
- Common headers for every request.
- Consistent error handling.
- Less duplicate code.
- Easier debugging.
- Easier change from development to production URL.

This structured API interaction flow improves maintainability in large frontend applications.

---

## 31. A web application displays product data, but repeated API calls are triggered on every user interaction, causing slow performance. Analyze the issue and apply suitable optimization techniques.

Repeated API calls during user interaction usually mean the API call is tied to component re-rendering or state changes incorrectly. For example, if a product list is fetched every time a filter, input, or counter changes, the application becomes slow.

Common incorrect pattern:

```jsx
useEffect(() => {
  fetchProducts();
}, [searchText, selectedProduct, cartCount]);
```

Here, `fetchProducts()` may run for unrelated changes like cart count.

Better pattern:

```jsx
useEffect(() => {
  fetchProducts();
}, []);
```

If products must be searched:

```jsx
useEffect(() => {
  const timer = setTimeout(() => {
    fetchProducts(searchText);
  }, 500);

  return () => clearTimeout(timer);
}, [searchText]);
```

This is called debouncing. It avoids calling the API on every keystroke.

Optimization flow:

```text
Identify repeated API calls
        |
        v
Check useEffect dependencies
        |
        v
Remove unrelated dependencies
        |
        v
Use debounce/cache/pagination
        |
        v
Reduce backend load and improve UI speed
```

Suitable techniques:

- Use correct `useEffect()` dependency array.
- Fetch data only when needed.
- Debounce search input.
- Cache product data.
- Use pagination or lazy loading.
- Avoid setting state in a loop.
- Avoid API calls inside render logic.
- Use browser developer tools Network tab to detect repeated calls.

This improves performance by reducing unnecessary backend requests.

---

## 32. In an online shopping system, deleting an item works correctly in backend testing tools but fails when triggered from the frontend. Analyze the request configuration and apply necessary fixes.

When deletion works in backend testing tools but fails in the frontend, the problem is usually not the backend delete logic. It is commonly caused by frontend request mismatch.

Possible frontend issues:

- Wrong endpoint URL.
- Wrong HTTP method.
- Missing item id in URL.
- CORS does not allow `DELETE`.
- Missing authentication header.
- Frontend sends query parameter while backend expects path variable.
- UI does not update after deletion.

Correct request:

```js
async function removeProductFromCart(cartItemId) {
  try {
    const response = await fetch(
      `http://localhost:8080/api/cart/items/${cartItemId}`,
      {
        method: "DELETE"
      }
    );

    if (!response.ok) {
      throw new Error(`Delete failed: ${response.status}`);
    }

    setCartItems((items) =>
      items.filter((item) => item.id !== cartItemId)
    );
  } catch (error) {
    console.log(error.message);
  }
}
```

Backend:

```java
@DeleteMapping("/api/cart/items/{id}")
public ResponseEntity<Void> deleteCartItem(@PathVariable Long id) {
    cartService.deleteCartItem(id);
    return ResponseEntity.noContent().build();
}
```

If authentication is required:

```js
headers: {
  "Authorization": `Bearer ${token}`
}
```

CORS must allow DELETE:

```java
.allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
```

Debugging flow:

```text
Compare Postman request with frontend request
        |
        v
Check URL, method, id, headers
        |
        v
Check browser Network tab
        |
        v
Fix mismatch
        |
        v
Update frontend state after success
```

The frontend request should match the tested backend request exactly.

---

## 33. In a booking application, API calls take time to complete and the UI becomes unresponsive. Apply asynchronous handling and introduce a loading indicator to improve user experience.

Slow booking APIs should be handled asynchronously so the UI can remain usable. A loading indicator tells the user that the request is in progress and prevents repeated clicks.

React example:

```jsx
function BookingPage() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [success, setSuccess] = useState("");

  async function confirmBooking() {
    setLoading(true);
    setError("");
    setSuccess("");

    try {
      const response = await fetch("http://localhost:8080/api/bookings", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          roomId: 5,
          date: "2026-05-10"
        })
      });

      if (!response.ok) {
        throw new Error("Booking could not be completed");
      }

      const booking = await response.json();
      setSuccess(`Booking confirmed with id ${booking.id}`);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }

  return (
    <div>
      <button onClick={confirmBooking} disabled={loading}>
        {loading ? "Confirming..." : "Confirm Booking"}
      </button>

      {loading && <p>Processing your booking...</p>}
      {error && <p>{error}</p>}
      {success && <p>{success}</p>}
    </div>
  );
}
```

Flow:

```text
Click Confirm Booking
        |
        v
loading = true
        |
        v
Async request sent
        |
        v
UI shows loading indicator
        |
        v
Response received
        |
        v
Show success/error and loading = false
```

Best practices:

- Use `async/await`.
- Use `try-catch-finally`.
- Disable buttons while loading.
- Show loading text or spinner.
- Show success or error response.
- Prevent duplicate booking submissions.

This improves user experience and avoids confusion during slow backend operations.

---

## 34. In a dashboard application, data received from the backend is shown as undefined on the UI. Analyze the response handling and apply correct parsing logic.

If dashboard data is shown as `undefined`, the data may be received but not handled correctly in the frontend. The problem is usually in JSON parsing, state initialization, property names, or rendering before data loads.

Example backend response:

```json
{
  "stats": {
    "users": 120,
    "orders": 50,
    "revenue": 75000
  }
}
```

Incorrect UI code:

```jsx
<p>Users: {dashboard.users}</p>
```

Here, `users` is inside `stats`, so the correct access is:

```jsx
dashboard.stats.users
```

Correct React code:

```jsx
import { useEffect, useState } from "react";

function Dashboard() {
  const [dashboard, setDashboard] = useState(null);

  useEffect(() => {
    async function loadData() {
      try {
        const response = await fetch("http://localhost:8080/api/dashboard");

        if (!response.ok) {
          throw new Error("Failed to load dashboard");
        }

        const data = await response.json();
        setDashboard(data);
      } catch (error) {
        console.log(error.message);
      }
    }

    loadData();
  }, []);

  if (!dashboard) {
    return <p>Loading...</p>;
  }

  return (
    <div>
      <h2>Dashboard</h2>
      <p>Users: {dashboard.stats.users}</p>
      <p>Orders: {dashboard.stats.orders}</p>
      <p>Revenue: {dashboard.stats.revenue}</p>
    </div>
  );
}

export default Dashboard;
```

Debugging flow:

```text
UI shows undefined
        |
        v
Console log actual API data
        |
        v
Check object nesting
        |
        v
Use correct property path
        |
        v
Render only after data is loaded
```

Correct parsing logic includes:

- Use `await response.json()`.
- Store parsed JSON in state.
- Match frontend property access with backend response structure.
- Use conditional rendering while data is loading.
- Check browser Network tab and console logs.

This fixes undefined values and ensures dashboard data is displayed correctly.

