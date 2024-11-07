# **Custom Hooks in React**

**Custom Hooks** are a way to reuse stateful logic between components in React. They allow you to extract component logic into reusable functions without changing the component hierarchy. Custom hooks follow the same rules as regular hooks, such as starting with the `use` prefix and adhering to the rules of hooks (e.g., they must be called at the top level of a component or other hooks).

By using custom hooks, you can keep components clean and encapsulate logic that can be shared across different parts of your app.

### **Why Use Custom Hooks?**

1. **Reusability**: Custom hooks allow you to extract common logic into reusable functions, making it easier to maintain and test.
2. **Code Simplification**: Complex logic in a component can be extracted into a custom hook, making the component simpler and more readable.
3. **Separation of Concerns**: Custom hooks allow you to separate logic from the UI, which results in cleaner, more modular code.
4. **Avoids Redundancy**: When multiple components require the same logic, custom hooks eliminate redundancy by centralizing the logic into a single hook.
5. **Improved Maintainability**: As your application grows, using custom hooks helps manage state and side effects more effectively, leading to better-maintained code.

### **Creating a Custom Hook**

A custom hook is simply a JavaScript function that can use React hooks (e.g., `useState`, `useEffect`, etc.) internally and return values or functions that can be used in components. Here's the basic structure:

```javascript
import { useState, useEffect } from 'react';

function useCustomHook() {
  const [value, setValue] = useState(initialValue);

  useEffect(() => {
    // Effect logic here
  }, [dependencies]);

  return [value, setValue]; // Return the necessary state or functions
}
```

### **Example 1: Custom Hook for Fetching Data**

A common use case for a custom hook is fetching data from an API. Below is an example of a custom hook that fetches data and handles loading and error states:

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        const result = await response.json();
        setData(result);
      } catch (error) {
        setError(error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

Now, you can use this `useFetch` hook in any component:

```javascript
import React from 'react';
import useFetch from './useFetch';

function DataDisplay({ url }) {
  const { data, loading, error } = useFetch(url);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return <div>{JSON.stringify(data)}</div>;
}
```

### **Example 2: Custom Hook for Window Size**

Another common use case is to create a custom hook that tracks the size of the browser window:

```javascript
import { useState, useEffect } from 'react';

function useWindowSize() {
  const [size, setSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });

  useEffect(() => {
    const handleResize = () => {
      setSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return size;
}

export default useWindowSize;
```

Now, you can use the `useWindowSize` hook in any component:

```javascript
import React from 'react';
import useWindowSize from './useWindowSize';

function DisplayWindowSize() {
  const { width, height } = useWindowSize();

  return (
    <div>
      Window size: {width} x {height}
    </div>
  );
}
```

### **Example 3: Custom Hook for Form Handling**

A custom hook can also be used for managing form states and handling form submissions.

```javascript
import { useState } from 'react';

function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);

  const handleChange = (event) => {
    const { name, value } = event.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  };

  const handleSubmit = (event, submitCallback) => {
    event.preventDefault();
    submitCallback(values);
  };

  return {
    values,
    handleChange,
    handleSubmit,
  };
}

export default useForm;
```

You can then use the `useForm` hook inside a form component:

```javascript
import React from 'react';
import useForm from './useForm';

function SignUpForm() {
  const { values, handleChange, handleSubmit } = useForm({
    username: '',
    email: '',
  });

  const handleSignUp = (formData) => {
    console.log('Form submitted with data:', formData);
  };

  return (
    <form onSubmit={(e) => handleSubmit(e, handleSignUp)}>
      <input
        type="text"
        name="username"
        value={values.username}
        onChange={handleChange}
        placeholder="Username"
      />
      <input
        type="email"
        name="email"
        value={values.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <button type="submit">Sign Up</button>
    </form>
  );
}
```

### **Best Practices for Custom Hooks**

1. **Prefix with `use`**: Always start the name of your custom hook with `use` to follow React's naming conventions and ensure proper linting.
   - Example: `useWindowSize`, `useFetch`, `useForm`

2. **Keep Hooks Pure**: Custom hooks should be pure, meaning they don’t modify anything outside their scope and are predictable.

3. **Avoid Side Effects in Custom Hooks**: Only manage state and side effects inside hooks if necessary. For example, avoid direct DOM manipulation inside custom hooks (this is the job of `useEffect`).

4. **Use Context for Global State**: If your custom hook manages shared state (e.g., user authentication state), consider using React’s context API alongside custom hooks for more complex state management.

5. **Return Only What’s Necessary**: When creating custom hooks, only return the values and functions that a component needs. This keeps the hook API clean and concise.

### **Summary of Key Points**

- **Custom hooks** allow for extracting and reusing stateful logic across multiple components.
- They follow the same rules as regular hooks: They must start with `use` and must be called at the top level of functional components or other hooks.
- **Benefits** include better code reusability, cleaner components, and easier maintenance.
- Common use cases include handling side effects, form management, API fetching, and tracking window dimensions.
- Custom hooks can be as simple as combining state management with side effects, or more complex for advanced logic.

Custom hooks empower developers to write cleaner, more modular, and reusable code while keeping the component hierarchy simple and manageable.