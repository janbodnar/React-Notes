# React Functional Components

## Introduction

Functional components are the modern, recommended way to build React  
applications. They are JavaScript functions that accept props as arguments  
and return React elements (JSX). Unlike class components, functional  
components are simpler, more concise, and easier to test.  

With the introduction of Hooks in React 16.8, functional components gained  
the ability to use state and lifecycle features previously only available in  
class components. This makes them the preferred choice for new React code.  

**Key characteristics of functional components:**  

- Written as JavaScript functions (regular or arrow functions)  
- Accept props as a single argument  
- Return JSX elements describing what should appear on the screen  
- Can use Hooks to add state and side effects  
- More predictable and easier to debug than class components  
- Support better code reusability through custom hooks  

Modern React applications are built almost entirely with functional  
components, leveraging hooks for state management, side effects, context,  
and performance optimization.  

---

## Basic Functional Component

A minimal functional component that renders static content.  

```jsx
function Welcome() {
  return <h1>Hello there!</h1>;
}

export default Welcome;
```

This is the simplest form of a functional component. It's a JavaScript  
function that returns JSX. The component name must start with a capital  
letter to differentiate it from regular HTML elements. This component can  
be used in other components by writing `<Welcome />`.  

---

## Functional Component with Props

Components accept data through props (properties) to make them reusable.  

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```

Props are passed to the component as a single object parameter. You access  
individual props using dot notation like `props.name`. This makes the  
component dynamic and reusable with different data. Usage example:  
`<Greeting name="Alice" />` will render "Hello, Alice!".  

---

## Functional Component with Destructured Props

Destructuring props in the function signature makes code cleaner.  

```jsx
function UserCard({ name, age, occupation }) {
  return (
    <div className="user-card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Occupation: {occupation}</p>
    </div>
  );
}

export default UserCard;
```

Destructuring extracts specific properties from the props object directly in  
the parameter list. This eliminates the need to write `props.name`,  
`props.age`, etc., making the code more readable. It's the preferred  
pattern in modern React development.  

---

## Functional Component with Default Props

Default values ensure components work even when props are missing.  

```jsx
function Button({ text = "Click me", variant = "primary" }) {
  return <button className={`btn btn-${variant}`}>{text}</button>;
}

export default Button;
```

Default parameter values (ES6 feature) provide fallback values when props  
aren't provided. If `<Button />` is rendered without props, it will use  
"Click me" and "primary" as defaults. This prevents undefined values and  
makes components more robust.  

---

## Arrow Function Component

Arrow functions provide a more concise syntax for functional components.  

```jsx
const Header = ({ title, subtitle }) => (
  <header>
    <h1>{title}</h1>
    {subtitle && <h2>{subtitle}</h2>}
  </header>
);

export default Header;
```

Arrow functions with implicit return (no curly braces) are perfect for  
simple components. The parentheses around the JSX allow multi-line returns.  
This style is popular in modern React for its brevity. The `subtitle &&`  
syntax conditionally renders the subtitle only if it exists.  

---

## Component Returning Multiple Elements with Fragment

Fragments let you group elements without adding extra DOM nodes.  

```jsx
function ProductInfo({ name, price, description }) {
  return (
    <>
      <h3>{name}</h3>
      <p className="price">${price.toFixed(2)}</p>
      <p className="description">{description}</p>
    </>
  );
}

export default ProductInfo;
```

The `<>` and `</>` syntax is shorthand for `<React.Fragment>`. Fragments  
are useful when you need to return multiple sibling elements without  
wrapping them in an unnecessary `<div>`. This keeps the DOM structure clean  
and semantic.  

---

## Component with Conditional Rendering

Components can render different content based on conditions.  

```jsx
function StatusMessage({ isLoggedIn, username }) {
  if (isLoggedIn) {
    return <p>Welcome back, {username}!</p>;
  }
  return <p>Please log in to continue.</p>;
}

export default StatusMessage;
```

Conditional rendering allows components to display different output based on  
props or state. This example uses an if-else statement, but you can also  
use ternary operators or logical AND operators. This pattern is fundamental  
for creating dynamic, interactive UIs.  

---

## Component with Ternary Conditional Rendering

Ternary operators provide inline conditional rendering.  

```jsx
function Alert({ type, message }) {
  return (
    <div className={`alert alert-${type}`}>
      {type === "error" ? "⚠️ " : "ℹ️ "}
      {message}
    </div>
  );
}

