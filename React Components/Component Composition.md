# **Component Composition in React**

Component composition is a key concept in React that refers to combining components to build complex UIs by nesting them together. Instead of creating a single monolithic component, composition allows us to break down an interface into smaller, reusable components, each responsible for its own part of the UI. This approach enhances code reusability, readability, and maintainability.

### Why Use Component Composition?
- **Reusability**: Smaller, independent components can be reused across different parts of an application.
- **Separation of Concerns**: Each component has a specific responsibility, making the code more modular.
- **Improved Maintainability**: Smaller components are easier to test, debug, and maintain over time.
- **Enhanced Flexibility**: Different components can be combined in various ways to create dynamic, complex UIs.

### Key Concepts in Component Composition
1. **Container and Presentational Components**
   - **Container Components**: Handle data fetching, state management, and pass data down as props.
   - **Presentational Components**: Focus on the UI and display the received data without managing the state.
   
   **Example:**
   ```javascript
   function UserListContainer() {
     const [users, setUsers] = useState([]);
     
     useEffect(() => {
       fetchUsers().then(setUsers);
     }, []);

     return <UserList users={users} />;
   }

   function UserList({ users }) {
     return (
       <ul>
         {users.map(user => (
           <UserItem key={user.id} user={user} />
         ))}
       </ul>
     );
   }
   ```

2. **Composition through Props**
   - Components can accept other components as props to create flexible and reusable structures.
   - This pattern allows a parent component to render different child components based on what is passed in.
   
   **Example:**
   ```javascript
   function Card({ children }) {
     return <div className="card">{children}</div>;
   }

   function App() {
     return (
       <Card>
         <h1>Title</h1>
         <p>Some content inside the card.</p>
       </Card>
     );
   }
   ```

3. **Children Prop**
   - The `children` prop is a special prop that lets you pass elements between the opening and closing tags of a component.
   - Using `children` allows you to nest components, making it possible to create complex layouts.

   **Example:**
   ```javascript
   function Modal({ children }) {
     return (
       <div className="modal">
         <div className="modal-content">{children}</div>
       </div>
     );
   }

   function App() {
     return (
       <Modal>
         <h2>Modal Title</h2>
         <p>This is the modal content.</p>
       </Modal>
     );
   }
   ```

4. **Higher-Order Components (HOCs)**
   - Higher-Order Components are functions that take a component as an argument and return a new component with additional props or functionality.
   - HOCs are useful for reusing logic across multiple components without duplicating code.

   **Example:**
   ```javascript
   function withLoading(Component) {
     return function LoadingComponent({ isLoading, ...props }) {
       if (isLoading) return <p>Loading...</p>;
       return <Component {...props} />;
     };
   }

   const UserListWithLoading = withLoading(UserList);

   function App() {
     return <UserListWithLoading isLoading={true} users={[]} />;
   }
   ```

5. **Render Props Pattern**
   - The Render Props pattern involves passing a function as a prop to a component, allowing more control over what to render.
   - This technique is helpful when a component needs to share logic but requires flexibility in rendering the UI.

   **Example:**
   ```javascript
   function DataFetcher({ render }) {
     const [data, setData] = useState(null);

     useEffect(() => {
       fetchData().then(setData);
     }, []);

     return render(data);
   }

   function App() {
     return (
       <DataFetcher
         render={(data) => (
           <div>
             {data ? <p>{data}</p> : <p>Loading...</p>}
           </div>
         )}
       />
     );
   }
   ```

6. **Context API for Deeply Nested Components**
   - The Context API allows you to pass data down through many layers of nested components without passing props at each level.
   - It is especially useful for passing global data, such as themes or user information, that many components need access to.

   **Example:**
   ```javascript
   const ThemeContext = React.createContext('light');

   function App() {
     return (
       <ThemeContext.Provider value="dark">
         <Toolbar />
       </ThemeContext.Provider>
     );
   }

   function Toolbar() {
     return <ThemedButton />;
   }

   function ThemedButton() {
     const theme = useContext(ThemeContext);
     return <button className={theme}>Themed Button</button>;
   }
   ```

### Common Patterns for Component Composition
1. **Page Layout Composition**
   - Create reusable layout components (like Header, Sidebar, Footer) that can be used across multiple pages.
   - Example: Using a `Layout` component with slots for `Header`, `Sidebar`, and `Footer`.

2. **Compound Components**
   - Components designed to work together, where each sub-component handles a specific part of the whole.
   - Useful for creating complex UI elements like tabs or forms with a cohesive structure.

   **Example:**
   ```javascript
   function Tabs({ children }) {
     return <div className="tabs">{children}</div>;
   }

   function Tab({ title, children }) {
     return (
       <div className="tab">
         <h3>{title}</h3>
         <div>{children}</div>
       </div>
     );
   }

   function App() {
     return (
       <Tabs>
         <Tab title="Tab 1">Content for Tab 1</Tab>
         <Tab title="Tab 2">Content for Tab 2</Tab>
       </Tabs>
     );
   }
   ```

3. **Slots Pattern**
   - Similar to the children prop but more explicit, allowing specific named slots for content.
   - Often used in frameworks like Vue but can be implemented in React for better control over component structure.

   **Example:**
   ```javascript
   function Dialog({ header, body, footer }) {
     return (
       <div className="dialog">
         <div className="dialog-header">{header}</div>
         <div className="dialog-body">{body}</div>
         <div className="dialog-footer">{footer}</div>
       </div>
     );
   }

   function App() {
     return (
       <Dialog
         header={<h1>Dialog Title</h1>}
         body={<p>This is the dialog content.</p>}
         footer={<button>Close</button>}
       />
     );
   }
   ```

### Summary
- **Container & Presentational Components**: Separate data handling from presentation.
- **Props and Children**: Pass components as props or use `children` to nest components.
- **HOCs and Render Props**: Reuse logic without duplicating code.
- **Context API**: Share data across deeply nested components.
- **Layout & Compound Components**: Modular and reusable structures for page layouts and complex UI elements.

Component composition in React encourages building UIs in a modular, flexible, and reusable manner. By understanding and implementing these patterns, developers can create scalable, maintainable, and dynamic applications.