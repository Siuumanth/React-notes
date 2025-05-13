## 🔄 What is **Render** in React?

**Rendering** in React means:

> **Converting your component’s JSX into actual DOM elements** on the web page.

Whenever a component:
- is shown for the first time, or
- its **state/props** change,

React calls the component function again to calculate what the UI should look like, and then **re-renders** (updates) the actual DOM if anything changed.

> React uses a **virtual DOM** to compare changes and update the **real DOM efficiently**.
**Example**: Clicking a button that updates state will cause a re-render.

---

## 🟢 What is **Mounting**?

Mounting means:

> The **first time** a React component is **created and inserted** into the DOM.

This includes:
- Running the component’s function
- Setting up internal state
- Attaching it to the DOM

### In `useEffect`:


`useEffect(() => {   console.log("Mounted"); }, []);`

This runs **only once**, after the component is mounted (because of the empty dependency array).

---

## 🔴 What is **Unmounting**?

Unmounting means:

> The component is **removed** from the DOM.

React will:

- Delete the DOM elements for that component
- Clean up any resources like intervals, subscriptions, or event listeners

### In `useEffect`:

`useEffect(() => {   return () => {     console.log("Unmounted");   }; }, []);`

The function inside `return` is the **cleanup function** — it runs **just before** the component is removed.

---

## ✅ Summary Table

| Term    | Meaning                                         | Triggered When?                                             |
| ------- | ----------------------------------------------- | ----------------------------------------------------------- |
| Render  | JSX is converted to DOM and updates are applied | On first mount and every state/props update                 |
| Mount   | Component is created and added to DOM           | When component first appears                                |
| Unmount | Component is removed from DOM                   | \| When you hide, navigate away, or conditionally remove it |
|         |                                                 |                                                             |

## React Hooks - Detailed Notes

### What Are Hooks?

Hooks are functions that let you "hook into" React state and lifecycle features from function components. Introduced in React 16.8, they allow functional components to handle features like state, side effects, context, and more—without converting them into class components.

Hooks promote **cleaner code**, **logic reuse**, and better **separation of concerns**.

---

### Rules of Hooks

1. **Only call hooks at the top level** of your function. Never use hooks inside loops, conditions, or nested functions.
    
2. **Only call hooks from React function components or custom hooks.** Not from regular JavaScript functions.
    

---

### Commonly Used Hooks

#### 1. `useState`

**Purpose**: To add local state to a functional component.

```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare a state variable called 'count', initialized to 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      {/* Increment the count on button click */}
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Explanation**: `useState(0)` initializes the `count` state with a value of 0. `setCount` is the function used to update this state. On button click, the state updates and the component re-renders with the new value.

---

## 2. `useEffect`

### 🌀 React `useEffect` Hook – Canvas Layout


## ✅ What is `useEffect`?

> `useEffect` is a built-in **React Hook** that lets you perform **side effects** in function components.

### ⚡ What are Side Effects?

- Fetching API data
- Setting up timers/intervals
- Subscribing to events or data streams
- Manually modifying the DOM
- Adding/removing event listeners

### 📜 In Class Components (Before Hooks)

- `componentDidMount`
- `componentDidUpdate`
- `componentWillUnmount`

Now all handled in **one unified API**: `useEffect`.
---

## 🧠 Syntax

```jsx
useEffect(() => {
  // Side-effect logic (e.g., API calls, setting up timers)

  return () => {
    // Cleanup logic (e.g., clearInterval, remove event listeners) () optional 
  };
}, [dependencies]);


```

---

## 📌 Variants of `useEffect`

### 1️⃣ No Dependency Array

```jsx
useEffect(() => {
  console.log("Runs every render");
});

```

- Runs **on every render**
- Cleanup runs **before next effect**
- ⚠️ Can lead to performance issues

---

### 2️⃣ Empty Dependency Array `[]`

```jsx
useEffect(() => {
  console.log("Effect ran once after initial render");

  return () => {
    console.log("Cleanup on unmount");
  };
}, []);