export default Alert;
```

The ternary operator (`condition ? true : false`) is perfect for simple  
conditional rendering within JSX. It's more concise than if-else statements  
and works well when you need to choose between two values based on a  
condition. This example adds different icons based on alert type.  

---

## Component Rendering Lists

Rendering arrays of data is common in React applications.  

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>
          {todo.completed ? "✓ " : "○ "}
          {todo.text}
        </li>
      ))}
    </ul>
  );
}

export default TodoList;
```

The `map()` method transforms an array into an array of JSX elements. Each  
element must have a unique `key` prop to help React identify which items  
changed. Keys should be stable identifiers (like IDs), not array indexes.  
This pattern is essential for rendering dynamic lists.  

---

## Component with Event Handlers

Event handlers make components interactive.  

```jsx
function Counter({ initialCount = 0 }) {
  const [count, setCount] = React.useState(initialCount);

  const handleIncrement = () => {
    setCount(count + 1);
  };

  const handleDecrement = () => {
    setCount(count - 1);
  };

  return (
    <div className="counter">
      <button onClick={handleDecrement}>-</button>
      <span>{count}</span>
      <button onClick={handleIncrement}>+</button>
    </div>
  );
}

export default Counter;
```

Event handlers are functions that respond to user actions. In React, you  
pass them to elements using camelCase attributes like `onClick`. This  
example uses `useState` to manage the counter value, demonstrating how  
functional components handle both interaction and state.  

---

## Component with useState Hook

The useState hook adds state management to functional components.  

```jsx
import { useState } from 'react';

function ToggleSwitch({ label }) {
  const [isOn, setIsOn] = useState(false);

  return (
    <div className="toggle-switch">
      <label>{label}</label>
      <button onClick={() => setIsOn(!isOn)}>
        {isOn ? "ON" : "OFF"}
      </button>
    </div>
  );
}

export default ToggleSwitch;
```

`useState` returns a pair: the current state value and a function to update  
it. The argument to `useState` is the initial state. When you call the  
setter function, React re-renders the component with the new state. This  
hook is the foundation of stateful functional components.  

---

## Component with Multiple State Variables

Components can manage multiple independent state values.  

```jsx
import { useState } from 'react';

function RegistrationForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [agreeToTerms, setAgreeToTerms] = useState(false);

  return (
    <form>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Password"
      />
      <label>
        <input
          type="checkbox"
          checked={agreeToTerms}
          onChange={(e) => setAgreeToTerms(e.target.checked)}
        />
        I agree to terms
      </label>
    </form>
  );
}

export default RegistrationForm;
```

Multiple `useState` calls let you manage separate pieces of state  
independently. Each state variable has its own setter function. This  
approach is cleaner than storing everything in one object when the state  
pieces are unrelated. React batches state updates for performance.  

---

## Component with useEffect Hook

The useEffect hook handles side effects like data fetching.  

```jsx
import { useState, useEffect } from 'react';

function DocumentTitle({ title }) {
  useEffect(() => {
    document.title = title;
  }, [title]);

  return <h1>{title}</h1>;
}

export default DocumentTitle;
```

`useEffect` runs after every render by default. The dependency array  
(second argument) controls when it runs—only when dependencies change. This  
example updates the browser tab title when the `title` prop changes. Empty  
array `[]` means run once; no array means run every render.  

---

## Component Fetching Data with useEffect

A common pattern for loading data when a component mounts.  

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    setLoading(true);
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, [userId]);

  if (loading) return <p>Loading...</p>;
  if (!user) return <p>User not found</p>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}

export default UserProfile;
```

This pattern handles the complete data loading lifecycle: loading state,  
fetching data, and displaying results. The effect runs when `userId`  
changes, allowing the component to fetch new data. Always handle loading  
and error states for better user experience.  

---

## Component with useContext Hook

The useContext hook accesses values from React Context.  

```jsx
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function ThemedButton({ children }) {
  const theme = useContext(ThemeContext);

  return (
    <button
      style={{
        background: theme.background,
        color: theme.foreground,
      }}
    >
      {children}
    </button>
  );
}

export default ThemedButton;
```

`useContext` provides access to context values without wrapper components.  
Context is useful for passing data through the component tree without  
manually passing props at every level. Common uses include themes, user  
authentication, and language preferences.  

---

## Component with useReducer Hook

The useReducer hook manages complex state logic.  

```jsx
import { useReducer } from 'react';

