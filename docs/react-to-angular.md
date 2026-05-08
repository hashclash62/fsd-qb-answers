# React → Angular: A Mental Model for React Developers

> You already know how to think in components. Angular is just a different *dialect* of that same language.

---

## Part 1: The Mental Model — Same Concept, Different Syntax

### The Core Idea

Both React and Angular are **component-based UI frameworks**. Every concept you know in React has a direct Angular counterpart. The difference is mostly in **where things live** and **how they're declared**.

```
React                          Angular
─────────────────────────────────────────────────────
JSX file (.tsx)           →   3 files: .ts + .html + .css
useState()                →   class property (+ signals/NgRx)
useEffect()               →   ngOnInit(), ngOnDestroy()
Props (parent → child)    →   @Input()
Callback props (child → parent) → @Output() + EventEmitter
Context API               →   Services + Dependency Injection
React Router              →   Angular Router (built-in)
Custom Hooks              →   Services
Conditional rendering     →   *ngIf / @if
List rendering (.map())   →   *ngFor / @for
className                 →   class
onClick={handler}         →   (click)="handler()"
value={state}             →   [value]="property"
onChange={handler}        →   (change)="handler($event)"
Two-way: value + onChange  →  [(ngModel)]="property"
```

---

## Part 2: Anatomy Comparison

### React Component Anatomy

```
MyComponent.tsx
┌─────────────────────────────────────────┐
│  imports                                │
│  interface Props { ... }               │
│                                         │
│  function MyComponent(props) {          │
│    const [state, setState] = useState() │  ← state
│                                         │
│    useEffect(() => { ... }, [])         │  ← lifecycle
│                                         │
│    const handleClick = () => { ... }    │  ← event handler
│                                         │
│    return (                             │
│      <div className="...">             │  ← template (JSX)
│        {state}                          │
│      </div>                             │
│    )                                    │
│  }                                      │
└─────────────────────────────────────────┘
```

### Angular Component Anatomy

```
my-component/
├── my-component.component.ts       ← logic + metadata
├── my-component.component.html     ← template (separate file)
└── my-component.component.css      ← styles (scoped)

my-component.component.ts
┌─────────────────────────────────────────────────────┐
│  imports                                            │
│                                                     │
│  @Component({                                       │
│    selector: 'app-my-component',   ← HTML tag name  │
│    templateUrl: './...',           ← template file  │
│    styleUrls: ['./...']            ← styles file    │
│  })                                                 │
│  export class MyComponent implements OnInit {       │
│    state = 'value'                 ← class property │
│                                                     │
│    @Input() propFromParent = ''    ← like props     │
│    @Output() eventToParent =       ← like callbacks │
│      new EventEmitter()                             │
│                                                     │
│    ngOnInit() { ... }              ← like useEffect(,[]) │
│    ngOnDestroy() { ... }           ← cleanup        │
│                                                     │
│    handleClick() { ... }           ← event handler  │
│  }                                                  │
└─────────────────────────────────────────────────────┘
```

---

## Part 3: Concept-by-Concept Mapping

### 3.1 State

**React** — `useState` hook returns a value and a setter:
```tsx
const [count, setCount] = useState(0);
setCount(count + 1);
```

**Angular** — just a class property. Mutate it directly:
```ts
count = 0;
increment() {
  this.count++;           // Angular's change detection picks it up
}
```

> **Mental model**: In React, you *replace* state with a setter. In Angular, you *mutate* a class property and the framework notices.

---

### 3.2 Props (Parent → Child Data Flow)

**React**:
```tsx
// Parent
<UserCard name="Alice" age={25} />

// Child
function UserCard({ name, age }: { name: string; age: number }) {
  return <p>{name} is {age}</p>;
}
```

**Angular**:
```ts
// Child component
@Input() name: string = '';
@Input() age: number = 0;

// Child template
<p>{{ name }} is {{ age }}</p>

// Parent template
<app-user-card [name]="'Alice'" [age]="25" />
```

> **Mental model**: Props in React = `@Input()` in Angular. Square brackets `[prop]` in the parent template = passing a dynamic value (like JSX curly braces `{}`).

---

### 3.3 Callbacks (Child → Parent Data Flow)

**React**:
```tsx
// Parent
<Button onClick={(val) => console.log(val)} />

// Child
function Button({ onClick }) {
  return <button onClick={() => onClick('clicked!')}>Click</button>;
}
```

