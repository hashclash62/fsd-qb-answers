# Unit 4 Answers: Frontend Designing

Subject: Full Stack Development  
Unit: Frontend Designing

---

## 1. A user types name in a textbox and it should display on screen. Write a simple React example using useState().

In React, `useState()` is a hook used to store and update state inside a functional component. For a textbox, the typed value can be stored in a state variable. Whenever the user types something, the state is updated and React re-renders the UI.

Example:

```jsx
import React, { useState } from "react";

function NameDisplay() {
  const [name, setName] = useState("");

  return (
    <div>
      <h2>Enter Your Name</h2>

      <input
        type="text"
        value={name}
        onChange={(event) => setName(event.target.value)}
        placeholder="Enter name"
      />

      <p>Your name is: {name}</p>
    </div>
  );
}

export default NameDisplay;
```

Working:

```text
User types in textbox
        |
        v
onChange event runs
        |
        v
setName() updates state
        |
        v
Component re-renders
        |
        v
Updated name is displayed
```

Here, `name` stores the current value and `setName()` updates it. The input is called a controlled component because its value is controlled by React state.

---

## 2. A page loads and should show data from an API. Show how useEffect() is used.

In React, `useEffect()` is used to perform side effects such as fetching data from an API, updating the document title, or setting timers. When a page or component loads, `useEffect()` can call an API and store the response in state.

Example:

```jsx
import React, { useEffect, useState } from "react";

function UsersList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => setUsers(data))
      .catch((error) => console.log("Error:", error));
  }, []);

  return (
    <div>
      <h2>Users</h2>

      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default UsersList;
```

The empty dependency array `[]` means the effect runs only once after the component first renders.

Flow:

```text
Component loads
      |
      v
useEffect() runs
      |
      v
API request is sent
      |
      v
Response is received
      |
      v
State is updated
      |
      v
Data is displayed
```

Thus, `useEffect()` is useful for loading data when a React component is mounted.

---

## 3. A parent component sends a message to a child component. Show how props are used.

In React, props are used to pass data from a parent component to a child component. Props are read-only values received by the child. They help in component communication and reusability.

Example:

```jsx
function ChildComponent(props) {
  return <h2>Message from parent: {props.message}</h2>;
}

function ParentComponent() {
  return (
    <div>
      <ChildComponent message="Welcome to React!" />
    </div>
  );
}

export default ParentComponent;
```

The same example using destructuring:

```jsx
function ChildComponent({ message }) {
  return <h2>Message from parent: {message}</h2>;
}
```

Flow:

```text
Parent Component
      |
      | passes message prop
      v
Child Component
      |
      v
Displays message
```

Important points:

- Props are passed from parent to child.
- Props make components reusable.
- Props should not be modified directly by the child.
- If child needs to communicate back, the parent can pass a function as a prop.

---

## 4. A button click should increase a counter value. Write a simple React example.

React can manage a counter value using the `useState()` hook. The counter value is stored in state, and a button click updates it.

Example:

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increaseCounter = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h2>Counter: {count}</h2>
      <button onClick={increaseCounter}>Increase</button>
    </div>
  );
}

export default Counter;
```

Working:

```text
Initial count = 0
      |
      v
User clicks button
      |
      v
increaseCounter() runs
      |
      v
setCount(count + 1)
      |
      v
UI shows updated count
```

Each time the button is clicked, `setCount()` updates the state. React then re-renders the component and displays the new counter value.

---

## 5. A list of items should be displayed on screen. Show how map() is used in React.

In React, the JavaScript `map()` function is commonly used to display lists. It converts each item in an array into a JSX element.

Example:

```jsx
function FruitsList() {
  const fruits = ["Apple", "Banana", "Mango", "Orange"];

  return (
    <div>
      <h2>Fruit List</h2>

      <ul>
        {fruits.map((fruit, index) => (
          <li key={index}>{fruit}</li>
        ))}
      </ul>
    </div>
  );
}

export default FruitsList;
```

Working:

```text
Array of items
      |
      v
map() loops over each item
      |
      v
Each item becomes JSX
      |
      v
List is displayed on screen
```

The `key` prop helps React identify each list item during re-rendering. In real applications, a unique id should be used as the key instead of the index when available.

Example with objects:

```jsx
const users = [
  { id: 1, name: "Amit" },
  { id: 2, name: "Neha" }
];

{users.map((user) => (
  <p key={user.id}>{user.name}</p>
))}
```

Thus, `map()` is useful for rendering dynamic lists in React.

---

## 6. A form should not submit if fields are empty. Show basic validation logic.

Form validation is used to check user input before submitting data. In React, basic validation can be done by checking state values before processing the form.

Example:

```jsx
import React, { useState } from "react";

function LoginForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();

    if (email.trim() === "" || password.trim() === "") {
      setError("Email and password are required");
      return;
    }

    setError("");
    alert("Form submitted successfully");
  };

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>

      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(event) => setEmail(event.target.value)}
      />

      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(event) => setPassword(event.target.value)}
      />

      {error && <p style={{ color: "red" }}>{error}</p>}

      <button type="submit">Submit</button>
    </form>
  );
}