function shoppingCartReducer(state, action) {
  switch (action.type) {
    case 'ADD_ITEM':
      return [...state, action.item];
    case 'REMOVE_ITEM':
      return state.filter(item => item.id !== action.id);
    case 'CLEAR_CART':
      return [];
    default:
      return state;
  }
}

function ShoppingCart() {
  const [cart, dispatch] = useReducer(shoppingCartReducer, []);

  return (
    <div>
      <button onClick={() => dispatch({ type: 'ADD_ITEM', item: { id: 1, name: 'Book' } })}>
        Add Book
      </button>
      <button onClick={() => dispatch({ type: 'CLEAR_CART' })}>
        Clear Cart
      </button>
      <ul>
        {cart.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default ShoppingCart;
```

`useReducer` is preferable to `useState` when state logic is complex or  
involves multiple sub-values. It follows the Redux pattern: dispatch actions  
to a reducer function that returns new state. This makes state updates more  
predictable and testable.  

---

## Component with useMemo Hook

The useMemo hook optimizes expensive calculations.  

```jsx
import { useState, useMemo } from 'react';

function ExpensiveCalculation({ numbers }) {
  const [multiplier, setMultiplier] = useState(1);

  const total = useMemo(() => {
    console.log('Calculating total...');
    return numbers.reduce((sum, num) => sum + num, 0) * multiplier;
  }, [numbers, multiplier]);

  return (
    <div>
      <p>Total: {total}</p>
      <button onClick={() => setMultiplier(multiplier + 1)}>
        Increase Multiplier
      </button>
    </div>
  );
}

export default ExpensiveCalculation;
```

`useMemo` memoizes computed values, recalculating only when dependencies  
change. This prevents expensive operations from running on every render.  
Use it for computationally intensive operations, not simple calculations.  
The memoized value is returned directly, not in a function.  

---

## Component with useCallback Hook

The useCallback hook memoizes functions to prevent unnecessary re-renders.  

```jsx
import { useState, useCallback } from 'react';

function SearchableList({ items }) {
  const [query, setQuery] = useState('');

  const handleSearch = useCallback((event) => {
    setQuery(event.target.value);
  }, []);

  const filteredItems = items.filter(item =>
    item.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <div>
      <input
        type="text"
        value={query}
        onChange={handleSearch}
        placeholder="Search..."
      />
      <ul>
        {filteredItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default SearchableList;
```

`useCallback` returns a memoized version of the callback function that only  
changes if dependencies change. This is useful when passing callbacks to  
optimized child components that rely on reference equality to prevent  
unnecessary renders. Without dependencies, the function is created once.  

---

## Component with useRef Hook

The useRef hook creates mutable references that persist across renders.  

```jsx
import { useRef, useEffect } from 'react';

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return (
    <div>
      <input
        ref={inputRef}
        type="text"
        placeholder="This input will auto-focus"
      />
    </div>
  );
}

export default FocusInput;
```

`useRef` creates a mutable reference object whose `.current` property  
persists across renders. It's commonly used to access DOM elements directly  
or store values that shouldn't trigger re-renders when changed. Unlike  
state, updating a ref doesn't cause a re-render.  

---

## Component with Custom Hook

Custom hooks extract reusable logic into separate functions.  

```jsx
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return width;
}

function ResponsiveComponent() {
  const width = useWindowWidth();

  return (
    <div>
      <p>Window width: {width}px</p>
      {width < 768 ? <p>Mobile view</p> : <p>Desktop view</p>}
    </div>
  );
}

export default ResponsiveComponent;
```

Custom hooks are functions that start with "use" and can call other hooks.  
They let you extract component logic into reusable functions. This example  
creates a hook that tracks window width, which can be reused across  
multiple components. The cleanup function prevents memory leaks.  

---

## Component with Multiple Hooks Combined

Real-world components often combine several hooks.  

```jsx
import { useState, useEffect, useCallback } from 'react';

function DataTable({ apiEndpoint }) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [sortField, setSortField] = useState('name');

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch(apiEndpoint);
      const json = await response.json();
      setData(json);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }, [apiEndpoint]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  const sortedData = [...data].sort((a, b) =>
    a[sortField] > b[sortField] ? 1 : -1
  );

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <button onClick={() => setSortField('name')}>Sort by Name</button>
      <button onClick={() => setSortField('date')}>Sort by Date</button>
      <ul>
        {sortedData.map(item => (
          <li key={item.id}>{item.name} - {item.date}</li>
        ))}
      </ul>
    </div>
  );
}

export default DataTable;
```

This component demonstrates combining multiple hooks for a complete  
feature. It uses `useState` for multiple state values, `useEffect` for data  
fetching, and `useCallback` to memoize the fetch function. This pattern is  
common in production applications.  

---

## Controlled Form Component

Controlled components sync form inputs with React state.  

```jsx
import { useState } from 'react';

function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      <button type="submit">Send</button>
    </form>
  );
}

