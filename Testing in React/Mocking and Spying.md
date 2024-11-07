# Mocking and Spying in Testing (with Jest)

Mocking and spying are critical techniques used in testing to isolate the code under test, control dependencies, and observe interactions in JavaScript applications. These techniques help simulate and track function behavior without relying on actual implementations, allowing for more efficient and focused unit tests.

In Jest (a popular testing framework for React and JavaScript applications), mocking and spying are integral to isolating components, testing their interactions, and verifying their behavior.

---

### 1. **Mocking in Jest**

Mocking allows you to replace functions, modules, or components with mock implementations in your tests. This helps isolate code under test by preventing real external dependencies from affecting the test environment.

#### **Purpose of Mocking:**
- Simulate behavior of complex or external dependencies.
- Avoid calling real APIs, databases, or file systems in tests.
- Control and inspect how the code interacts with the mocked dependencies.

#### **Creating Mocks:**
In Jest, you can create mocks in several ways.

##### **1.1. Mocking Functions (`jest.fn()`):**

- `jest.fn()` is used to create a mock function.
- It can track how many times a function was called, what arguments were passed, and what it returned.

**Example:**

```js
test('mocking a function', () => {
  const mockFunction = jest.fn();
  
  // Call the mock function
  mockFunction('hello');
  
  // Assertions
  expect(mockFunction).toHaveBeenCalledTimes(1);
  expect(mockFunction).toHaveBeenCalledWith('hello');
});
```

##### **1.2. Mocking Return Values:**

You can specify what a mock function should return when called.

**Example:**

```js
test('mocking return values', () => {
  const mockFunction = jest.fn().mockReturnValue(42);

  const result = mockFunction();
  
  expect(mockFunction).toHaveBeenCalledTimes(1);
  expect(result).toBe(42);
});
```

You can also mock the return value dynamically based on the input:

```js
test('mocking return values dynamically', () => {
  const mockFunction = jest.fn((x) => x + 2);
  
  expect(mockFunction(3)).toBe(5);
  expect(mockFunction(4)).toBe(6);
});
```

##### **1.3. Mocking Modules with `jest.mock()`**

If you want to mock an entire module (e.g., an API or utility), you can use `jest.mock()`.

**Example:**

```js
// utils.js
export const fetchData = async () => {
  // A real network request (which we don't want in tests)
  return 'real data';
};

// In the test file
jest.mock('./utils.js', () => ({
  fetchData: jest.fn().mockResolvedValue('mocked data')
}));

test('mocking an entire module', async () => {
  const { fetchData } = require('./utils');
  const data = await fetchData();
  
  expect(data).toBe('mocked data');
});
```

---

### 2. **Spying in Jest**

Spying is used to track the behavior of a function, method, or object without altering its actual implementation. This allows you to observe how functions are called, with what arguments, and how many times they are invoked.

#### **Purpose of Spying:**
- Monitor and track the usage of functions without changing their behavior.
- Test if certain functions or methods were called with expected arguments.
- Check how often functions were called within the component or class.

#### **Creating Spies:**
In Jest, you can create spies using `jest.spyOn()`. This allows you to spy on methods of objects (e.g., class methods, global methods).

##### **2.1. Spying on Methods:**

You can spy on object methods or class methods to see if they were called.

**Example:**

```js
test('spying on a method', () => {
  const object = {
    method: (x) => x * 2,
  };
  
  // Spy on the 'method' of the object
  const spy = jest.spyOn(object, 'method');
  
  object.method(3);
  
  expect(spy).toHaveBeenCalledTimes(1);
  expect(spy).toHaveBeenCalledWith(3);
});
```

##### **2.2. Spying on `console` Methods:**

You can spy on methods like `console.log`, `console.error`, etc., to check if they were called during a test.

**Example:**

```js
test('spying on console.log', () => {
  const spy = jest.spyOn(console, 'log').mockImplementation(() => {});

  console.log('Hello World');
  
  expect(spy).toHaveBeenCalledWith('Hello World');
  
  spy.mockRestore(); // Restore the original console.log implementation
});
```

##### **2.3. Spying on Instance Methods (Class Methods):**

You can spy on instance methods of a class or objects.

**Example:**

```js
class MyClass {
  sayHello(name) {
    return `Hello, ${name}`;
  }
}

test('spying on class methods', () => {
  const instance = new MyClass();
  
  const spy = jest.spyOn(instance, 'sayHello');
  
  instance.sayHello('Afsal');
  
  expect(spy).toHaveBeenCalledTimes(1);
  expect(spy).toHaveBeenCalledWith('Afsal');
});
```

---

### 3. **Mocking Timers**

In JavaScript, functions like `setTimeout`, `setInterval`, and `Date.now()` can be mocked to test time-sensitive logic without having to wait for actual time delays.

#### **Mocking `setTimeout` and `setInterval`:**

Jest provides functions to mock timers and simulate delayed executions.

**Example:**

```js
jest.useFakeTimers();

test('mocking setTimeout', () => {
  const mockFunction = jest.fn();

  setTimeout(mockFunction, 1000);

  // Fast-forward until all timers have been executed
  jest.runAllTimers();

  expect(mockFunction).toHaveBeenCalledTimes(1);
});
```

#### **Mocking `Date.now()`**

```js
test('mocking Date.now()', () => {
  const dateNowSpy = jest.spyOn(Date, 'now').mockReturnValue(1234567890);

  const result = Date.now();
  
  expect(result).toBe(1234567890);

  dateNowSpy.mockRestore();
});
```

---

### 4. **Resetting and Restoring Mocks/Spies**

After creating mocks and spies, it’s important to clean up after tests to ensure that tests do not interfere with each other.

- **`mockReset()`**: Resets the mock’s state (e.g., call count, arguments).
- **`mockRestore()`**: Restores the original function or method if it was spied on.

**Example:**

```js
const mockFunction = jest.fn();

test('mock reset', () => {
  mockFunction();
  expect(mockFunction).toHaveBeenCalledTimes(1);
  
  mockFunction.mockReset();
  
  expect(mockFunction).toHaveBeenCalledTimes(0); // Resets call count
});
```

---

### 5. **Summary of Key Concepts**

| **Method**               | **Description**                                            |
|--------------------------|------------------------------------------------------------|
| `jest.fn()`               | Creates a mock function that tracks calls and arguments.   |
| `jest.mock()`             | Mocks entire modules or specific functions.               |
| `jest.spyOn()`            | Spies on functions or methods without changing their behavior. |
| `jest.useFakeTimers()`    | Mocks `setTimeout`, `setInterval`, and `Date.now()`.       |
| `mockReset()`             | Resets the state of mocks and spies.                       |
| `mockRestore()`           | Restores the original implementation of spied functions.   |

---

### 6. **Best Practices**

- **Use mocks for external dependencies**: Mock out external APIs, modules, and other side effects that are not relevant to the current test.
- **Avoid over-mocking**: Mock only what's necessary. Over-mocking can make tests harder to maintain.
- **Use spies for tracking function calls**: Use spies when you want to observe interactions with functions but don't need to change their behavior.
- **Ensure proper cleanup**: Always reset or restore mocks after tests to avoid affecting other tests.