export default LoginForm;
```

Flow:

```text
User clicks Submit
      |
      v
preventDefault() stops normal form submit
      |
      v
Check empty fields
      |
   +--+--+
   |     |
 Empty  Valid
   |     |
Show   Submit
error  form
```

This prevents invalid data from being submitted and improves user experience.

---

## 7. An Angular component should display a variable value. Show interpolation example.

In Angular, interpolation is a one-way data binding technique used to display component data in the template. It uses double curly braces `{{ }}`.

Component file:

```ts
import { Component } from "@angular/core";

@Component({
  selector: "app-student",
  templateUrl: "./student.component.html"
})
export class StudentComponent {
  studentName: string = "Amit";
  course: string = "Full Stack Development";
}
```

Template file:

```html
<h2>Student Name: {{ studentName }}</h2>
<p>Course: {{ course }}</p>
```

Output:

```text
Student Name: Amit
Course: Full Stack Development
```

Working:

```text
Component property
      |
      v
Interpolation {{ property }}
      |
      v
Value displayed in HTML
```

Interpolation is one-way because data flows from the component class to the template. It is useful for displaying variables, expressions, and method return values.

---

## 8. Apply TypeScript to define interfaces and use them in a component-based application.

TypeScript is a superset of JavaScript that adds static typing. In component-based applications, TypeScript interfaces are used to define the structure of objects. This improves readability, type safety, and maintainability.

Example interface:

```ts
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}
```

React component using interface:

```tsx
interface User {
  id: number;
  name: string;
  email: string;
}

interface UserCardProps {
  user: User;
}

function UserCard({ user }: UserCardProps) {
  return (
    <div>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
}

function App() {
  const user: User = {
    id: 1,
    name: "Amit",
    email: "amit@example.com"
  };

  return <UserCard user={user} />;
}

export default App;
```

Flow:

```text
Interface defines object shape
        |
        v
Component receives typed props
        |
        v
TypeScript checks correct data usage
        |
        v
Fewer runtime errors
```

Benefits of interfaces:

- They define a clear data structure.
- They catch type errors during development.
- They improve code documentation.
- They make components easier to reuse.
- They help teams work on large applications safely.

Thus, TypeScript interfaces are useful for strongly typed component-based frontend development.

---

## 9. Apply React useState() hook to manage form input and update UI dynamically.

The `useState()` hook allows a React functional component to store and update form input values. When the user types in a form field, the state changes and the UI updates automatically.

Example:

```jsx
import React, { useState } from "react";

function FeedbackForm() {
  const [feedback, setFeedback] = useState("");

  return (
    <div>
      <h2>Feedback Form</h2>

      <textarea
        value={feedback}
        onChange={(event) => setFeedback(event.target.value)}
        placeholder="Write feedback"
      />

      <h3>Preview:</h3>
      <p>{feedback}</p>
    </div>
  );
}

export default FeedbackForm;
```

Explanation:

- `feedback` stores the current input value.
- `setFeedback()` updates the value.
- `onChange` runs whenever the user types.
- The preview text changes immediately as state changes.

Flow:

```text
User types feedback
        |
        v
onChange reads input value
        |
        v
setFeedback updates state
        |
        v
React re-renders component
        |
        v
Preview displays latest input
```

This is called dynamic UI updating because the displayed output changes immediately according to user input.

---

## 10. Differentiate between React and Angular for front end development

React and Angular are both popular tools for frontend development, but they follow different approaches.

React is a JavaScript library mainly used for building user interfaces. Angular is a complete frontend framework developed by Google. Angular provides many built-in features, while React often uses additional libraries for routing, state management, and form handling.

| Point | React | Angular |
|---|---|---|
| Type | JavaScript library | Complete frontend framework |
| Developed by | Meta | Google |
| Language | JavaScript or TypeScript | TypeScript mainly |
| Architecture | Component-based UI library | Full MVC-like framework |
| Data binding | Mostly one-way data flow | Supports one-way and two-way binding |
| DOM | Uses Virtual DOM | Uses real DOM with change detection |
| Routing | Requires `react-router-dom` | Built-in Angular Router |
| State management | `useState`, `useContext`, Redux, etc. | Services, RxJS, state libraries |
| Learning curve | Easier to start | More structured but more complex |
| Project structure | Flexible | Opinionated and organized |

React example:

```jsx
function App() {
  return <h1>Hello React</h1>;
}
```

Angular example:

```ts
@Component({
  selector: "app-root",
  template: "<h1>Hello Angular</h1>"
})
export class AppComponent {}
```

Simple comparison:

```text
React
  |
  +-- Library
  +-- Flexible
  +-- Uses JSX

Angular
  |
  +-- Framework
  +-- Structured
  +-- Uses TypeScript, templates, modules, services
```

React is suitable when flexibility and lightweight UI development are needed. Angular is suitable for large enterprise applications that require a complete structured framework.

---

## 11. Apply props to enable communication between parent and child components in React.

Props are used in React to pass data from a parent component to a child component. This is one of the main methods of inter-component communication in React.

Example:

```jsx
function UserProfile({ name, email }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{email}</p>
    </div>
  );
}