```

- Runs **only once**, after first render
- Cleanup runs **only once**, on unmount
- Equivalent to `componentDidMount` + `componentWillUnmount`

---

### 3️⃣ With Specific Dependencies

```jsx
useEffect(() => {
  console.log("Effect ran because 'count' changed");

  return () => {
    console.log("Cleanup before effect re-runs or on unmount");
  };
}, [count]);

```


- Runs on **first render** and when any dependency changes
- Cleanup runs **before the effect re-runs** or on **unmount**
    

---

## 🧹 Why Cleanup Is Important

To prevent:

- 🧠 **Memory leaks**
- 🧪 **Unexpected side-effects**
- 🕸️ **Timers/event handlers stacking up**

### ✅ Good Cleanup Example:

```jsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log("Tick");
  }, 1000);

  return () => {
    clearInterval(interval); // ✅ Prevents memory leaks
  };
}, []);
```

### 🔍 What Happens Without Dependencies?

- The effect runs after **every render**, not just reload.
- Cleanup runs **before each new effect**, preventing stacking side effects.

---

#### 4. `useRef`

**Purpose**: To persist values across renders or directly reference DOM elements.

```jsx
import React, { useRef } from 'react';

function InputFocus() {
  // Create a ref object for the input element
  const inputRef = useRef(null);

  const focusInput = () => {
    // Access the input DOM node and focus it
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

**Explanation**: `useRef` holds a mutable reference to the input element. When the button is clicked, the input field receives focus via `inputRef.current.focus()`.

---

#### 5. `useMemo`

**Purpose**: To memoize expensive computations and avoid unnecessary recalculations.

```jsx
import React, { useMemo, useState } from 'react';

function ExpensiveCalculation({ num }) {
  const [count, setCount] = useState(0);

  // Memoize the square of 'num', only recalculating when 'num' changes
  const squared = useMemo(() => {
    console.log("Calculating square...");
    return num * num;
  }, [num]);

  return (
    <div>
      <p>Squared: {squared}</p>
      {/* Button to trigger re-render without changing 'num' */}
      <button onClick={() => setCount(count + 1)}>Re-render</button>
    </div>
  );
}
```

**Explanation**: The square of `num` is recalculated only when `num` changes. `useMemo` helps optimize performance by skipping unnecessary recalculations during renders.

---

#### 6. `useCallback`

**Purpose**: To memoize functions so that they don't get recreated on every render.

```jsx
import React, { useState, useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  // Memoize the increment function so its reference doesn't change on every render
  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return <Child onClick={increment} />;
}

function Child({ onClick }) {
  return <button onClick={onClick}>Increment from Child</button>;
}
```

**Explanation**: `increment` remains the same function between renders due to `useCallback`. This is useful for performance, especially when passing functions to memoized or optimized child components.

---

#### 7. `useReducer`

**Purpose**: To manage complex state logic similar to Redux reducers.

```jsx
import React, { useReducer } from 'react';

// Reducer function to handle different action types
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  // Initialize state and dispatcher
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      {/* Dispatch actions to modify state */}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

**Explanation**: `useReducer` is a more structured way to manage complex state transitions. Actions are dispatched, and the reducer function handles how the state updates based on action type.

---

### Summary Table

|Hook|Purpose|
|---|---|
|`useState`|Add local state|
|`useEffect`|Perform side effects|
|`useContext`|Consume context|
|`useRef`|Mutable ref object / DOM access|
|`useMemo`|Memoize expensive computations|
|`useCallback`|Memoize functions|
|`useReducer`|Complex state management (like Redux)|

---

### Custom Hooks

You can combine the above hooks to create **custom hooks** for encapsulating and reusing logic across multiple components.

```jsx
import { useState } from 'react';

// A custom hook to manage a boolean toggle state
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = () => setValue(v => !v);

  return [value, toggle];
}

function ToggleComponent() {
  // Use the custom hook to control visibility
  const [isVisible, toggleVisibility] = useToggle();

  return (
    <div>
      <button onClick={toggleVisibility}>Toggle</button>
      {/* Show content conditionally based on 'isVisible' */}
      {isVisible && <p>Now you see me!</p>}
    </div>
  );
}
```

**Explanation**: `useToggle` is a reusable custom hook that provides a boolean value and a toggle function. It encapsulates toggle logic for reuse across multiple components in a clean and efficient way.