**Angular**:
```ts
// Child
@Output() clicked = new EventEmitter<string>();
onButtonClick() {
  this.clicked.emit('clicked!');
}

// Child template
<button (click)="onButtonClick()">Click</button>

// Parent template
<app-button (clicked)="handleIt($event)" />

// Parent class
handleIt(val: string) { console.log(val); }
```

> **Mental model**: Callback props in React = `@Output() + EventEmitter` in Angular. The `(event)` syntax in parent template listens to it — like `onClick={}` in JSX.

---

### 3.4 Lifecycle Methods

**React**:
```tsx
useEffect(() => {
  // on mount
  fetchData();

  return () => {
    // on unmount (cleanup)
    cleanup();
  };
}, []); // empty array = run once
```

**Angular**:
```ts
export class MyComponent implements OnInit, OnDestroy {
  ngOnInit() {
    this.fetchData();    // on mount
  }

  ngOnDestroy() {
    this.cleanup();      // on unmount
  }
}
```

| React Hook | Angular Lifecycle |
|---|---|
| `useEffect(fn, [])` | `ngOnInit()` |
| cleanup in `useEffect` return | `ngOnDestroy()` |
| `useEffect(fn, [dep])` | `ngOnChanges()` |
| `useEffect(fn)` | `ngDoCheck()` |

---

### 3.5 Template Syntax: Binding

| Purpose | React (JSX) | Angular (HTML) |
|---|---|---|
| Show variable | `{count}` | `{{ count }}` |
| Bind attribute | `value={x}` | `[value]="x"` |
| Listen to event | `onClick={fn}` | `(click)="fn()"` |
| Two-way binding | `value={x} onChange={setX}` | `[(ngModel)]="x"` |
| Dynamic class | `className={cond ? 'a':'b'}` | `[class.active]="cond"` |
| Inline style | `style={{ color: 'red' }}` | `[style.color]="'red'"` |

---

### 3.6 Conditional Rendering

**React**:
```tsx
{isLoggedIn && <Dashboard />}
{isLoggedIn ? <Dashboard /> : <Login />}
```

**Angular** (old syntax with directives):
```html
<app-dashboard *ngIf="isLoggedIn"></app-dashboard>
<app-login *ngIf="!isLoggedIn"></app-login>
```

**Angular** (new @if syntax — Angular 17+):
```html
@if (isLoggedIn) {
  <app-dashboard />
} @else {
  <app-login />
}
```

---

### 3.7 List Rendering

**React**:
```tsx
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}
```

**Angular** (old):
```html
<li *ngFor="let item of items; trackBy: trackById">
  {{ item.name }}
</li>
```

**Angular** (new @for syntax — Angular 17+):
```html
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
}
```

> **Mental model**: React's `.map()` with a `key` = Angular's `*ngFor` with `trackBy`. Both exist for the same performance reason: tracking identity during re-renders.

---

### 3.8 Services = Custom Hooks + Context

**React** — you share logic via custom hooks and global state via Context:
```tsx
// Custom hook
function useCounter() {
  const [count, setCount] = useState(0);
  return { count, increment: () => setCount(c => c + 1) };
}
```

**Angular** — you share logic via **Services** injected via **Dependency Injection**:
```ts
@Injectable({ providedIn: 'root' })  // 'root' = singleton, like Context
export class CounterService {
  count = 0;
  increment() { this.count++; }
}

// In a component:
constructor(private counter: CounterService) {}
ngOnInit() { console.log(this.counter.count); }
```

> **Mental model**: Angular Services ≈ React custom hooks + Context combined. `providedIn: 'root'` makes it a global singleton (like Context). Injected per-component = local scope (like a hook).

---

## Part 4: Full Side-by-Side Code Example

We'll build a **User Task Manager** component that covers:
- State management
- Props / Inputs
- Callback / Output
- Lifecycle (fetch on mount)
- Conditional rendering
- List rendering
- Two-way form binding
- Event handling

---

### React Version