function App() {
  return (
    <div>
      <UserProfile name="Amit" email="amit@example.com" />
      <UserProfile name="Neha" email="neha@example.com" />
    </div>
  );
}

export default App;
```

In this example:

- `App` is the parent component.
- `UserProfile` is the child component.
- `name` and `email` are props.
- The same child component is reused with different data.

Communication flow:

```text
Parent Component
      |
      | name, email props
      v
Child Component
      |
      v
Displays user details
```

Props can also pass functions:

```jsx
function Child({ onButtonClick }) {
  return <button onClick={onButtonClick}>Click</button>;
}

function Parent() {
  const showMessage = () => alert("Child button clicked");

  return <Child onButtonClick={showMessage} />;
}
```

This allows child-to-parent communication through callback functions.

---

## 12. Design basic routing in React to navigate between Home and Profile pages.

Routing allows a React application to show different components for different URLs. React does not include routing by default, so the `react-router-dom` library is commonly used.

Install:

```bash
npm install react-router-dom
```

Example:

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function Home() {
  return <h2>Home Page</h2>;
}

function Profile() {
  return <h2>Profile Page</h2>;
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/profile">Profile</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/profile" element={<Profile />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

Routing flow:

```text
User clicks navigation link
          |
          v
URL changes
          |
          v
React Router checks matching route
          |
          v
Matching component is rendered
```

Important parts:

- `BrowserRouter` enables routing.
- `Link` is used for navigation without page reload.
- `Routes` contains route definitions.
- `Route` maps a path to a component.

This creates a single-page application where navigation happens without refreshing the entire page.

---

## 13. Apply Redux to manage shared state across multiple components.

Redux is a state management library used to manage shared application state in a predictable way. It is useful when multiple components need access to the same data, such as logged-in user details, cart items, or application settings.

Redux architecture:

```text
Component
   |
   | dispatch action
   v
Action
   |
   v
Reducer
   |
   v
Store updated
   |
   v
Components receive updated state
```

Example using Redux Toolkit:

```jsx
import { configureStore, createSlice } from "@reduxjs/toolkit";
import { Provider, useDispatch, useSelector } from "react-redux";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    }
  }
});

const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
});

const { increment } = counterSlice.actions;

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => dispatch(increment())}>Increase</button>
    </div>
  );
}

function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
}

export default App;
```

Explanation:

- Store holds global state.
- Actions describe what happened.
- Reducers update the state.
- Components read state using `useSelector()`.
- Components update state using `dispatch()`.

Redux improves state management in large applications by keeping shared state centralized and predictable.

---

## 14. Apply Angular data binding (one-way and two-way) in a form-based application.

Angular data binding connects component data with the HTML template. In form-based applications, data binding is used to display values, update inputs, and respond to user actions.

### One-way data binding

One-way binding means data flows in one direction.

Interpolation:

```html
<h2>{{ title }}</h2>
```

Property binding:

```html
<input [value]="username" />
```

Event binding:

```html
<button (click)="saveUser()">Save</button>
```

Component:

```ts
export class UserComponent {
  title = "User Form";
  username = "Amit";

  saveUser() {
    console.log("User saved");
  }
}
```

### Two-way data binding

Two-way binding means data flows from component to view and from view to component. Angular uses `[(ngModel)]`.

Template:

```html
<input [(ngModel)]="username" placeholder="Enter username" />
<p>You entered: {{ username }}</p>
```

Component:

```ts
export class UserComponent {
  username = "";
}
```

Flow:

```text
Component property
      |
      v
Input field displays value
      |
      v
User changes input
      |
      v
Component property updates
```

For `ngModel`, `FormsModule` must be imported in the Angular module.

Data binding makes Angular forms easier to build because form data and UI stay synchronized.

---

## 15. Demonstrate use of Angular directives (*ngIf, *ngFor) in UI rendering.

Angular directives are used to change the structure or behavior of the DOM. Two commonly used structural directives are `*ngIf` and `*ngFor`.

`*ngIf` is used to conditionally display an element. `*ngFor` is used to repeat an element for each item in a collection.

Component:

```ts
export class ProductComponent {
  isLoggedIn = true;

  products = [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Mouse" },
    { id: 3, name: "Keyboard" }
  ];
}
```

Template:

```html
<h2 *ngIf="isLoggedIn">Welcome User</h2>
<p *ngIf="!isLoggedIn">Please login first</p>

<h3>Product List</h3>
<ul>
  <li *ngFor="let product of products">
    {{ product.name }}
  </li>
</ul>
```

Working:

```text
*ngIf
  |
  +-- condition true  -> element displayed
  +-- condition false -> element removed

*ngFor
  |
  +-- loops over array
  +-- creates one element per item
```

Directives are useful in UI rendering because frontend pages often need to show or hide elements and display dynamic lists based on application data.

---

## 16. Apply Angular services with dependency injection to share data.

An Angular service is a class used to share data or logic across multiple components. Dependency Injection, or DI, is the mechanism Angular uses to provide service objects to components.

Service:

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root"
})
export class UserService {
  private username = "Amit";

  getUsername(): string {
    return this.username;
  }

  setUsername(name: string): void {
    this.username = name;
  }
}
```

