# React

### General React Questions

---

### 1. What's the difference between a functional component and a class component?

**Functional components** are JavaScript functions that return JSX. They're simpler to write and read and are generally preferred in modern React because they leverage **React Hooks** to manage state and side effects.

**Class components** are ES6 classes that extend `React.Component` and have a `render()` method that returns JSX. They're stateful and were traditionally used for more complex components, but they've been largely superseded by functional components and Hooks.

### 2. What are React Hooks and why are they so important?

**React Hooks** are functions that let you "hook into" React state and lifecycle features from functional components. They were introduced to allow developers to use state and other React features without writing a class.

Key Hooks include:

- `useState()`: For managing state in a functional component.
- `useEffect()`: For handling side effects like data fetching, subscriptions, or manually changing the DOM.
- `useContext()`: For accessing the **React Context API**, which avoids **prop drilling**.

Hooks simplify code, make it more reusable, and improve the overall readability of components.

### 3. What is JSX?

**JSX** (JavaScript XML) is a syntax extension for JavaScript. It looks a lot like HTML but is used to describe what the UI should look like in a declarative way. React then compiles the JSX into standard JavaScript function calls (like `React.createElement()`).

For example, `<h1>Hello, world!</h1>` is a JSX expression that gets transformed into `React.createElement('h1', null, 'Hello, world!')`.

### 4. What's the virtual DOM?

The **virtual DOM** is a lightweight, in-memory representation of the actual DOM. When state changes in a React component, React first updates this virtual representation. It then uses a process called **diffing** to compare the new virtual DOM with the old one, determining the minimal set of changes needed. Finally, it applies only those changes to the real DOM, which is much faster than manipulating the real DOM directly. This efficient update mechanism is a core reason for React's performance.

### 5. What are props in React?

**Props** (short for properties) are a way to pass data from a parent component down to a child component. They are read-only, meaning a child component cannot change the props it receives. This one-way data flow is a fundamental part of React's architecture and helps maintain a predictable state.

### React State Management and Performance

---

### 6. Explain the concept of state in React.

**State** is a JavaScript object that stores a component's dynamic data. Unlike props, the state is managed within the component itself and can be updated by the component. When a component's state changes, React automatically re-renders the component to reflect the new state. The `useState()` Hook is the standard way to manage state in a functional component.

### 7. What is **prop drilling** and how can you avoid it?

**Prop drilling** is the process of passing data through multiple layers of nested components, even if some of those components don't need the data themselves. It can make code harder to read, maintain, and debug.

You can avoid prop drilling using:

- **The Context API**: This allows you to create a global state that can be accessed by any component within a certain tree without having to pass props explicitly.
- **State management libraries**: Libraries like **Redux** or **Zustand** are designed to manage application-wide state in a more structured way, making it accessible to any component that needs it.

### 8. What's the purpose of the `key` prop when rendering a list of elements?

The `key` prop is a special string attribute you must include when creating lists of elements in React. React uses the key to uniquely identify each element in the list. This helps React efficiently update the UI. Without a unique key, React might have trouble keeping track of which items have changed, been added, or been removed, which can lead to performance issues and unexpected behavior. The `key` should be a unique identifier, like an ID from a database.

### 9. How do you optimize the performance of a React application?

Key performance optimization techniques include:

- **Memoization**: Using `React.memo()`, `useCallback()`, and `useMemo()` to prevent unnecessary re-renders of components, functions, or expensive calculations.
- **Code splitting**: Splitting your code into smaller chunks that are loaded only when needed, which reduces the initial load time.
- **Lazy loading**: Using `React.lazy()` to load components only when they're rendered, which also helps with initial load time.
- **List virtualization**: For very long lists, only rendering the items that are currently visible in the viewport.

### Advanced Topics

---

### 10. What is React Context?

The **React Context API** is a feature that provides a way to share state and data across a component tree without manually passing props down through every level. It's a great tool for managing state that's considered "global" to a certain part of the application, such as user authentication, themes, or language settings. It consists of a `Provider` component that wraps a component tree and a `Consumer` or `useContext()` Hook that allows components to access the provided value.

### 11. How do you handle asynchronous operations in React?

Asynchronous operations, like fetching data from an API, are handled using the `useEffect()` Hook. Inside `useEffect()`, you can call an async function. Since `useEffect()`'s cleanup function needs to be synchronous, you'll often define an `async` function inside the effect and then call it immediately. State is used to store the data once it's fetched, and the component re-renders to display it.