**`TaskManager.tsx`** — Parent + orchestration
```tsx
import { useState, useEffect } from 'react';
import TaskList from './TaskList';
import AddTaskForm from './AddTaskForm';

interface Task {
  id: number;
  title: string;
  done: boolean;
}

export default function TaskManager() {
  const [tasks, setTasks] = useState<Task[]>([]);
  const [loading, setLoading] = useState(true);
  const [filter, setFilter] = useState<'all' | 'done' | 'pending'>('all');

  // Lifecycle: fetch on mount
  useEffect(() => {
    setTimeout(() => {
      setTasks([
        { id: 1, title: 'Learn Angular', done: false },
        { id: 2, title: 'Build a project', done: false },
        { id: 3, title: 'Pass the exam', done: true },
      ]);
      setLoading(false);
    }, 800);
  }, []);

  // Add task (callback from child)
  const handleAddTask = (title: string) => {
    const newTask: Task = { id: Date.now(), title, done: false };
    setTasks(prev => [...prev, newTask]);
  };

  // Toggle done
  const handleToggle = (id: number) => {
    setTasks(prev =>
      prev.map(t => t.id === id ? { ...t, done: !t.done } : t)
    );
  };

  // Delete task
  const handleDelete = (id: number) => {
    setTasks(prev => prev.filter(t => t.id !== id));
  };

  const filteredTasks = tasks.filter(t => {
    if (filter === 'done') return t.done;
    if (filter === 'pending') return !t.done;
    return true;
  });

  return (
    <div className="task-manager">
      <h1>Task Manager</h1>

      {/* Conditional rendering */}
      {loading ? (
        <p>Loading tasks...</p>
      ) : (
        <>
          {/* Filter buttons */}
          <div className="filters">
            {(['all', 'done', 'pending'] as const).map(f => (
              <button
                key={f}
                className={filter === f ? 'active' : ''}
                onClick={() => setFilter(f)}
              >
                {f}
              </button>
            ))}
          </div>

          {/* Child component: form — callback via prop */}
          <AddTaskForm onAdd={handleAddTask} />

          {/* Child component: list — props + callbacks */}
          <TaskList
            tasks={filteredTasks}
            onToggle={handleToggle}
            onDelete={handleDelete}
          />

          {/* Inline conditional */}
          {filteredTasks.length === 0 && <p>No tasks found.</p>}
        </>
      )}
    </div>
  );
}
```

**`AddTaskForm.tsx`** — Child with form state + callback
```tsx
import { useState } from 'react';

interface Props {
  onAdd: (title: string) => void;
}

export default function AddTaskForm({ onAdd }: Props) {
  const [input, setInput] = useState('');   // local state for input

  const handleSubmit = () => {
    if (!input.trim()) return;
    onAdd(input.trim());                    // fire callback to parent
    setInput('');
  };

  return (
    <div className="add-form">
      {/* Controlled input: value + onChange = two-way binding in React */}
      <input
        type="text"
        value={input}
        onChange={e => setInput(e.target.value)}
        placeholder="New task..."
      />
      <button onClick={handleSubmit}>Add</button>
    </div>
  );
}
```

**`TaskList.tsx`** — Child for rendering list
```tsx
interface Task { id: number; title: string; done: boolean; }

interface Props {
  tasks: Task[];
  onToggle: (id: number) => void;
  onDelete: (id: number) => void;
}

export default function TaskList({ tasks, onToggle, onDelete }: Props) {
  return (
    <ul className="task-list">
      {/* List rendering with .map() */}
      {tasks.map(task => (
        <li key={task.id} className={task.done ? 'done' : ''}>
          <span onClick={() => onToggle(task.id)}>{task.title}</span>
          <button onClick={() => onDelete(task.id)}>✕</button>
        </li>
      ))}
    </ul>
  );
}
```

---

### Angular Version

**`task.interface.ts`** — Shared type
```ts
export interface Task {
  id: number;
  title: string;
  done: boolean;
}
```

