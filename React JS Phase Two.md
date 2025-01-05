
**Phase 2: Intermediate Concepts**. This phase focuses on hooks, routing, and state management with the Context API. Each topic includes detailed explanations, examples, and exercises.

---

### **1. React Hooks**
Hooks allow you to use React features like state and lifecycle methods in functional components.

---

#### **1.1 `useEffect`: Managing Side Effects**
**Purpose**:
- Perform side effects like fetching data, subscriptions, or DOM updates.

**Example**:
```jsx
import React, { useState, useEffect } from 'react';

function Timer() {
    const [count, setCount] = useState(0);

    useEffect(() => {
        const timer = setInterval(() => {
            setCount((prevCount) => prevCount + 1);
        }, 1000);

        // Cleanup function
        return () => clearInterval(timer);
    }, []); // Empty dependency array ensures the effect runs only once.

    return <h1>Timer: {count}s</h1>;
}

export default Timer;
```

#### **Exercise**:
- Create a `DataFetcher` component that fetches data from an API and displays it.
  - Use `useEffect` to fetch the data when the component mounts.
  - Example API: [https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts).

---

#### **1.2 `useRef`: Accessing DOM Elements**
**Purpose**:
- Access DOM nodes or persist mutable values without re-rendering the component.

**Example**:
```jsx
import React, { useRef } from 'react';

function InputFocus() {
    const inputRef = useRef(null);

    const handleFocus = () => {
        inputRef.current.focus();
    };

    return (
        <div>
            <input ref={inputRef} type="text" placeholder="Focus me!" />
            <button onClick={handleFocus}>Focus Input</button>
        </div>
    );
}

export default InputFocus;
```

#### **Exercise**:
- Create a `Stopwatch` component using `useRef` to track elapsed time.

---

### **2. React Router**
React Router allows navigation between pages in a React application.

---

#### **2.1 Setting Up Routing**
**Installation**:
```bash
npm install react-router-dom
```

**Basic Setup**:
```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Routes, Link } from 'react-router-dom';

function Home() {
    return <h1>Home Page</h1>;
}

function About() {
    return <h1>About Page</h1>;
}

function App() {
    return (
        <Router>
            <nav>
                <Link to="/">Home</Link> | <Link to="/about">About</Link>
            </nav>
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
            </Routes>
        </Router>
    );
}

export default App;
```

#### **Exercise**:
- Add a "Contact" page to the above example and navigate to it using a link.

---

#### **2.2 Route Parameters and Query Strings**
**Example**:
```jsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, useParams } from 'react-router-dom';

function UserProfile() {
    const { userId } = useParams();
    return <h1>User Profile for ID: {userId}</h1>;
}

function App() {
    return (
        <Router>
            <Routes>
                <Route path="/user/:userId" element={<UserProfile />} />
            </Routes>
        </Router>
    );
}

export default App;
```

#### **Exercise**:
- Create a blog app with a dynamic route `/post/:postId` that displays post details based on the `postId`.

---

### **3. Context API**
The Context API allows you to manage and share global state without prop drilling.

---

#### **3.1 Creating a Context**
**Steps**:
1. Create the context.
2. Use the `Provider` to wrap components.
3. Consume the context using `useContext`.

**Example**:
```jsx
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

function ThemeProvider({ children }) {
    const [theme, setTheme] = useState('light');

    const toggleTheme = () => {
        setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
    };

    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            {children}
        </ThemeContext.Provider>
    );
}

function ThemedComponent() {
    const { theme, toggleTheme } = useContext(ThemeContext);
    return (
        <div style={{ background: theme === 'light' ? '#fff' : '#333', color: theme === 'light' ? '#000' : '#fff' }}>
            <h1>Current Theme: {theme}</h1>
            <button onClick={toggleTheme}>Toggle Theme</button>
        </div>
    );
}

function App() {
    return (
        <ThemeProvider>
            <ThemedComponent />
        </ThemeProvider>
    );
}

export default App;
```

#### **Exercise**:
- Create a `LanguageProvider` to switch between English and Spanish texts.

---

### **Project for Phase 2**
#### **Build a Multi-Page App**
**Requirements**:
1. Use React Router to create pages: Home, About, and Contact.
2. Add a Context API for theme switching.
3. Fetch and display data from an API on one of the pages.
