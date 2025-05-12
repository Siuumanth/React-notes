# React Hooks - Detailed Notes

## What Are Hooks?

Hooks are functions that let you "hook into" React state and lifecycle features from function components. They were introduced in React 16.8 to allow developers to use state and other React features without writing a class component.

Hooks work only in functional components and provide a cleaner, simpler way to reuse stateful logic.

---

## Rules of Hooks

1. **Only call hooks at the top level** of your function. Don't call them inside loops, conditions, or nested functions.
    
2. **Only call hooks from React function components or custom hooks.**

---

## Commonly Used Hooks

### 1. `useState`

**Purpose**: To add local state to a functional component.

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### 2. `useEffect`

**Purpose**: To perform side effects (e.g., fetch data, update DOM, set timers) in a function component.

```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(s => s + 1);
    }, 1000);

    return () => clearInterval(interval); // cleanup
  }, []); // empty dependency array -> runs only once

  return <p>Timer: {seconds}s</p>;
}
```

---

### 3. `useContext`

**Purpose**: To access values from React Context without using `<Context.Consumer>`.

```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return <div>Current theme: {theme}</div>;
}
```

---

### 4. `useRef`

**Purpose**: To access DOM elements directly or persist mutable values without causing re-renders.

```jsx
import React, { useRef } from 'react';

function InputFocus() {
  const inputRef = useRef(null);

  const focusInput = () => {
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

---

### 5. `useMemo`

**Purpose**: To memoize expensive computations and avoid recalculations on every render.

```jsx
import React, { useMemo, useState } from 'react';

function ExpensiveCalculation({ num }) {
  const [count, setCount] = useState(0);

  const squared = useMemo(() => {
    console.log("Calculating square...");
    return num * num;
  }, [num]);

  return (
    <div>
      <p>Squared: {squared}</p>
      <button onClick={() => setCount(count + 1)}>Re-render</button>
    </div>
  );
}
```

---

### 6. `useCallback`

**Purpose**: To memoize functions so that they are not recreated on every render.

```jsx
import React, { useState, useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return <Child onClick={increment} />;
}

function Child({ onClick }) {
  return <button onClick={onClick}>Increment from Child</button>;
}
```

---

### 7. `useReducer`

**Purpose**: To manage complex state logic (like in Redux) within a component.

```jsx
import React, { useReducer } from 'react';

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
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

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

You can also create **custom hooks** by combining these hooks for reusable logic across components.