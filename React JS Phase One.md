Let’s dive into **Phase 1: Fundamentals of React** step by step. Each topic includes explanations, examples, and exercises for hands-on practice.

---

### **1. Introduction to React**
**What is React?**
- React is a JavaScript library for building user interfaces.
- It is declarative, component-based, and focuses on a single responsibility: the view layer of your app.
- React apps are built using reusable components.

#### **Getting Started**
1. Install Node.js (required for React apps).
2. Set up a React project:
   ```bash
   npx create-react-app my-first-react-app
   cd my-first-react-app
   npm start
   ```
3. This will create a default project and start a development server.

#### **Exercise**
- Start your first React project and explore the `src` folder.
- Look at `App.js` and modify the `<h1>` text.

---

### **2. JSX and Rendering**
**What is JSX?**
- JSX (JavaScript XML) is a syntax extension for JavaScript. It allows you to write HTML-like code in your JavaScript files.
- Example:
   ```jsx
   const element = <h1>Hello, React!</h1>;
   ```

#### **Rendering Elements**
React elements are rendered to the DOM using `ReactDOM.createRoot`:
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

const element = <h1>Hello, React!</h1>;
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(element);
```

#### **Exercise**
- Modify `App.js` to display:
  ```jsx
  <h1>Welcome to My React App</h1>
  <p>This is my first React application!</p>
  ```

---

### **3. Components and Props**
**What are Components?**
- Components are reusable pieces of UI.  
- Two types: Function Components and Class Components.  
- Example of a Function Component:
  ```jsx
  function Welcome(props) {
      return <h1>Hello, {props.name}!</h1>;
  }

  export default Welcome;
  ```

**Using Props**
- Props allow you to pass data to components.
- Example:
  ```jsx
  function App() {
      return (
          <div>
              <Welcome name="Alice" />
              <Welcome name="Bob" />
          </div>
      );
  }
  ```

#### **Exercise**
- Create a `Greeting` component that takes a `name` prop and renders `Hello, [name]!`.

---

### **4. State and Event Handling**
**What is State?**
- State is data managed within a component that can change over time.
- Example using `useState`:
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

  export default Counter;
  ```

**Handling Events**
- React supports standard DOM events.
- Example:
  ```jsx
  function App() {
      const handleClick = () => alert('Button clicked!');

      return <button onClick={handleClick}>Click Me</button>;
  }
  ```

#### **Exercise**
- Create a button that toggles between "ON" and "OFF" states when clicked.

---

### **5. Lists and Keys**
**Rendering Lists**
- Use the `map()` method to render lists.
- Example:
  ```jsx
  function TodoList() {
      const tasks = ['Task 1', 'Task 2', 'Task 3'];

      return (
          <ul>
              {tasks.map((task, index) => (
                  <li key={index}>{task}</li>
              ))}
          </ul>
      );
  }
  ```

**Why Keys?**
- Keys help React identify which items have changed, added, or removed.
- Use a unique key for each item in a list.

#### **Exercise**
- Create a `ShoppingList` component that displays a list of items passed as props.

---

### **6. Conditional Rendering**
**Techniques for Conditional Rendering**
1. **Using Ternary Operator**:
   ```jsx
   function Greeting({ isLoggedIn }) {
       return <h1>{isLoggedIn ? 'Welcome back!' : 'Please sign in.'}</h1>;
   }
   ```

2. **Using `&&`**:
   ```jsx
   function App({ showMessage }) {
       return <>{showMessage && <p>This is a conditional message.</p>}</>;
   }
   ```

#### **Exercise**
- Create a component that displays "Logged In" or "Guest" based on a prop.

---

### **7. Basic Forms**
**Controlled Components**
- React forms manage the state of form inputs using `useState`.
- Example:
  ```jsx
  function LoginForm() {
      const [username, setUsername] = useState('');

      const handleSubmit = (e) => {
          e.preventDefault();
          alert(`Welcome, ${username}!`);
      };

      return (
          <form onSubmit={handleSubmit}>
              <input
                  type="text"
                  value={username}
                  onChange={(e) => setUsername(e.target.value)}
                  placeholder="Enter username"
              />
              <button type="submit">Login</button>
          </form>
      );
  }
  ```

#### **Exercise**
- Create a form with "Name" and "Email" fields. Display the entered data when the form is submitted.

---

### **Project for Phase 1**
#### **Build a Simple React App**
**Requirements**:
1. Create a greeting component that changes based on the time of day.
2. Add a form to take user input and display it.
3. Include a list of tasks with a "mark as done" button for each.