Component using service:

```ts
import { Component } from "@angular/core";
import { UserService } from "./user.service";

@Component({
  selector: "app-profile",
  template: "<h2>User: {{ name }}</h2>"
})
export class ProfileComponent {
  name: string;

  constructor(private userService: UserService) {
    this.name = this.userService.getUsername();
  }
}
```

Flow:

```text
Angular Service
      |
      v
Registered with DI container
      |
      v
Injected into component constructor
      |
      v
Component uses shared data/methods
```

Benefits:

- Shared data can be reused across components.
- Business logic is separated from UI code.
- Components become cleaner.
- Testing becomes easier.
- Services can call APIs using `HttpClient`.

Thus, Angular services and dependency injection help build organized and maintainable applications.

---

## 17. Design Angular forms with basic validation rules.

Angular forms are used to collect and validate user input. Angular supports template-driven forms and reactive forms. For basic validation, template-driven forms are easy to use.

Example template-driven form:

```html
<form #userForm="ngForm" (ngSubmit)="submitForm(userForm)">
  <label>Name</label>
  <input
    type="text"
    name="name"
    [(ngModel)]="user.name"
    required
    #name="ngModel"
  />
  <p *ngIf="name.invalid && name.touched">Name is required</p>

  <label>Email</label>
  <input
    type="email"
    name="email"
    [(ngModel)]="user.email"
    required
    email
    #email="ngModel"
  />
  <p *ngIf="email.invalid && email.touched">Valid email is required</p>

  <button type="submit" [disabled]="userForm.invalid">
    Submit
  </button>
</form>
```

Component:

```ts
export class RegisterComponent {
  user = {
    name: "",
    email: ""
  };

  submitForm(form: any) {
    if (form.valid) {
      console.log("Form submitted", this.user);
    }
  }
}
```

Validation flow:

```text
User enters form data
      |
      v
Angular checks validation rules
      |
      v
Are fields valid?
      |
   +--+--+
   |     |
  Yes    No
   |     |
Submit Show errors
```

Common validation rules:

- `required`
- `email`
- `minlength`
- `maxlength`
- `pattern`

Angular forms improve user experience by showing errors early and preventing invalid form submission.

---

## 18. Apply TypeScript to define interfaces and use them in a component-based application.

TypeScript interfaces define the shape of data used by components. In a component-based application, this is useful because components often exchange data through props, inputs, or services.

Angular example:

```ts
export interface Product {
  id: number;
  name: string;
  price: number;
  inStock: boolean;
}
```

Component:

```ts
import { Component } from "@angular/core";
import { Product } from "./product";

@Component({
  selector: "app-product-list",
  templateUrl: "./product-list.component.html"
})
export class ProductListComponent {
  products: Product[] = [
    { id: 1, name: "Laptop", price: 50000, inStock: true },
    { id: 2, name: "Mouse", price: 700, inStock: false }
  ];
}
```

Template:

```html
<div *ngFor="let product of products">
  <h3>{{ product.name }}</h3>
  <p>Price: {{ product.price }}</p>
  <p *ngIf="product.inStock">Available</p>
  <p *ngIf="!product.inStock">Out of stock</p>
</div>
```

Flow:

```text
Product interface
      |
      v
Component declares Product[]
      |
      v
Template displays typed data
      |
      v
Errors caught during development
```

Advantages:

- Interfaces make data models clear.
- They reduce mistakes in property names and types.
- They improve code completion in editors.
- They make components easier to maintain.
- They are useful in both React and Angular applications.

Thus, TypeScript interfaces support safer and cleaner component-based frontend development.

---

## 19. A user registration form requires dynamic input handling. Write a React component using useState() to capture and display user inputs.

A registration form can use `useState()` to store multiple input values in one state object. Each input updates the corresponding property using its `name` attribute.

Example:

```jsx
import React, { useState } from "react";

function RegistrationForm() {
  const [user, setUser] = useState({
    name: "",
    email: "",
    password: ""
  });

  const handleChange = (event) => {
    const { name, value } = event.target;

    setUser({
      ...user,
      [name]: value
    });
  };

  return (
    <div>
      <h2>User Registration</h2>

      <input
        type="text"
        name="name"
        placeholder="Name"
        value={user.name}
        onChange={handleChange}
      />

      <input
        type="email"
        name="email"
        placeholder="Email"
        value={user.email}
        onChange={handleChange}
      />

      <input
        type="password"
        name="password"
        placeholder="Password"
        value={user.password}
        onChange={handleChange}
      />

      <h3>Preview</h3>
      <p>Name: {user.name}</p>
      <p>Email: {user.email}</p>
    </div>
  );
}

export default RegistrationForm;
```

Dynamic input flow:

```text
User types in any field
       |
       v
handleChange() reads name and value
       |
       v
State object is updated
       |
       v
Component re-renders
       |
       v
Updated values are displayed
```

The expression `[name]: value` dynamically updates the correct field in the state object. This is useful when a form has multiple inputs.

---