**`task-manager.component.ts`** — Parent component
```ts
import { Component, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Task } from './task.interface';
import { TaskListComponent } from './task-list/task-list.component';
import { AddTaskFormComponent } from './add-task-form/add-task-form.component';

@Component({
  selector: 'app-task-manager',
  standalone: true,                         // no NgModule needed (Angular 17+)
  imports: [CommonModule, TaskListComponent, AddTaskFormComponent],
  templateUrl: './task-manager.component.html',
})
export class TaskManagerComponent implements OnInit {

  // State = class properties (no useState needed)
  tasks: Task[] = [];
  loading = true;
  filter: 'all' | 'done' | 'pending' = 'all';

  // Lifecycle: runs once on mount — equivalent to useEffect(fn, [])
  ngOnInit(): void {
    setTimeout(() => {
      this.tasks = [
        { id: 1, title: 'Learn Angular', done: false },
        { id: 2, title: 'Build a project', done: false },
        { id: 3, title: 'Pass the exam', done: true },
      ];
      this.loading = false;
    }, 800);
  }

  // Callback from AddTaskForm child via @Output EventEmitter
  handleAddTask(title: string): void {
    const newTask: Task = { id: Date.now(), title, done: false };
    this.tasks = [...this.tasks, newTask];  // immutable update (same as React)
  }

  handleToggle(id: number): void {
    this.tasks = this.tasks.map(t =>
      t.id === id ? { ...t, done: !t.done } : t
    );
  }

  handleDelete(id: number): void {
    this.tasks = this.tasks.filter(t => t.id !== id);
  }

  // Computed value — a getter instead of useMemo
  get filteredTasks(): Task[] {
    if (this.filter === 'done') return this.tasks.filter(t => t.done);
    if (this.filter === 'pending') return this.tasks.filter(t => !t.done);
    return this.tasks;
  }

  setFilter(f: 'all' | 'done' | 'pending'): void {
    this.filter = f;
  }
}
```

**`task-manager.component.html`** — Parent template
```html
<div class="task-manager">
  <h1>Task Manager</h1>

  <!-- Conditional rendering: *ngIf or @if (Angular 17+) -->
  @if (loading) {
    <p>Loading tasks...</p>
  } @else {

    <!-- Filter buttons: event binding with (click) -->
    <div class="filters">
      @for (f of ['all', 'done', 'pending']; track f) {
        <button
          [class.active]="filter === f"
          (click)="setFilter(f)"
        >
          {{ f }}
        </button>
      }
    </div>

    <!-- Child component with @Output listener: (taskAdded) = prop callback -->
    <app-add-task-form (taskAdded)="handleAddTask($event)" />

    <!-- Child component with @Input + @Output -->
    <app-task-list
      [tasks]="filteredTasks"
      (toggled)="handleToggle($event)"
      (deleted)="handleDelete($event)"
    />

    @if (filteredTasks.length === 0) {
      <p>No tasks found.</p>
    }

  }
</div>
```

---

**`add-task-form.component.ts`** — Child with two-way binding + Output
```ts
import { Component, Output, EventEmitter } from '@angular/core';
import { FormsModule } from '@angular/forms';   // needed for ngModel

@Component({
  selector: 'app-add-task-form',
  standalone: true,
  imports: [FormsModule],
  templateUrl: './add-task-form.component.html',
})
export class AddTaskFormComponent {
  input = '';   // local state — equivalent to useState('')

  // @Output = callback prop in React
  @Output() taskAdded = new EventEmitter<string>();

  handleSubmit(): void {
    if (!this.input.trim()) return;
    this.taskAdded.emit(this.input.trim());   // fire event to parent
    this.input = '';
  }
}
```

**`add-task-form.component.html`**
```html
<div class="add-form">
  <!--
    [(ngModel)] = two-way binding
    React equivalent: value={input} onChange={e => setInput(e.target.value)}
    The [()] "banana in a box" syntax = [property binding] + (event binding)
  -->
  <input
    type="text"
    [(ngModel)]="input"
    placeholder="New task..."
  />
  <button (click)="handleSubmit()">Add</button>
</div>
```

---

**`task-list.component.ts`** — Child for list rendering
```ts
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Task } from '../task.interface';

@Component({
  selector: 'app-task-list',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './task-list.component.html',
})
export class TaskListComponent {
  // @Input() = props in React
  @Input() tasks: Task[] = [];

  // @Output() = callback props in React
  @Output() toggled = new EventEmitter<number>();
  @Output() deleted = new EventEmitter<number>();
}
```

**`task-list.component.html`**
```html
<ul class="task-list">
  <!-- *ngFor = .map() in React; track = key prop -->
  @for (task of tasks; track task.id) {
    <li [class.done]="task.done">
      <span (click)="toggled.emit(task.id)">{{ task.title }}</span>
      <button (click)="deleted.emit(task.id)">✕</button>
    </li>
  }
</ul>
```

---

## Part 5: Key Differences — The "Gotchas"

