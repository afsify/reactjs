# `useContext` Hook in React

The `useContext` hook is a React hook that allows functional components to access values from a **context** without having to pass them through every level of the component tree via props. This is a powerful way to manage global state or data that is shared across many components in an application, such as user authentication, theme settings, or language preferences.

#### **Why Use `useContext`?**
`useContext` is commonly used to:
- Avoid **prop drilling**: Prop drilling refers to the process of passing data down through multiple layers of components. `useContext` provides a cleaner way to access shared data without having to pass it explicitly through each level of the component tree.
- Share **global state**: Context is often used for managing global state such as user settings, authentication status, theme preferences, etc., which are needed by multiple components.

#### **How `useContext` Works**
1. **Create a Context**: First, you need to create a context using `React.createContext()` or `useContext` in combination with `createContext` in the parent component.
2. **Provide the Context**: Wrap your component tree (or part of it) with the `Context.Provider` and pass the value that you want to share as a prop to `Provider`.
3. **Consume the Context**: Use `useContext` inside any child component to access the context value.

### **Steps to Use `useContext`**

#### **1. Creating a Context**

Use `React.createContext()` to create a context. This function returns an object with two components:
- **Provider**: A component that allows you to provide a value to the context.
- **Consumer**: A component (or hook in modern React) that allows you to access the value from the context.

```jsx
// Creating a context
const ThemeContext = React.createContext();
```

#### **2. Providing Context Value**

Wrap your component tree with the `Context.Provider` and pass the shared value as a `value` prop to the `Provider`. The components wrapped by `Provider` can now consume this value.

```jsx
function App() {
  const theme = "dark";

  return (
    <ThemeContext.Provider value={theme}>
      <ComponentA />
    </ThemeContext.Provider>
  );
}
```

#### **3. Consuming Context Value with `useContext`**

To access the value provided by the context, you can use the `useContext` hook inside any child component of the context provider.

```jsx
import { useContext } from 'react';

function ComponentA() {
  const theme = useContext(ThemeContext);
  
  return <p>The current theme is: {theme}</p>;
}
```

In this example:
- `ComponentA` can access the `theme` value directly using `useContext(ThemeContext)`.
- This eliminates the need for `props` drilling or passing the `theme` through intermediate components.

#### **4. Updating Context Value (Optional)**

If the value in the context needs to be updated, you can make the value dynamic. Typically, you will have the state in the **Provider** and update the state to pass new values to consumers.

```jsx
function App() {
  const [theme, setTheme] = useState("dark");

  const toggleTheme = () => {
    setTheme(theme === "dark" ? "light" : "dark");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <ComponentA />
    </ThemeContext.Provider>
  );
}

function ComponentA() {
  const { theme, toggleTheme } = useContext(ThemeContext);
  
  return (
    <div>
      <p>The current theme is: {theme}</p>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}
```

In this case, `ComponentA` not only consumes the context value but can also update the `theme` value using the `toggleTheme` function passed through the context.

### **Key Points of `useContext`**

1. **Global State Management**: `useContext` is often used to access and manage global state or shared data across many components (e.g., user authentication, theming, language preferences).
   
2. **Eliminating Prop Drilling**: It allows you to avoid prop drilling, where props are passed down through many layers of components, making the code cleaner and easier to maintain.
   
3. **Dynamic Context**: The context value can be dynamic and managed using `useState` or other state management solutions. This allows consumers to react to changes in the context.

4. **Performance Considerations**: `useContext` re-renders the consuming component whenever the context value changes. Therefore, be mindful of unnecessary re-renders that may affect performance, especially if the context is used in many places or contains large objects.

5. **Using Context with Multiple Providers**: You can nest multiple context providers to provide different values in different parts of the component tree.

   ```jsx
   <UserContext.Provider value={user}>
     <ThemeContext.Provider value={theme}>
       <Component />
     </ThemeContext.Provider>
   </UserContext.Provider>
   ```

6. **Custom Hooks**: You can combine `useContext` with your own custom hooks to simplify context consumption, especially if you're working with complex context structures or need to access multiple contexts.

### **Example: Sharing User Authentication Status**

```jsx
import React, { useState, useContext, createContext } from "react";

// Create the Context
const AuthContext = createContext();

function App() {
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  const login = () => setIsAuthenticated(true);
  const logout = () => setIsAuthenticated(false);

  return (
    <AuthContext.Provider value={{ isAuthenticated, login, logout }}>
      <Header />
      <Main />
    </AuthContext.Provider>
  );
}

function Header() {
  const { isAuthenticated, logout } = useContext(AuthContext);

  return (
    <header>
      {isAuthenticated ? (
        <button onClick={logout}>Logout</button>
      ) : (
        <button>Login</button>
      )}
    </header>
  );
}

function Main() {
  const { isAuthenticated, login } = useContext(AuthContext);

  return (
    <main>
      <h1>{isAuthenticated ? "Welcome!" : "Please log in"}</h1>
      {!isAuthenticated && <button onClick={login}>Log In</button>}
    </main>
  );
}

export default App;
```

In this example:
- The `AuthContext` is created to share the authentication status across the components.
- The context provides the `isAuthenticated` state and `login`/`logout` functions to control the state.
- Both `Header` and `Main` components consume this context via `useContext` to render different views based on the authentication status.

### **Summary of `useContext`**

- **Purpose**: The `useContext` hook provides a simple way to access the context value in functional components, enabling shared state across many components without prop drilling.
- **Usage**:
  1. Create context using `createContext`.
  2. Wrap components with `Context.Provider` to provide context value.
  3. Use `useContext` in child components to consume the context.
- **Best Practices**: It’s ideal for managing global state (e.g., themes, user authentication), but use it wisely, as it may lead to unnecessary re-renders when context values change frequently.