## 20. A component needs to fetch and display data from an API when it loads. Write code using useEffect() to achieve this.

The `useEffect()` hook is used to run code after a component renders. For API calls, it is commonly used with `useState()` to store fetched data.

Example:

```jsx
import React, { useEffect, useState } from "react";

function Posts() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => response.json())
      .then((data) => {
        setPosts(data.slice(0, 5));
        setLoading(false);
      })
      .catch((error) => {
        console.log("API Error:", error);
        setLoading(false);
      });
  }, []);

  if (loading) {
    return <p>Loading posts...</p>;
  }

  return (
    <div>
      <h2>Posts</h2>
      {posts.map((post) => (
        <div key={post.id}>
          <h3>{post.title}</h3>
          <p>{post.body}</p>
        </div>
      ))}
    </div>
  );
}

export default Posts;
```

Explanation:

- `posts` stores API data.
- `loading` shows loading state.
- `useEffect()` runs after the component loads.
- The empty dependency array `[]` prevents repeated API calls on every render.

Flow:

```text
Component first render
        |
        v
useEffect() executes
        |
        v
fetch() calls API
        |
        v
setPosts() stores data
        |
        v
UI displays posts
```

This pattern is widely used for loading API data in React applications.

---

## 21. A parent component needs to send user details to a child component. Write React code demonstrating props-based communication.

Props allow a parent component to send data to a child component. User details such as name, email, and role can be passed as an object.

Example:

```jsx
function UserDetails({ user }) {
  return (
    <div>
      <h2>{user.name}</h2>
      <p>Email: {user.email}</p>
      <p>Role: {user.role}</p>
    </div>
  );
}

function ParentComponent() {
  const user = {
    name: "Amit",
    email: "amit@example.com",
    role: "Student"
  };

  return (
    <div>
      <h1>Profile</h1>
      <UserDetails user={user} />
    </div>
  );
}

export default ParentComponent;
```

Data flow:

```text
ParentComponent
      |
      | user object as prop
      v
UserDetails
      |
      v
Displays name, email, role
```

Important points:

- Props flow from parent to child.
- Props are read-only in the child component.
- Passing objects as props is useful for grouped data.
- Props help create reusable components.

This type of communication is commonly used in React for displaying cards, profiles, tables, and lists.

---

## 22. An application requires navigation between Home and About pages. Write code to implement React routing.

React routing can be implemented using `react-router-dom`. It allows a single-page application to navigate between pages without refreshing the browser.

Install:

```bash
npm install react-router-dom
```

Example:

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function Home() {
  return <h2>Home Page</h2>;
}