JavaScript

```jsx
import React, { useState, useEffect } from 'react';

function MyComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      const response = await fetch('https://api.example.com/data');
      const json = await response.json();
      setData(json);
    }
    fetchData();
  }, []); // Empty dependency array means this effect runs once

  return (
    <div>
      {data ? <p>{data.message}</p> : <p>Loading...</p>}
    </div>
  );
}
```

### 12. What's the difference between `useState` and `useReducer`?

- **`useState`** is for managing simple state changes, like a boolean, string, or number. It's a good choice when the state logic is simple and straightforward.
- **`useReducer`** is an alternative to `useState` for managing more complex state logic. It's especially useful when the state updates depend on the previous state or when you have multiple related state variables. It's based on the **reducer pattern**, where a `reducer` function receives the current state and an `action` and returns a new state.

- `useReducer` hook
    
    The `useReducer` Hook is an alternative to `useState` that's useful for managing more complex state logic. It's based on the **reducer pattern**, a concept from functional programming. This pattern centralizes state updates in a single function, making the logic predictable and easier to test.
    
    ### How `useReducer` Works
    
    The `useReducer` Hook takes two main arguments:
    
    1. **A `reducer` function**: This function receives the current `state` and an `action` object as input. Based on the `action`, the reducer returns a **new state**. It's a "pure" function, meaning it doesn't have side effects and always returns the same output for the same input.
    2. **An initial `state`**: This is the starting value for your state.
    
    `useReducer` returns an array with two elements:
    
    1. The current `state`.
    2. A `dispatch` function.
    
    You use the `dispatch` function to send an `action` object to the `reducer`. This action describes what happened (e.g., `'INCREMENT'`, `'DECREMENT'`). The `reducer` then takes this action and computes the next state.
    
    ### Example: A Counter with `useReducer`
    
    Let's compare a simple counter using both `useState` and `useReducer` to see the difference.
    
    ### `useState` Counter
    
    For a simple counter, `useState` is very straightforward.
    
    ```jsx
    import React, { useState } from 'react';
    
    function CounterWithState() {
      const [count, setCount] = useState(0);
    
      return (
        <div>
          <p>Count: {count}</p>
          <button onClick={() => setCount(count + 1)}>Increment</button>
          <button onClick={() => setCount(count - 1)}>Decrement</button>
        </div>
      );
    }
    ```
    
    ---
    
    ### `useReducer` Counter
    
    When using `useReducer`, you first define the `reducer` function outside the component.
    
    ```jsx
    import React, { useReducer } from 'react';
    
    // The reducer function
    function reducer(state, action) {
      switch (action.type) {
        case 'increment':
          return { count: state.count + 1 };
        case 'decrement':
          return { count: state.count - 1 };
        default:
          throw new Error();
      }
    }
    
    // The component
    function CounterWithReducer() {
      const [state, dispatch] = useReducer(reducer, { count: 0 });
    
      return (
        <div>
          <p>Count: {state.count}</p>
          <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
          <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
        </div>
      );
    }
    ```
    
    ### When to Use `useReducer`
    
    While the counter example might seem more complex with `useReducer`, this approach shines when:
    
    - **State is complex or nested**: When your state is an object with multiple properties, managing updates with a single `reducer` function is much cleaner than using multiple `useState` calls.
    - **State updates depend on the previous state**: The `reducer` always receives the most recent state, making it ideal for updates that rely on the state's previous value.
    - **You need to share state logic**: The `reducer` function can be defined outside the component and shared across different components.
    - **You need to test state logic**: Because the `reducer` is a pure function, you can easily test its logic in isolation without needing to mount the component.
    
    `useReducer` provides a more structured and predictable way to manage state, especially as your application grows in complexity. It's the Hook of choice when `useState` starts to feel messy.
    

---

### 13. What is the difference between controlled and uncontrolled components?

The difference lies in how they manage their form data.

- **Controlled Components**: In a controlled component, the form data is managed by the **component's state**. The state is the "single source of truth." Every time the user types, an `onChange` event handler updates the component's state, which in turn updates the input's value. This gives you instant control over the data and allows you to implement features like real-time validation, formatting, or conditionally disabling a button.
- **Uncontrolled Components**: An uncontrolled component's form data is managed by the **DOM itself**. You use a `ref` to get the value directly from the DOM element when you need it (e.g., when the form is submitted). Uncontrolled components are simpler for very basic forms but offer less control and are not recommended for most use cases in modern React.

