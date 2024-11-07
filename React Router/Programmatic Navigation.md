# Programmatic Navigation in React

Programmatic navigation in React refers to navigating between different routes or pages without relying on user interactions like clicking on links. Instead, navigation is triggered by JavaScript code based on certain conditions, events, or actions (e.g., after form submission, after a successful API request, etc.).

In React, programmatic navigation is usually handled using the **React Router** library, which provides methods like `useNavigate` (for React Router v6+) or `history.push()` (for React Router v5 and earlier).

---

### **1. React Router Overview**
React Router is the standard library for routing in React applications. It enables developers to manage navigation between different views, routes, and components. It allows for both declarative navigation (using `<Link />` components) and programmatic navigation (via JavaScript).

---

### **2. React Router v6+ (useNavigate Hook)**

In **React Router v6**, the navigation is controlled via the `useNavigate` hook. This hook provides a function that can be used to programmatically navigate to different routes.

#### **Syntax of `useNavigate`**
```jsx
const navigate = useNavigate();
```

#### **Navigating Programmatically**

You can call `navigate()` with either a path or an object, and optionally specify navigation behavior such as replacing the current history entry or controlling the number of steps to navigate.

**Example:**
```jsx
import React from 'react';
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  const handleSubmit = () => {
    // After some action, navigate to another route
    navigate('/dashboard');  // Navigate to the 'dashboard' route
  };

  return (
    <div>
      <button onClick={handleSubmit}>Go to Dashboard</button>
    </div>
  );
}

export default MyComponent;
```
**Explanation:**
- When the button is clicked, `navigate('/dashboard')` is called, programmatically navigating to the `/dashboard` route.

#### **Navigate with Query Parameters**

You can pass additional state or query parameters using the `navigate()` function.

```jsx
navigate('/dashboard', { state: { userId: 123 } });
```
To retrieve the state in the destination component:

```jsx
import { useLocation } from 'react-router-dom';

function Dashboard() {
  const location = useLocation();
  const userId = location.state?.userId;

  return <div>User ID: {userId}</div>;
}
```

#### **Navigate with Replace Option**

By default, `navigate()` pushes a new entry onto the history stack. If you want to replace the current entry in the history (for instance, after a login), you can use the `replace` option.

```jsx
navigate('/login', { replace: true });
```

This replaces the current route entry with the new one, meaning the user cannot navigate back to the previous route using the browser’s back button.

---

### **3. React Router v5 and Earlier (Using `history.push()` or `history.replace()`)**

In **React Router v5** and earlier, the `history` object is used to perform programmatic navigation. You can access the `history` object through the **withRouter** higher-order component or using hooks like `useHistory` (v5).

#### **Using `useHistory` Hook (v5)**

```jsx
import React from 'react';
import { useHistory } from 'react-router-dom';

function MyComponent() {
  const history = useHistory();

  const handleSubmit = () => {
    history.push('/dashboard');  // Navigate to the dashboard
  };

  return (
    <div>
      <button onClick={handleSubmit}>Go to Dashboard</button>
    </div>
  );
}

export default MyComponent;
```
**Explanation:**
- `history.push('/dashboard')` is used to navigate to the `/dashboard` route.

#### **Using `history.replace()` (v5)**

If you want to replace the current route in the history stack (similar to `navigate()` with `{ replace: true }` in v6), you can use `history.replace()`.

```jsx
history.replace('/login');  // Replaces the current route with '/login'
```

---

### **4. Conditional Programmatic Navigation**

Programmatic navigation is often used when certain conditions are met, such as after successful form submissions, authentication, or API requests.

#### **Example: Redirect After Form Submission**

```jsx
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function LoginForm() {
  const [username, setUsername] = useState('');
  const navigate = useNavigate();

  const handleSubmit = (event) => {
    event.preventDefault();

    // Simulate successful login
    if (username === 'admin') {
      navigate('/dashboard');  // Navigate to dashboard after login
    } else {
      alert('Invalid credentials');
    }
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

export default LoginForm;
```
**Explanation:**
- After submitting the form, if the username is valid, the user is navigated to the `/dashboard` route.
- If the username is invalid, an alert is shown.

---

### **5. Navigation Based on Events or Side Effects**

In some cases, you might want to navigate based on certain side effects or after performing an action, such as making an API request.

#### **Example: Redirect After API Call**

```jsx
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';

function SignupForm() {
  const [email, setEmail] = useState('');
  const navigate = useNavigate();

  const handleSubmit = async (event) => {
    event.preventDefault();

    // Simulating an API call to register the user
    const response = await fetch('/api/signup', {
      method: 'POST',
      body: JSON.stringify({ email }),
      headers: { 'Content-Type': 'application/json' }
    });

    if (response.ok) {
      // Redirect to the confirmation page after successful signup
      navigate('/confirmation');
    } else {
      alert('Signup failed');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Enter email"
      />
      <button type="submit">Sign Up</button>
    </form>
  );
}

export default SignupForm;
```
**Explanation:**
- After the form submission, an API request is made. If the signup is successful, the user is redirected to a `/confirmation` page.

---

### **6. Navigation with Scroll Restoration**

By default, React Router does not automatically restore the scroll position when navigating between routes. You can add scroll restoration behavior to your app by using `window.scrollTo()` after navigating programmatically.

#### **Example: Scroll to Top After Navigation**

```jsx
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  const goToHome = () => {
    navigate('/home');
    window.scrollTo(0, 0); // Scroll to the top after navigation
  };

  return <button onClick={goToHome}>Go to Home</button>;
}
```

---

### **7. History API (Advanced)**
In addition to React Router’s built-in navigation methods, you can also use the native **History API** for programmatic navigation.

#### **Example with `window.history.pushState()`**

```jsx
const navigateToHome = () => {
  window.history.pushState({}, '', '/home');
};
```

While this works for basic navigation, using **React Router** is generally preferred as it integrates well with the React component lifecycle and provides advanced features like dynamic route matching.

---

### **Conclusion**

Programmatic navigation in React is a powerful way to manage route transitions based on user actions or application logic. Whether using `useNavigate` in React Router v6+ or `history.push()` in React Router v5, programmatic navigation helps create more dynamic, event-driven web applications. Understanding how and when to use it is crucial for building user-friendly React applications with smooth navigation.