export default ContactForm;
```

In controlled components, React state is the "single source of truth" for  
form values. Every input's value comes from state, and every change updates  
state. This gives you full control over form behavior and makes validation  
easier. The spread operator preserves other form fields when updating.  

---

## Uncontrolled Form Component with Refs

Uncontrolled components use refs to access form values.  

```jsx
import { useRef } from 'react';

function QuickForm() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    const formData = {
      name: nameRef.current.value,
      email: emailRef.current.value
    };
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={nameRef}
        type="text"
        placeholder="Name"
        defaultValue=""
      />
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
        defaultValue=""
      />
      <button type="submit">Submit</button>
    </form>
  );
}

export default QuickForm;
```

Uncontrolled components let the DOM handle form state, accessing values via  
refs when needed. This is simpler for basic forms but gives you less  
control. Use `defaultValue` instead of `value` to set initial values. This  
approach is useful for simple forms or integrating with non-React code.  

---

## Component with Form Validation

Form validation ensures data quality before submission.  

```jsx
import { useState } from 'react';

function SignupForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});

  const validate = () => {
    const newErrors = {};
    if (!email.includes('@')) {
      newErrors.email = 'Invalid email address';
    }
    if (password.length < 8) {
      newErrors.password = 'Password must be at least 8 characters';
    }
    return newErrors;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const newErrors = validate();
    if (Object.keys(newErrors).length === 0) {
      console.log('Form is valid!', { email, password });
    } else {
      setErrors(newErrors);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Email"
        />
        {errors.email && <span className="error">{errors.email}</span>}
      </div>
      <div>
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      <button type="submit">Sign Up</button>
    </form>
  );
}

export default SignupForm;
```

Client-side validation provides immediate feedback to users. This example  
validates on submit, but you can also validate on blur or change. Always  
perform server-side validation too, as client-side validation can be  
bypassed. Storing errors in state allows displaying multiple error messages.  

---

## Component with Children Props

Children props allow component composition and nesting.  

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      {title && <h3 className="card-title">{title}</h3>}
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

function App() {
  return (
    <Card title="User Profile">
      <p>Name: John Doe</p>
      <p>Role: Developer</p>
    </Card>
  );
}

export default Card;
```

The `children` prop contains everything between a component's opening and  
closing tags. This enables powerful composition patterns where components  
can wrap and enhance arbitrary content. It's fundamental to creating  
flexible, reusable layout components.  

---

## Component with Render Props Pattern

Render props share code between components using a prop whose value is a  
function.  

```jsx
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (e) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    window.addEventListener('mousemove', handleMouseMove);
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return render(position);
}

function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <p>Mouse position: {x}, {y}</p>
      )}
    />
  );
}

export default MouseTracker;
```

Render props let you share logic between components by passing a function  
that returns React elements. The component calls this function with data,  
and the function determines what to render. This pattern has been largely  
replaced by custom hooks but is still useful in some scenarios.  

---

## Higher-Order Component Pattern

Higher-Order Components (HOC) wrap components to add functionality.  

```jsx
function withLoading(Component) {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    }
    return <Component {...props} />;
  };
}

function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

const UserListWithLoading = withLoading(UserList);

// Usage: <UserListWithLoading isLoading={false} users={[...]} />

export default UserListWithLoading;
```

HOCs are functions that take a component and return a new component with  
additional props or behavior. They enable code reuse and cross-cutting  
concerns. While custom hooks are now preferred for logic reuse, HOCs are  
still useful for wrapping components with additional rendering logic.  

---

## Component with Lazy Loading

Lazy loading components improves initial load performance.  

```jsx
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<div>Loading component...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```

`React.lazy` enables code splitting by loading components only when needed.  
The `import()` function returns a Promise that resolves to a module. Wrap  
lazy components in `<Suspense>` to show a fallback while loading. This  
significantly reduces initial bundle size for large applications.  

