**Phase 4: Mastering Redux Toolkit**! Redux Toolkit (RTK) simplifies Redux development by reducing boilerplate and providing powerful utilities. In this phase, we’ll focus on `createSlice`, `configureStore`, `createAsyncThunk`, and advanced Redux Toolkit features.

---

## **1. Introduction to Redux Toolkit**
RTK is the official, recommended way to write Redux logic. It provides:
1. `createSlice`: Simplifies reducers and actions.
2. `configureStore`: Sets up the store with sensible defaults.
3. `createAsyncThunk`: Handles asynchronous logic like API calls.
4. Built-in DevTools and middleware support.

**Installation**:
```bash
npm install @reduxjs/toolkit react-redux
```

---

### **2. Setting Up Redux Toolkit**
Here’s how to set up a Redux Toolkit project.

---

#### **2.1 `createSlice`**
`createSlice` combines reducers, actions, and initial state in one place.

**Example**:
```jsx
// features/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
    name: 'counter',
    initialState: { value: 0 },
    reducers: {
        increment: (state) => {
            state.value += 1;
        },
        decrement: (state) => {
            state.value -= 1;
        },
        incrementByAmount: (state, action) => {
            state.value += action.payload;
        },
    },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

**Setting Up the Store**:
```jsx
// app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counterSlice';

const store = configureStore({
    reducer: {
        counter: counterReducer,
    },
});

export default store;
```

**Connecting React to Redux**:
```jsx
// App.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from './features/counterSlice';
import { Provider } from 'react-redux';
import store from './app/store';

function Counter() {
    const count = useSelector((state) => state.counter.value);
    const dispatch = useDispatch();

    return (
        <div>
            <h1>Count: {count}</h1>
            <button onClick={() => dispatch(increment())}>Increment</button>
            <button onClick={() => dispatch(decrement())}>Decrement</button>
            <button onClick={() => dispatch(incrementByAmount(5))}>Increment by 5</button>
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

---

#### **2.2 Exercise**:
1. Create a slice for managing a TODO list:
   - Actions: `addTodo`, `removeTodo`, `toggleComplete`.
   - State: An array of objects (`[{ id, text, completed }]`).
2. Connect it to a React app and implement basic CRUD functionality.

---

### **3. Asynchronous Logic with `createAsyncThunk`**
`createAsyncThunk` simplifies API call management in Redux.

---

#### **3.1 Fetching Data**
**Example**:
```jsx
// features/postsSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchPosts = createAsyncThunk('posts/fetchPosts', async () => {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    return await response.json();
});

const postsSlice = createSlice({
    name: 'posts',
    initialState: { posts: [], loading: false, error: null },
    extraReducers: (builder) => {
        builder
            .addCase(fetchPosts.pending, (state) => {
                state.loading = true;
                state.error = null;
            })
            .addCase(fetchPosts.fulfilled, (state, action) => {
                state.loading = false;
                state.posts = action.payload;
            })
            .addCase(fetchPosts.rejected, (state, action) => {
                state.loading = false;
                state.error = action.error.message;
            });
    },
});

export default postsSlice.reducer;
```

**Using the Thunk**:
```jsx
// App.js
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchPosts } from './features/postsSlice';
import store from './app/store';
import { Provider } from 'react-redux';

function Posts() {
    const { posts, loading, error } = useSelector((state) => state.posts);
    const dispatch = useDispatch();

    useEffect(() => {
        dispatch(fetchPosts());
    }, [dispatch]);

    if (loading) return <p>Loading...</p>;
    if (error) return <p>Error: {error}</p>;

    return (
        <ul>
            {posts.map((post) => (
                <li key={post.id}>{post.title}</li>
            ))}
        </ul>
    );
}

function App() {
    return (
        <Provider store={store}>
            <Posts />
        </Provider>
    );
}

export default App;
```

---

#### **3.2 Exercise**:
- Create a user management system:
  - Fetch users from [https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users).
  - Handle loading, success, and error states.
  - Display user details in a list.

---

### **4. Advanced Redux Toolkit Features**
---

#### **4.1 Using Middleware**
Middleware lets you extend Redux functionality, like logging actions or handling side effects.

**Example: Adding a Logger Middleware**:
```jsx
import { configureStore, getDefaultMiddleware } from '@reduxjs/toolkit';

const loggerMiddleware = (store) => (next) => (action) => {
    console.log('Dispatching:', action);
    const result = next(action);
    console.log('Next state:', store.getState());
    return result;
};

const store = configureStore({
    reducer: { /* your reducers */ },
    middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(loggerMiddleware),
});
```

---

#### **4.2 Normalizing Data with RTK Query**
RTK Query is a powerful data-fetching and caching tool.

**Setup**:
```bash
npm install @reduxjs/toolkit
```

**Example**:
```jsx
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const api = createApi({
    reducerPath: 'api',
    baseQuery: fetchBaseQuery({ baseUrl: 'https://jsonplaceholder.typicode.com/' }),
    endpoints: (builder) => ({
        getPosts: builder.query({
            query: () => 'posts',
        }),
    }),
});

export const { useGetPostsQuery } = api;
```

**Using RTK Query**:
```jsx
function Posts() {
    const { data: posts, error, isLoading } = useGetPostsQuery();

    if (isLoading) return <p>Loading...</p>;
    if (error) return <p>Error: {error.message}</p>;

    return (
        <ul>
            {posts.map((post) => (
                <li key={post.id}>{post.title}</li>
            ))}
        </ul>
    );
}
```

---

### **Project for Phase 4**
#### **Build an Advanced Blog App**
**Requirements**:
1. Use Redux Toolkit for managing global state (posts, users).
2. Implement `createAsyncThunk` to fetch posts and user data.
3. Use RTK Query to fetch individual post details.
4. Add a theme switcher using the Context API for practice.
