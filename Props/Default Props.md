# Default Props in React

In React, **default props** are used to define default values for props in case the parent component does not provide them. They help ensure that your components have sensible default values, preventing undefined behavior when props are not passed explicitly. 

---

### 1. **What are Default Props?**
Default props are values assigned to props in a React component when they are not provided by the parent. These default values ensure that the component can render correctly even if the parent doesn't pass all expected props.

For example, if a parent component doesn’t pass a specific prop, the child component can use the default value specified for that prop.

---

### 2. **Why Use Default Props?**
- **Avoid Undefined Errors**: Default props ensure that a component can safely render even when a prop is missing or undefined.
- **Simplify Component Logic**: They help avoid checking if a prop is undefined every time you use it inside the component.
- **Improve Readability**: Default props can make your component’s interface more predictable and easier to understand.
- **Enhance Reusability**: With default props, you can reuse the same component with different configurations without needing to always pass every prop.

---

### 3. **How to Define Default Props**

#### 3.1 **Setting Default Props for a Functional Component**

In functional components, you can define default props directly by setting `defaultProps` on the component.

**Example:**
```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Defining default props
Greeting.defaultProps = {
  name: 'Guest'
};
```

In this example, if the `name` prop is not provided by the parent component, the default value `'Guest'` will be used.

---

#### 3.2 **Setting Default Props for a Class Component**

In class components, default props are defined in a similar manner. You assign the `defaultProps` object to the class component.

**Example:**
```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

// Defining default props
Greeting.defaultProps = {
  name: 'Guest'
};
```

Just like in the functional component, if the `name` prop is missing, `'Guest'` will be used as the default value.

---

### 4. **Accessing Default Props**

When you access props in a React component, the default props are already merged with the props passed from the parent. React automatically assigns the default value when a prop is not passed.

**Example:**
```jsx
function Welcome(props) {
  return <h2>Welcome, {props.name}!</h2>;
}

Welcome.defaultProps = {
  name: 'Visitor'
};

function App() {
  return <Welcome />;
}

```

- In this case, `App` does not provide a `name` prop to the `Welcome` component, so `Visitor` will be used as the default value.

---

### 5. **Default Props in Nested Components**

If you're working with nested components, you can use default props to ensure that deeply nested components also receive sensible defaults.

**Example:**
```jsx
function Profile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.age} years old</p>
    </div>
  );
}

Profile.defaultProps = {
  user: {
    name: 'Anonymous',
    age: 'Not provided'
  }
};

function App() {
  return <Profile />;
}
```
Here, if `user` is not provided by the parent component, the `Profile` component will use the default values for `name` and `age`.

---

### 6. **Default Props with Functional Components Using Destructuring**

When using destructuring in functional components, default values can be set directly in the destructuring pattern.

**Example:**
```jsx
function Greeting({ name = 'Guest' }) {
  return <h1>Hello, {name}!</h1>;
}
```

In this example, the default value `'Guest'` is assigned to `name` if no `name` prop is provided by the parent.

---

### 7. **Default Props with PropTypes**

React allows you to combine `defaultProps` with **PropTypes** to validate the types of props and provide default values for them.

**Example:**
```jsx
import PropTypes from 'prop-types';

function Greeting({ name, age }) {
  return <h1>{name} is {age} years old.</h1>;
}

Greeting.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
};

Greeting.defaultProps = {
  name: 'Guest',
  age: 30
};
```

In this example:
- `name` is a required prop (must be passed by the parent).
- `age` is an optional prop with a default value of `30` if not provided.

---

### 8. **Behavior of Default Props in Case of `null` or `undefined`**

- **When the prop is explicitly passed as `undefined`**: The default value will be used because `undefined` is considered "missing" by React.

- **When the prop is explicitly passed as `null`**: The default value will **not** be used. React treats `null` as a valid value.

**Example:**
```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

Greeting.defaultProps = {
  name: 'Guest'
};

function App() {
  return (
    <>
      <Greeting />  {/* Outputs: Hello, Guest! */}
      <Greeting name={null} />  {/* Outputs: Hello, null! */}
      <Greeting name={undefined} />  {/* Outputs: Hello, Guest! */}
    </>
  );
}
```

---

### 9. **Overriding Default Props**

Props passed from the parent always take precedence over default props. If a parent provides a value for a prop, the default value is ignored.

**Example:**
```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

Greeting.defaultProps = {
  name: 'Guest'
};

function App() {
  return <Greeting name="Afsal" />;  {/* Outputs: Hello, Afsal! */}
}
```
In this case, the default value `'Guest'` is overridden by the value `'Afsal'` passed from the parent.

---

### 10. **Best Practices for Using Default Props**

- **Always define default props for optional props** to avoid undefined values and improve component robustness.
- **Use default props for complex objects**: Avoid sending `undefined` for complex objects, arrays, or functions unless required. Define sensible defaults to ensure the component works as expected.
- **Use PropTypes** along with defaultProps to validate and ensure the correct data is being passed to components.
- **Use destructuring with default values** in functional components when appropriate for cleaner code.

---

### Key Points:
- Default props are used to provide fallback values when props are not provided.
- You can define default props using `defaultProps` on both functional and class components.
- In functional components, you can also use destructuring to assign default values.
- Default props can be combined with PropTypes for type checking and ensuring the correct values are passed to components.
- React will not use default props if a prop is explicitly passed as `null`.

---

By using default props, you can ensure that your components are flexible, robust, and easy to maintain, even when certain props are not always provided by the parent component.