---

### 14. Explain the component lifecycle of a class component.

Class components have a lifecycle with methods that are called at different stages of a component's existence. The main phases are:

- **Mounting**: The component is being created and inserted into the DOM. Key methods: `constructor()`, `render()`, `componentDidMount()`. `componentDidMount()` is often used for initial data fetching or setting up subscriptions.
- **Updating**: The component's props or state change, causing it to re-render. Key methods: `shouldComponentUpdate()`, `render()`, `componentDidUpdate()`. `componentDidUpdate()` is used to perform side effects after a re-render.
- **Unmounting**: The component is being removed from the DOM. Key method: `componentWillUnmount()`. This is used for cleanup, such as canceling network requests or removing event listeners.

Note: With the introduction of Hooks, the functional component lifecycle is handled by `useEffect()`, which combines the logic of `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

---

### 15. What are Higher-Order Components (HOCs)?

A Higher-Order Component (HOC) is an advanced technique in React for reusing component logic. An HOC is a **function that takes a component as an argument and returns a new, enhanced component**. HOCs are not part of the React API; they are a pattern that emerged from React's compositional nature.

A classic example is a HOC that provides a component with a subscription to a data source:

JavaScript

`function withData(WrappedComponent) {
  return class extends React.Component {
    // ... logic to fetch data
    render() {
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}`

In modern React, **Hooks are the preferred way to achieve logic reuse**, as they are simpler and don't introduce the "wrapper hell" problem that HOCs sometimes create.

---

### 16. What's the purpose of `useCallback` and `useMemo`?

Both are memoization Hooks that optimize performance by caching values between renders.

- **`useCallback`**: This Hook **memoizes a function**. You wrap a function with `useCallback` to prevent it from being re-created on every render. This is crucial when passing functions as props to child components that rely on `React.memo` to prevent unnecessary re-renders. Without `useCallback`, the child component would always see a "new" function reference, even if its logic is the same, breaking the memoization.
- **`useMemo`**: This Hook **memoizes a value**. You use it to store the result of an expensive calculation. If the dependencies of the `useMemo` call haven't changed, it will return the cached value without re-running the calculation. This is useful for avoiding heavy computations on every re-render.

The key difference is what they memoize: `useCallback` memoizes the function itself, and `useMemo` memoizes the result of a function call.

---

### 17. How do you handle component-level state and application-level state?

- **Component-Level State**: This refers to state that is only relevant to a single component and doesn't need to be shared with others. It's best managed using the `useState` or `useReducer` Hooks within the component itself.
- **Application-Level State (Global State)**: This is state that needs to be shared across many components, potentially across different parts of your application. Managing this with props is a bad idea (prop drilling). Instead, you use:
    - **The Context API**: Good for managing simple global state like themes, user authentication status, or language. It provides a straightforward way for a component to subscribe to a part of the state without having to pass it down manually.
    - **State Management Libraries**: Libraries like **Redux**, **Zustand**, or **Jotai** are designed for complex global state management. They provide a structured, predictable way to update and access shared state, which is crucial for large applications.

---

### 18. What is the difference between a `<div>` and a `<Fragment>`?

Both can be used to group JSX elements, but they serve different purposes.

- **`<div>`**: A `div` is a standard HTML element that gets rendered to the DOM. It adds an extra node to the DOM tree, which can sometimes impact performance or break CSS layouts (e.g., when a parent component expects a direct child of a certain type).
- **`<Fragment>`**: A `Fragment` (or `<>`) is a special component that allows you to group multiple elements without adding an extra node to the DOM. It's rendered to nothing. This is particularly useful when a component must return a single root element but you don't want to wrap it in an unnecessary `div`. It keeps the DOM structure clean and efficient.

---

### 19. When would you use `useRef`?

The `useRef` Hook is used to hold a value that you want to **persist across renders without causing a re-render** when the value changes. It's most commonly used for two purposes:

- **Accessing a DOM Element**: You can create a ref and attach it to a JSX element. The ref's `current` property will then point directly to the DOM node, allowing you to interact with it directly (e.g., focus on an input, measure dimensions).
- **Storing a Mutable Value**: You can use `useRef` to store any value that doesn't need to trigger a re-render when it's updated. This is ideal for things like a timer ID (`setInterval`), a previous state value, or a counter that doesn't affect the UI.

Unlike `useState`, changing a ref's `current` value does not trigger a re-render, which makes it perfect for managing non-UI-related data.