function About() {
  return <h2>About Page</h2>;
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

Working:

```text
User clicks About link
       |
       v
URL becomes /about
       |
       v
React Router matches /about
       |
       v
About component is displayed
```

Components used:

- `BrowserRouter`: provides routing support.
- `Link`: navigates without page reload.
- `Routes`: groups route definitions.
- `Route`: maps URL path to component.

React routing improves user experience by making navigation fast and smooth.

---

## 23. Multiple components need access to shared data like user login status. Write code using useContext() to manage this.

The `useContext()` hook is used to share data across multiple components without passing props manually at every level. It is useful for global data such as login status, theme, language, or current user.

Example:

```jsx
import React, { createContext, useContext, useState } from "react";

const AuthContext = createContext();

function AuthProvider({ children }) {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const login = () => setIsLoggedIn(true);
  const logout = () => setIsLoggedIn(false);

  return (
    <AuthContext.Provider value={{ isLoggedIn, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

function Navbar() {
  const { isLoggedIn, logout } = useContext(AuthContext);

  return (
    <nav>
      <span>Status: {isLoggedIn ? "Logged In" : "Guest"}</span>
      {isLoggedIn && <button onClick={logout}>Logout</button>}
    </nav>
  );
}

function LoginButton() {
  const { login } = useContext(AuthContext);

  return <button onClick={login}>Login</button>;
}

function App() {
  return (
    <AuthProvider>
      <Navbar />
      <LoginButton />
    </AuthProvider>
  );
}

export default App;
```

Flow:

```text
AuthProvider stores login state
        |
        v
Context provides state and functions
        |
        v
Navbar and LoginButton read same data
        |
        v
UI updates when login status changes
```

`useContext()` reduces prop drilling and keeps shared state accessible to related components.

---

## 24. A user registration form requires dynamic input handling. Write a React component using useState() to capture and display user inputs.

This problem can be solved by using a single state object for the form and a common change handler for all input fields. This avoids writing separate handlers for each field.

Example:

```jsx
import React, { useState } from "react";

function RegisterUser() {
  const [formData, setFormData] = useState({
    firstName: "",
    lastName: "",
    email: ""
  });

  const handleInputChange = (event) => {
    const fieldName = event.target.name;
    const fieldValue = event.target.value;

    setFormData({
      ...formData,
      [fieldName]: fieldValue
    });
  };

  return (
    <div>
      <h2>Register User</h2>

      <input
        name="firstName"
        placeholder="First Name"
        value={formData.firstName}
        onChange={handleInputChange}
      />

      <input
        name="lastName"
        placeholder="Last Name"
        value={formData.lastName}
        onChange={handleInputChange}
      />

      <input
        name="email"
        placeholder="Email"
        value={formData.email}
        onChange={handleInputChange}
      />

      <h3>Entered Details</h3>
      <p>First Name: {formData.firstName}</p>
      <p>Last Name: {formData.lastName}</p>
      <p>Email: {formData.email}</p>
    </div>
  );
}

export default RegisterUser;
```

Explanation:

- `formData` stores all form values.
- `handleInputChange()` handles all input fields.
- The `name` attribute decides which property should be updated.
- The spread operator keeps old values while updating one field.

Flow:

```text
Input field changes
        |
        v
Read event.target.name and value
        |
        v
Update matching property in formData
        |
        v
Display updated form values
```

This approach is useful in registration forms because such forms usually contain many fields.

---

## 25. A component needs to fetch and display data from an API when it loads. Write code using useEffect() to achieve this.

When a React component needs API data immediately after loading, `useEffect()` is used with an empty dependency array. The fetched data is stored in state and then rendered in the UI.

Example using `async/await`:

```jsx
import React, { useEffect, useState } from "react";

function Products() {
  const [products, setProducts] = useState([]);
  const [error, setError] = useState("");

  useEffect(() => {
    async function loadProducts() {
      try {
        const response = await fetch("https://fakestoreapi.com/products");
        const data = await response.json();
        setProducts(data);
      } catch (err) {
        setError("Unable to load products");
      }
    }

    loadProducts();
  }, []);

  return (
    <div>
      <h2>Products</h2>

      {error && <p>{error}</p>}

      {products.map((product) => (
        <div key={product.id}>
          <h3>{product.title}</h3>
          <p>Price: {product.price}</p>
        </div>
      ))}
    </div>
  );
}

export default Products;
```

Flow:

```text
Products component mounts
        |
        v
useEffect() runs once
        |
        v
loadProducts() calls API
        |
        v
Response data stored in products state
        |
        v
products.map() displays UI
```

Why dependency array is important:

```jsx
useEffect(() => {
  // API call
}, []);
```

The `[]` means the API call happens once after the first render. Without it, the effect may run repeatedly and cause unnecessary API calls.

For a high-quality API component, loading and error states should also be handled.

---

## 26. An e-commerce application requires global state for cart, user, and orders. Design a React architecture using Redux and explain how data flows between components.

In an e-commerce application, many components need shared data. For example, the navbar needs cart count, the checkout page needs cart items, the profile page needs user details, and the order page needs order history. Redux is useful because it stores global state in one centralized store.

Suggested Redux architecture:

```text
src/
  app/
    store.js
  features/
    auth/
      authSlice.js
    cart/
      cartSlice.js
    orders/
      ordersSlice.js
  components/
    Navbar.jsx
    ProductCard.jsx
  pages/
    ProductsPage.jsx
    CartPage.jsx
    CheckoutPage.jsx
    OrdersPage.jsx
```

Redux store structure:

```text
Redux Store
   |
   +-- auth
   |     +-- user
   |     +-- isLoggedIn
   |
   +-- cart
   |     +-- items
   |     +-- totalAmount
   |
   +-- orders
         +-- orderList
         +-- loading
```

Data flow:

```text
User clicks Add to Cart
        |
        v
ProductCard dispatches addToCart action
        |
        v
cartSlice reducer updates cart state
        |
        v
Redux store is updated
        |
        v
Navbar and CartPage receive updated cart data
```

Example cart slice:

```jsx
import { createSlice } from "@reduxjs/toolkit";

const cartSlice = createSlice({
  name: "cart",
  initialState: {
    items: [],
    totalAmount: 0
  },
  reducers: {
    addToCart: (state, action) => {
      state.items.push(action.payload);
      state.totalAmount += action.payload.price;
    },
    clearCart: (state) => {
      state.items = [];
      state.totalAmount = 0;
    }
  }
});

export const { addToCart, clearCart } = cartSlice.actions;
export default cartSlice.reducer;
```

Store:

```jsx
import { configureStore } from "@reduxjs/toolkit";
import cartReducer from "../features/cart/cartSlice";
import authReducer from "../features/auth/authSlice";
import ordersReducer from "../features/orders/ordersSlice";

export const store = configureStore({
  reducer: {
    cart: cartReducer,
    auth: authReducer,
    orders: ordersReducer
  }
});
```

Component usage:

```jsx
const cartItems = useSelector((state) => state.cart.items);
const dispatch = useDispatch();

dispatch(addToCart(product));
```

Benefits:

- Global state is stored in one place.
- Components do not need complex prop passing.
- Data flow is predictable.
- Debugging is easier using Redux DevTools.
- Large applications become more maintainable.

Thus, Redux provides a clean architecture for managing shared e-commerce data such as cart, user, and orders.

---

## 27. A React application has multiple pages with restricted access (only logged-in users). Design routing with protected routes and explain how access control is handled.

Protected routes are used to prevent unauthenticated users from accessing private pages. In React, this can be done using React Router and an authentication state such as `isLoggedIn`.

Architecture:

```text
App
 |
 +-- Public Routes
 |     +-- Login
 |     +-- Register
 |
 +-- Protected Routes
       +-- Dashboard
       +-- Profile
       +-- Orders
```

Protected route component:

```jsx
import { Navigate } from "react-router-dom";

function ProtectedRoute({ isLoggedIn, children }) {
  if (!isLoggedIn) {
    return <Navigate to="/login" replace />;
  }

  return children;
}

export default ProtectedRoute;
```

Routing:

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import ProtectedRoute from "./ProtectedRoute";

function App() {
  const isLoggedIn = true;

  return (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<Login />} />

        <Route
          path="/dashboard"
          element={
            <ProtectedRoute isLoggedIn={isLoggedIn}>
              <Dashboard />
            </ProtectedRoute>
          }
        />

        <Route
          path="/profile"
          element={
            <ProtectedRoute isLoggedIn={isLoggedIn}>
              <Profile />
            </ProtectedRoute>
          }
        />
      </Routes>
    </BrowserRouter>
  );
}
```

Access control flow:

```text
User opens /dashboard
        |
        v