---

## Component with Error Boundary

Error boundaries catch JavaScript errors in child components.  

```jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
        </div>
      );
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

Error boundaries must be class components (no functional equivalent yet).  
They catch errors during rendering, in lifecycle methods, and in  
constructors of child components. Use them to prevent entire app crashes  
and provide graceful error handling. Wrap parts of your tree in boundaries.  

---

## Component with Portal

Portals render children into a different DOM node.  

```jsx
import { createPortal } from 'react-dom';

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={(e) => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.body
  );
}

export default Modal;
```

Portals render components outside their parent DOM hierarchy while  
maintaining React's component tree. This is perfect for modals, tooltips,  
and dropdowns that need to break out of parent overflow/z-index  
constraints. Events still bubble through the React tree normally.  

---

## Component with Local Storage Sync

Syncing state with localStorage persists data across sessions.  

```jsx
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

function ThemeSelector() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');

  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

export default ThemeSelector;
```

This custom hook synchronizes state with localStorage, persisting data  
across page reloads. The lazy initializer function (first useState  
argument) runs only once, reading from localStorage on mount. The effect  
saves to localStorage whenever the value changes. Perfect for user  
preferences.  

---

## Advanced Component with Debouncing

Debouncing delays function execution until after user stops typing.  

```jsx
import { useState, useEffect, useCallback } from 'react';

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

function SearchBox() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  useEffect(() => {
    if (debouncedSearchTerm) {
      console.log('Searching for:', debouncedSearchTerm);
      // Perform API call here
    }
  }, [debouncedSearchTerm]);

  return (
    <input
      type="text"
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Search..."
    />
  );
}

export default SearchBox;
```

Debouncing prevents excessive function calls during rapid user input. This  
custom hook delays updating the debounced value until the user stops typing  
for the specified delay. The cleanup function cancels the previous timeout  
on each keystroke. Essential for search inputs that trigger API calls.  

---

## Component with Memoization for Performance

React.memo prevents unnecessary re-renders of functional components.  

```jsx
import { memo } from 'react';

const ExpensiveListItem = memo(({ item, onSelect }) => {
  console.log('Rendering item:', item.id);
  return (
    <div onClick={() => onSelect(item.id)}>
      <h4>{item.title}</h4>
      <p>{item.description}</p>
    </div>
  );
});

function OptimizedList({ items, onSelectItem }) {
  return (
    <div>
      {items.map(item => (
        <ExpensiveListItem
          key={item.id}
          item={item}
          onSelect={onSelectItem}
        />
      ))}
    </div>
  );
}

export default OptimizedList;
```

`React.memo` is a higher-order component that memoizes the result. It only  
re-renders if props change (shallow comparison). Use it for components that  
render often with the same props. Combine with `useCallback` for function  
props to maximize effectiveness. Don't over-optimize; measure first.  

---

## Best Practices and Modern Patterns

Summary of functional component best practices.  

```jsx
import { useState, useEffect, useCallback, memo } from 'react';

// 1. Named exports for better tree-shaking
export const UserDashboard = memo(({ userId }) => {
  // 2. Group related state
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  // 3. Memoize callbacks passed to children
  const handleRefresh = useCallback(() => {
    setLoading(true);
    // fetch user data
  }, []);

  // 4. Clear dependencies in useEffect
  useEffect(() => {
    let cancelled = false;

    async function loadUser() {
      const data = await fetchUser(userId);
      if (!cancelled) {
        setUser(data);
        setLoading(false);
      }
    }

    loadUser();
    
    // 5. Cleanup to prevent memory leaks
    return () => {
      cancelled = true;
    };
  }, [userId]);

  // 6. Early returns for different states
  if (loading) return <LoadingSpinner />;
  if (!user) return <ErrorMessage />;

  // 7. Main render
  return (
    <div className="dashboard">
      <UserProfile user={user} />
      <button onClick={handleRefresh}>Refresh</button>
    </div>
  );
});

// 8. PropTypes or TypeScript for type safety (if using PropTypes)
UserDashboard.displayName = 'UserDashboard';
```

Modern React development follows these patterns: use functional components  
with hooks, memoize expensive operations, handle cleanup properly, provide  
early returns for different states, and use TypeScript or PropTypes for  
type safety. Keep components focused and extract logic into custom hooks.  
Always consider performance but don't premature optimize.
