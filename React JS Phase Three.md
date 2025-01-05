**Phase 3: Advanced Concepts**, where we focus on performance optimization, custom hooks, and the basics of Redux. Each topic includes detailed explanations, examples, and exercises for hands-on learning.

---

### **1. Performance Optimization**
Optimizing React apps ensures they are fast and responsive, especially when handling large amounts of data or complex UIs.

---

#### **1.1 Memoization with `React.memo`**
**Purpose**:
- Prevent unnecessary re-renders of components.

**Example**:
```jsx
import React, { useState } from 'react';

const ChildComponent = React.memo(({ value }) => {
    console.log('Child component rendered');
    return <h1>Value: {value}</h1>;
});

function App() {
    const [count, setCount] = useState(0);
    const [otherValue, setOtherValue] = useState(0);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>Increment Count</button>
            <button onClick={() => setOtherValue(otherValue + 1)}>Increment Other</button>
            <ChildComponent value={count} />
        </div>
    );
}

export default App;
```

**How It Works**:
- `React.memo` ensures the `ChildComponent` only re-renders when its `value` prop changes.

#### **Exercise**:
- Create a parent-child component setup where only specific children re-render when a button is clicked.

---

#### **1.2 `useCallback` for Memoizing Functions**
**Purpose**:
- Prevent recreation of functions during renders.

**Example**:
```jsx
import React, { useState, useCallback } from 'react';

function Button({ onClick, label }) {
    console.log(`Rendering button: ${label}`);
    return <button onClick={onClick}>{label}</button>;
}

const MemoizedButton = React.memo(Button);

function App() {
    const [count, setCount] = useState(0);

    const increment = useCallback(() => {
        setCount((prevCount) => prevCount + 1);
    }, []);

    return (
        <div>
            <h1>Count: {count}</h1>
            <MemoizedButton onClick={increment} label="Increment" />
        </div>
    );
}

export default App;
```

#### **Exercise**:
- Modify the above example to include multiple buttons, each triggering different state changes, and memoize their functions.

---

#### **1.3 `useMemo` for Expensive Calculations**
**Purpose**:
- Cache expensive computations to avoid recalculating unnecessarily.

**Example**:
```jsx
import React, { useState, useMemo } from 'react';

function App() {
    const [count, setCount] = useState(0);
    const [otherValue, setOtherValue] = useState(0);

    const expensiveCalculation = useMemo(() => {
        console.log('Calculating...');
        return count * 10;
    }, [count]);

    return (
        <div>
            <h1>Count: {count}</h1>
            <h2>Other Value: {otherValue}</h2>
            <h3>Expensive Calculation: {expensiveCalculation}</h3>
            <button onClick={() => setCount(count + 1)}>Increment Count</button>
            <button onClick={() => setOtherValue(otherValue + 1)}>Increment Other</button>
        </div>
    );
}

export default App;
```

#### **Exercise**:
- Create a React app that calculates the factorial of a number using `useMemo`.

---

### **2. Custom Hooks**
Custom hooks allow you to extract reusable logic from components.

---

#### **2.1 Creating a Custom Hook**
**Example**:
```jsx
import React, { useState } from 'react';

function useCounter(initialValue = 0) {
    const [count, setCount] = useState(initialValue);

    const increment = () => setCount(count + 1);
    const decrement = () => setCount(count - 1);

    return { count, increment, decrement };
}

function Counter() {
    const { count, increment, decrement } = useCounter(10);

    return (
        <div>
            <h1>Count: {count}</h1>
            <button onClick={increment}>Increment</button>
            <button onClick={decrement}>Decrement</button>
        </div>
    );
}

export default Counter;
```

#### **Exercise**:
- Create a custom hook `useToggle` to toggle between `true` and `false`.

---

### **3. State Management with Redux**
Redux helps manage global state in large applications.

---

#### **3.1 Core Concepts of Redux**
1. **Store**: Centralized state container.
2. **Actions**: Plain objects describing changes.
3. **Reducers**: Pure functions that update the state.

---

#### **3.2 Setting Up Redux**
**Installation**:
```bash
npm install redux react-redux
```

**Basic Example**:
```jsx
// store.js
import { createStore } from 'redux';

const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
    switch (action.type) {
        case 'INCREMENT':
            return { count: state.count + 1 };
        case 'DECREMENT':
            return { count: state.count - 1 };
        default:
            return state;
    }
}

const store = createStore(counterReducer);
export default store;
```

```jsx
// App.js
import React from 'react';
import { Provider, useDispatch, useSelector } from 'react-redux';
import store from './store';

function Counter() {
    const count = useSelector((state) => state.count);
    const dispatch = useDispatch();

    return (
        <div>
            <h1>Count: {count}</h1>
            <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
            <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
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

#### **Exercise**:
- Add an input field to the above example that sets a custom increment value.

---

### **Project for Phase 3**
#### **Build a Redux-Powered Counter App**
**Requirements**:
1. Implement a counter with Increment, Decrement, and Reset buttons.
2. Add a toggle switch to show/hide the counter (use `useMemo` for optimization).
3. Use Redux to manage the counter state.