ProtectedRoute checks login status
        |
   +----+----+
   |         |
Logged in  Not logged in
   |         |
Show       Redirect to
Dashboard  /login
```

In a real application, login status may come from:

- React Context
- Redux store
- JWT token in storage
- Backend session check

Important point: frontend protected routes improve user experience, but they are not enough for security. The backend must also protect APIs because users can bypass frontend checks.

---

## 28. A large React application becomes difficult to maintain due to poor component structure. Redesign the component architecture and explain how modularization improves scalability.

Poor component structure makes a React application hard to maintain. Problems may include large components, repeated code, unclear folder structure, too much prop passing, and mixed UI/business logic.

A better approach is to divide the application into reusable modules and components.

Suggested structure:

```text
src/
  components/
    Button/
      Button.jsx
      Button.css
    Input/
      Input.jsx
  layouts/
    MainLayout.jsx
    AuthLayout.jsx
  pages/
    HomePage.jsx
    LoginPage.jsx
    ProfilePage.jsx
  features/
    auth/
      LoginForm.jsx
      authService.js
      authSlice.js
    products/
      ProductList.jsx
      ProductCard.jsx
      productService.js
  hooks/
    useAuth.js
    useProducts.js
  services/
    apiClient.js
  routes/
    AppRoutes.jsx
```

Component hierarchy:

```text
App
 |
 +-- AppRoutes
      |
      +-- Layout
           |
           +-- Page
                |
                +-- Feature Components
                     |
                     +-- Reusable UI Components
```

Modularization principles:

- Keep components small and focused.
- Separate pages from reusable components.
- Keep API logic in services.
- Keep shared logic in custom hooks.
- Group feature-specific files together.
- Use Context or Redux for shared state when needed.

Example separation:

```text
ProductPage
   |
   +-- ProductList
          |
          +-- ProductCard
                 |
                 +-- Button
```

Benefits:

- Easier to understand code.
- Easier to reuse components.
- Easier to test individual parts.
- Team members can work on separate features.
- Changes in one module affect fewer files.
- Large applications become scalable and maintainable.

For example, instead of writing product display, cart logic, and API calls in one large component, the application should use `ProductCard`, `CartSlice`, and `productService`. This keeps responsibilities clear.

Thus, modular component architecture improves scalability by organizing code according to responsibility and feature area.

---

## 29. An Angular application needs to manage multiple modules like Admin, User, and Reports. Design the module structure and explain how Angular modules improve organization.

Angular modules are used to organize an application into functional blocks. Each module can contain components, services, routes, pipes, and directives related to a specific feature.

For an application with Admin, User, and Reports sections, a feature module structure is suitable.

Suggested structure:

```text
src/app/
  app.module.ts
  app-routing.module.ts

  core/
    auth.service.ts
    auth.guard.ts
    api.service.ts

  shared/
    components/
      button/
      table/
    pipes/
    directives/
    shared.module.ts

  admin/
    admin.module.ts
    admin-routing.module.ts
    dashboard/
    manage-users/

  user/
    user.module.ts
    user-routing.module.ts
    profile/
    settings/

  reports/
    reports.module.ts
    reports-routing.module.ts
    sales-report/
    user-report/
```

Module relationship:

```text
AppModule
   |
   +-- CoreModule
   +-- SharedModule
   +-- AdminModule
   +-- UserModule
   +-- ReportsModule
```

Example feature module:

```ts
@NgModule({
  declarations: [
    AdminDashboardComponent,
    ManageUsersComponent
  ],
  imports: [
    CommonModule,
    AdminRoutingModule,
    SharedModule
  ]
})
export class AdminModule {}
```

Benefits of Angular modules:

- They organize related code in one place.
- They make large applications easier to maintain.
- They support lazy loading for better performance.
- They separate responsibilities by feature.
- They improve team collaboration.
- Shared components can be reused through `SharedModule`.

Lazy loading example:

```ts
{
  path: "admin",
  loadChildren: () =>
    import("./admin/admin.module").then((m) => m.AdminModule)
}
```

With lazy loading, the Admin module loads only when the user opens the admin route. This reduces the initial bundle size.

Thus, Angular modules improve organization, performance, and scalability in large frontend applications.

---

## 30. A form-heavy Angular application faces frequent validation errors and poor user experience. Design a solution using Angular forms and validation techniques.

For a form-heavy Angular application, a structured form design is needed. Angular reactive forms are suitable because they provide strong control over validation, dynamic fields, and error handling.

Suggested solution:

```text
Reactive Form
     |
     +-- FormGroup for full form
     +-- FormControl for each input
     +-- Validators for rules
     +-- Error messages in template
     +-- Disable submit until valid