### 1. File Structure
- **React**: One `.tsx` file = everything (logic + template + optional CSS-in-JS)
- **Angular**: One component = 3 files (`.ts` + `.html` + `.css`). You can use inline templates but the convention is separate files.

### 2. Change Detection
- **React**: You *must* call a setter (`setState`, `setCount`) to trigger a re-render. Mutating state directly breaks React.
- **Angular**: You mutate class properties directly (`this.count++`) and Angular's **Zone.js** detects the change automatically. You can also use `ChangeDetectionStrategy.OnPush` for React-like behavior.

### 3. Modules vs Standalone Components
- Older Angular (< v14) required every component to be declared inside an `NgModule`. This had no React equivalent.
- Modern Angular (v17+) supports **standalone components** — just `standalone: true` in `@Component`. This is closer to how React works. For your exam, know both but prefer standalone.

### 4. Template Syntax is Strict HTML
- React JSX is JavaScript first — you can write any JS expression inline: `{items.filter(...).map(...)}`
- Angular templates are HTML first — you can only use **template expressions** (no arbitrary JS). Logic goes in the component class. This forces cleaner separation.

### 5. Dependency Injection (No React Equivalent)
Angular has a built-in DI system. You don't `import` a service manually — you declare it in the constructor and Angular provides it:
```ts
// Angular
constructor(private userService: UserService) {}

// React equivalent (kind of)
const userService = useContext(UserServiceContext);
```

### 6. Two-Way Binding
- React has no built-in two-way binding. You wire `value` + `onChange` manually (controlled inputs).
- Angular has `[(ngModel)]` which does both automatically. The `[()]` syntax = "banana in a box" = property binding + event binding combined.

### 7. TypeScript is Mandatory
React works with plain JS (though TS is recommended). Angular **requires** TypeScript — the framework is built with and for TypeScript.

---

## Part 6: Quick Reference Card

```
┌─────────────────────────────────┬──────────────────────────────────────┐
│ React                           │ Angular                              │
├─────────────────────────────────┼──────────────────────────────────────┤
│ function Component() {}         │ export class Component {}            │
│ useState(val)                   │ property = val                       │
│ setState(newVal)                │ this.property = newVal               │
│ useEffect(fn, [])               │ ngOnInit()                           │
│ useEffect cleanup               │ ngOnDestroy()                        │
│ useMemo(() => val, [deps])      │ get computedProp() { return val; }   │
│ props.value                     │ @Input() value                       │
│ props.onEvent(data)             │ @Output() onEvent = new EventEmitter │
│                                 │ this.onEvent.emit(data)              │
│ {variable}                      │ {{ variable }}                       │
│ <tag prop={val}>                │ <tag [prop]="val">                   │
│ <tag onClick={fn}>              │ <tag (click)="fn()">                 │
│ value={x} onChange={setX}       │ [(ngModel)]="x"                      │
│ {cond && <Comp />}              │ @if (cond) { <app-comp /> }          │
│ items.map(i => <Li key={i.id}>) │ @for (i of items; track i.id) { }   │
│ className="active"              │ class="active"                       │
│ [class.active]="cond"           │ [class.active]="cond"  ✓ same!      │
│ Custom Hook                     │ Service                              │
│ Context Provider                │ Service with providedIn: 'root'      │
│ React Router                    │ @angular/router (built-in)           │
└─────────────────────────────────┴──────────────────────────────────────┘
```

---

## Part 7: What to Focus on for Your Exam

1. **`@Component` decorator** — know `selector`, `templateUrl`, `styleUrls`, `standalone`, `imports`
2. **`@Input()` and `@Output()`** — the most-tested data flow concept
3. **Template syntax** — `{{ }}`, `[ ]`, `( )`, `[( )]` — know each bracket type's meaning
4. **`*ngFor` and `*ngIf`** — classic exam staples (also know `@if`/`@for` for modern Angular)
5. **Lifecycle hooks** — especially `ngOnInit` and `ngOnDestroy`
6. **Services + DI** — injecting a service in a constructor
7. **`FormsModule` / `ReactiveFormsModule`** — needed for `ngModel` and form controls
8. **`CommonModule`** — needed for `*ngIf`, `*ngFor` in older code

---

*The bottom line: Angular is more opinionated and structured than React. React gives you freedom; Angular gives you a rulebook. Once you accept that and map each concept you already know to its Angular counterpart, the transition is mostly about learning new syntax — not new thinking.*
