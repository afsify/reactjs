# CSS Modules in React

**CSS Modules** is a technique for writing modular, reusable, and scoped CSS. It helps to avoid the global scope issues commonly found with regular CSS by scoping styles locally to components. This approach ensures that styles are only applied to the component they are intended for, preventing naming conflicts and making the CSS more maintainable.

### Key Concepts

1. **Modular CSS**: Each CSS file is scoped to the component, meaning styles in one component won't accidentally affect others.
2. **Local Scoping**: In CSS Modules, class names are scoped locally by default. When you import a CSS module into a React component, the class names are automatically converted into unique identifiers to avoid collisions.
3. **Class Name Mapping**: CSS classes are imported as objects, and the class names are mapped to unique values at build time.

---

### 1. **Setting Up CSS Modules in React**

CSS Modules are supported by default in Create React App (CRA) or other bundlers like Webpack. To use CSS Modules in React, follow these steps:

#### Step 1: Create a CSS file with `.module.css` extension.

For example:
```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
}
```

#### Step 2: Import the CSS Module into your React component.

```jsx
import React from 'react';
import styles from './Button.module.css';  // Importing CSS module

function Button() {
  return <button className={styles.button}>Click me</button>;
}

export default Button;
```

Notice that the `styles.button` is used instead of a plain class name. This is how CSS Modules prevent class name collisions by generating a unique class name.

---

### 2. **How CSS Modules Work**

CSS Modules works by transforming your CSS files so that all class names and animation names are locally scoped. When the CSS file is imported into the React component, the class names are replaced with unique identifiers at build time.

For example, the CSS file:
```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
}
```

May be transformed into something like:
```css
/* Compiled output (Button.module.css) */
.button_Button__1k4gD {
  background-color: blue;
  color: white;
}
```

And in your component:
```jsx
<button className={styles.button_Button__1k4gD}>Click me</button>
```

This unique class name (`button_Button__1k4gD`) ensures that styles do not conflict with other components' styles.

---

### 3. **Using CSS Modules with Dynamic Class Names**

You can dynamically apply CSS classes using JavaScript expression and template literals. This is useful when you need to conditionally apply a class or combine multiple classes.

```jsx
import React, { useState } from 'react';
import styles from './Button.module.css';

function Button() {
  const [isClicked, setIsClicked] = useState(false);
  
  const handleClick = () => {
    setIsClicked(!isClicked);
  };

  return (
    <button
      className={`${styles.button} ${isClicked ? styles.clicked : ''}`}
      onClick={handleClick}
    >
      {isClicked ? 'Clicked!' : 'Click me'}
    </button>
  );
}

export default Button;
```

In this case, the `clicked` class is conditionally applied based on the component's state.

---

### 4. **Nested Styles with CSS Modules**

CSS Modules also support **nesting** of styles, but you need to use the `composes` keyword or write more specific styles within the module.

For example:
```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
}

.buttonPrimary {
  composes: button from './Button.module.css'; /* Composing a base style */
  font-size: 16px;
}
```

However, nesting as you would in SCSS or LESS is not directly supported with CSS Modules unless using a preprocessor like SCSS.

---

### 5. **Advantages of CSS Modules**

1. **Local Scoping**: Styles are scoped locally to the component, preventing class name conflicts and ensuring that styles only affect the component they belong to.
2. **Modular and Reusable**: Styles are reusable and easily portable between components without the risk of style leakage.
3. **Collaboration-Friendly**: Since styles are scoped, multiple developers can work on different components without worrying about conflicting styles.
4. **Cleaner Global Styles**: Since global styles are avoided, you can use CSS Modules without polluting the global scope.

---

### 6. **Disadvantages of CSS Modules**

1. **Requires Build Step**: Since CSS Modules rely on a build tool (e.g., Webpack), it's not suitable for projects that do not use a build pipeline.
2. **Limited to Scoped Classes**: While CSS Modules work well for scoping classes, handling global styles (e.g., body styles, reset styles) may require a workaround, such as using a global stylesheet or external libraries.
3. **Cannot Easily Share Global Styles**: If you want to apply global styles, you would need to manage them separately, outside the module system.

---

### 7. **Best Practices for Using CSS Modules**

- **Use Descriptive Names**: Choose meaningful class names in your CSS module to improve readability and maintainability.
  
  ```css
  /* Button.module.css */
  .primaryButton {
    background-color: blue;
    color: white;
  }
  ```

- **Avoid Deep Nesting**: Since CSS Modules are intended for component-level styles, deep nesting can make the styles harder to maintain. Keep your CSS rules flat.

- **Global Styles**: For global styles (e.g., resets, fonts), use global CSS files that aren't scoped by CSS Modules or use a different approach like `styled-components`.

- **Composition**: Use the `composes` keyword to compose styles from other CSS Modules when needed. This keeps your styles reusable and modular.

---

### 8. **Working with CSS Variables in CSS Modules**

You can also use CSS variables in CSS Modules to allow for dynamic styling in components:

```css
/* Button.module.css */
:root {
  --button-color: blue;
}

.button {
  background-color: var(--button-color);
  color: white;
}
```

This approach allows dynamic theming, where the CSS variable can be modified in JavaScript to change styles.

---

### 9. **Handling CSS Modules with TypeScript**

When using TypeScript, it's necessary to tell TypeScript how to handle the imported CSS Modules. You can do this by creating a declaration file.

1. **Create a `css-modules.d.ts` file** in your `src` directory:
   
   ```typescript
   declare module '*.module.css' {
     const classes: { [key: string]: string };
     export default classes;
   }
   ```

2. **Then, use the CSS Module as you normally would in a component**:
   
   ```tsx
   import styles from './Button.module.css';
   ```

This ensures TypeScript recognizes the imports from `.module.css` files as objects with string keys and values.

---

### 10. **CSS Modules with Styled Components or Emotion**

While **styled-components** and **emotion** are alternative CSS-in-JS solutions, CSS Modules can still be used in conjunction with these libraries. For example, you can use CSS Modules to handle local component styles and **styled-components** for more complex or conditional styles.

---

### Conclusion

CSS Modules provide a way to write CSS that is scoped to individual components in React, which avoids global namespace pollution and ensures styles do not leak. By using unique class names generated at build time, you get the benefits of modularity and maintainability while still writing plain CSS. The approach is compatible with modern React applications, including those using TypeScript, and can be used alongside other styling solutions like **styled-components**.