```

Component:

```ts
import { Component } from "@angular/core";
import { FormBuilder, Validators } from "@angular/forms";

@Component({
  selector: "app-register",
  templateUrl: "./register.component.html"
})
export class RegisterComponent {
  registerForm = this.fb.group({
    name: ["", [Validators.required, Validators.minLength(3)]],
    email: ["", [Validators.required, Validators.email]],
    password: ["", [Validators.required, Validators.minLength(6)]]
  });

  constructor(private fb: FormBuilder) {}

  submitForm() {
    if (this.registerForm.valid) {
      console.log(this.registerForm.value);
    } else {
      this.registerForm.markAllAsTouched();
    }
  }
}
```

Template:

```html
<form [formGroup]="registerForm" (ngSubmit)="submitForm()">
  <label>Name</label>
  <input formControlName="name" />
  <p *ngIf="registerForm.get('name')?.touched && registerForm.get('name')?.invalid">
    Name must contain at least 3 characters
  </p>

  <label>Email</label>
  <input formControlName="email" />
  <p *ngIf="registerForm.get('email')?.touched && registerForm.get('email')?.invalid">
    Enter a valid email
  </p>

  <label>Password</label>
  <input type="password" formControlName="password" />
  <p *ngIf="registerForm.get('password')?.touched && registerForm.get('password')?.invalid">
    Password must contain at least 6 characters
  </p>

  <button type="submit" [disabled]="registerForm.invalid">
    Register
  </button>
</form>
```

Validation flow:

```text
User enters input
      |
      v
FormControl validates value
      |
      v
FormGroup calculates overall validity
      |
      v
Show field-level error messages
      |
      v
Submit only if form is valid
```

Techniques to improve user experience:

- Show validation messages only after field is touched.
- Disable submit button for invalid forms.
- Use `markAllAsTouched()` after failed submit.
- Use clear error messages.
- Use custom validators for rules like password match.
- Group large forms into smaller sections.
- Use reactive forms for complex validation.

This approach reduces validation errors and gives users immediate guidance while filling the form.

---

## 31. An Angular application requires communication between unrelated components. Design a solution using services and dependency injection and explain the data flow.

Unrelated components do not have a direct parent-child relationship. Therefore, `@Input()` and `@Output()` are not suitable. In Angular, a shared service with dependency injection is commonly used for communication between unrelated components.

Scenario:

```text
HeaderComponent needs to show selected user
UserListComponent selects a user

These components are unrelated.
```

Shared service:

```ts
import { Injectable } from "@angular/core";
import { BehaviorSubject } from "rxjs";

@Injectable({
  providedIn: "root"
})
export class UserStateService {
  private selectedUserSource = new BehaviorSubject<string>("No user selected");
  selectedUser$ = this.selectedUserSource.asObservable();

  updateSelectedUser(name: string): void {
    this.selectedUserSource.next(name);
  }
}
```

Component that updates data:

```ts
import { Component } from "@angular/core";
import { UserStateService } from "./user-state.service";

@Component({
  selector: "app-user-list",
  template: `
    <button (click)="selectUser('Amit')">Select Amit</button>
    <button (click)="selectUser('Neha')">Select Neha</button>
  `
})
export class UserListComponent {
  constructor(private userStateService: UserStateService) {}

  selectUser(name: string): void {
    this.userStateService.updateSelectedUser(name);
  }
}
```

Component that receives data:

```ts
import { Component } from "@angular/core";
import { UserStateService } from "./user-state.service";

@Component({
  selector: "app-header",
  template: "<h2>Selected User: {{ selectedUser }}</h2>"
})
export class HeaderComponent {
  selectedUser = "";

  constructor(private userStateService: UserStateService) {
    this.userStateService.selectedUser$.subscribe((name) => {
      this.selectedUser = name;
    });
  }
}
```

Data flow:

```text
UserListComponent
      |
      | calls updateSelectedUser()
      v
Shared UserStateService
      |
      | BehaviorSubject emits new value
      v
HeaderComponent receives updated value
      |
      v
UI displays selected user
```

Why this works:

- The service is provided at root level, so Angular creates one shared instance.
- Both unrelated components receive the same service instance through dependency injection.
- `BehaviorSubject` stores the latest value and notifies subscribers when data changes.

Benefits:

- Avoids complex parent-child prop passing.
- Keeps shared state in one place.
- Makes components loosely coupled.
- Improves maintainability.
- Works well for cross-component communication.

Thus, Angular services with dependency injection provide a clean solution for communication between unrelated components.

