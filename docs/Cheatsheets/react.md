
# React

A quick reference for modern React (v18+). Covers core concepts, hooks, state management, context, performance, and testing.

---

## ⚙️ Basics

### JSX
- Combines **JavaScript + HTML-like syntax**
- Must return **one parent element**
- Use `{}` for embedding JS expressions

```jsx
const name = "John";
const element = <h1>Hello, {name}!</h1>;
````

### Components

React apps are built from **components** (functions or classes).

#### Function Component

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

#### Class Component (legacy)

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Props

* **Read-only** data passed from parent → child

```jsx
function Greeting({ name }) {
  return <p>Hello {name}</p>;
}

<Greeting name="Alice" />
```

### Conditional Rendering

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
{notifications.length > 0 && <Alert />}
```

### Lists & Keys

```jsx
<ul>
  {items.map((item) => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>
```

---

## 🔁 State and Lifecycle

### useState

Manages local state in a function component.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>Count: {count}</button>
  );
}
```

### useEffect

Performs side effects (fetching, subscriptions, timers, etc.).

```jsx
import { useEffect } from "react";

useEffect(() => {
  console.log("Component mounted");
  return () => console.log("Cleanup");
}, []); // dependencies
```

Common patterns:

* `[]` → run once (on mount)
* `[dep]` → run when dep changes
* no array → run after every render

---

## 🪝 Common Hooks

| Hook              | Purpose                                   |
| ----------------- | ----------------------------------------- |
| `useState`        | Manage local component state              |
| `useEffect`       | Perform side effects                      |
| `useContext`      | Access context values                     |
| `useRef`          | Access DOM or persist mutable values      |
| `useMemo`         | Memoize expensive computations            |
| `useCallback`     | Memoize functions                         |
| `useReducer`      | Alternative to useState for complex logic |
| `useLayoutEffect` | Like `useEffect` but fires before paint   |
| `useId`           | Generate stable unique IDs                |

---

## ⚡ Handling Events

```jsx
<button onClick={() => alert("Clicked!")}>Click me</button>
```

Passing event objects:

```jsx
function handleClick(e) {
  e.preventDefault();
  console.log("Button clicked");
}
```

---

## 📋 Forms

### Controlled Components

```jsx
const [value, setValue] = useState("");

<input value={value} onChange={(e) => setValue(e.target.value)} />
```

### Multiple Inputs

```jsx
const [form, setForm] = useState({ name: "", email: "" });

function handleChange(e) {
  setForm({ ...form, [e.target.name]: e.target.value });
}

<input name="name" onChange={handleChange} />
<input name="email" onChange={handleChange} />
```

---

## 🧩 Context API

### Creating and Using Context

```jsx
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Styled</button>;
}
```

---

## 🧠 Custom Hooks

Encapsulate reusable logic.

```jsx
function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);
  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize", handleResize);
    return () => window.removeEventListener("resize", handleResize);
  }, []);
  return width;
}
```

Usage:

```jsx
const width = useWindowWidth();
```

---

## 🚀 Performance Optimization

### Memoization

```jsx
import { memo, useMemo, useCallback } from "react";

const ExpensiveComponent = memo(({ data }) => {
  console.log("Rendered");
  return <div>{data.value}</div>;
});
```

### useMemo Example

```jsx
const sorted = useMemo(() => items.sort(compareFn), [items]);
```

### useCallback Example

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

---

## 🌐 Data Fetching

### useEffect + fetch

```jsx
const [data, setData] = useState([]);

useEffect(() => {
  fetch("/api/users")
    .then((res) => res.json())
    .then(setData);
}, []);
```

### With async/await

```jsx
useEffect(() => {
  const load = async () => {
    const res = await fetch("/api/users");
    setData(await res.json());
  };
  load();
}, []);
```

---

## 🧭 Routing (React Router v6+)

```bash
npm install react-router-dom
```

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

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
```

---

## 🧰 State Management (Context + Reducer Example)

```jsx
const CounterContext = createContext();

function reducer(state, action) {
  switch (action.type) {
    case "increment": return { count: state.count + 1 };
    case "decrement": return { count: state.count - 1 };
    default: return state;
  }
}

function CounterProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
}

function Counter() {
  const { state, dispatch } = useContext(CounterContext);
  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </>
  );
}
```

---

## 🧱 Refs and DOM Access

```jsx
const inputRef = useRef();

function focusInput() {
  inputRef.current.focus();
}

<input ref={inputRef} />
<button onClick={focusInput}>Focus</button>
```

---

## 🧪 Testing (React Testing Library)

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

```jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Counter from "./Counter";

test("increments counter", async () => {
  render(<Counter />);
  await userEvent.click(screen.getByText("Increment"));
  expect(screen.getByText("Count: 1")).toBeInTheDocument();
});
```

---

## 🧩 Common Patterns

### Conditional ClassNames

```jsx
<div className={active ? "active" : "inactive"} />
```

### Destructuring Props

```jsx
function User({ name, email }) {
  return <p>{name} - {email}</p>;
}
```

### Default Props

```jsx
User.defaultProps = {
  name: "Anonymous",
};
```

---

## 🧱 File Structure Example

```
src/
  components/
    Header.jsx
    Footer.jsx
  hooks/
    useFetch.js
  context/
    AuthContext.js
  pages/
    Home.jsx
    About.jsx
  App.jsx
  index.jsx
```

---

## 🧰 Useful Packages

| Package                          | Purpose                      |
| -------------------------------- | ---------------------------- |
| `react-router-dom`               | Routing                      |
| `axios`                          | HTTP client                  |
| `classnames`                     | Conditional class management |
| `zustand` / `jotai` / `redux`    | State management             |
| `react-query` / `tanstack-query` | Server-state management      |
| `framer-motion`                  | Animations                   |
| `react-hook-form`                | Form management              |

---

## 🧾 References

* [React Docs (react.dev)](https://react.dev/)
* [React Router](https://reactrouter.com/)
* [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
* [Hooks API Reference](https://react.dev/reference/react)

---
