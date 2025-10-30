# React Props and State

## Introduction

**Props** (short for properties) and **State** are two fundamental concepts  
in React that control how data flows through your application and how  
components render and update.

**Props** are read-only data passed from parent components to child  
components. They allow you to create reusable components by providing  
different data to the same component structure. Props are immutable within  
the receiving component, ensuring a unidirectional data flow.

**State** is mutable data managed within a component. When state changes,  
React automatically re-renders the component to reflect the new data. State  
is managed using the `useState` Hook in functional components, which is the  
modern React approach.

Understanding props and state is crucial for building interactive React  
applications. Props enable component composition and reusability, while  
state enables interactivity and dynamic behavior.

---

## Basic Props Usage

### Passing Simple Props

The most basic use of props involves passing primitive values like strings,  
numbers, or booleans from a parent component to a child component.

```jsx
function Greeting({ name }) {
  return <h1>Hello there, {name}!</h1>;
}

function App() {
  return <Greeting name="Alice" />;
}

export default App;
```

In this example, the `Greeting` component receives a `name` prop from its  
parent `App` component. The prop is destructured in the function parameters  
for cleaner code. When rendered, it displays "Hello there, Alice!".

---

### Multiple Props

Components can receive multiple props of different types to build more  
complex interfaces.

```jsx
function UserCard({ name, age, email }) {
  return (
    <div className="user-card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

function App() {
  return (
    <UserCard 
      name="Bob Smith" 
      age={28} 
      email="bob@example.com" 
    />
  );
}

export default App;
```

This example demonstrates passing multiple props including strings and  
numbers. The curly braces around `{28}` indicate it's a JavaScript  
expression rather than a string.

---

### Props with Objects

You can pass entire objects as props to send complex data structures to  
child components.

```jsx
function ProductCard({ product }) {
  return (
    <div className="product">
      <h3>{product.name}</h3>
      <p>Price: ${product.price}</p>
      <p>Category: {product.category}</p>
    </div>
  );
}

function App() {
  const product = {
    name: "Laptop",
    price: 999,
    category: "Electronics"
  };

  return <ProductCard product={product} />;
}

export default App;
```

Passing objects as props is useful when you have related data that belongs  
together. The child component accesses object properties using dot notation.

---

### Props with Arrays

Arrays can be passed as props and mapped to create lists of elements.

```jsx
function TodoList({ tasks }) {
  return (
    <ul>
      {tasks.map((task, index) => (
        <li key={index}>{task}</li>
      ))}
    </ul>
  );
}

function App() {
  const myTasks = ["Learn React", "Build a project", "Deploy app"];

  return <TodoList tasks={myTasks} />;
}

export default App;
```

When rendering lists from arrays, React requires a `key` prop to efficiently  
track elements. Here we use the array index, though in production you'd  
typically use unique IDs.

---

### Boolean Props

Boolean props are commonly used to conditionally render or style components.

```jsx
function Alert({ message, isError }) {
  const className = isError ? "alert-error" : "alert-success";
  
  return (
    <div className={className}>
      {message}
    </div>
  );
}

function App() {
  return (
    <div>
      <Alert message="Operation successful!" isError={false} />
      <Alert message="Something went wrong!" isError={true} />
    </div>
  );
}

export default App;
```

Boolean props control component behavior. This example uses the `isError`  
prop to determine which CSS class to apply, creating different visual styles.

---

## Intermediate Props Patterns

### Default Props with Destructuring

You can provide default values for props using JavaScript's default  
parameter syntax in destructuring.

```jsx
function Button({ text = "Click me", variant = "primary" }) {
  return (
    <button className={`btn btn-${variant}`}>
      {text}
    </button>
  );
}

function App() {
  return (
    <div>
      <Button />
      <Button text="Submit" variant="success" />
    </div>
  );
}

export default App;
```

Default props ensure components work even when certain props aren't  
provided. The first button uses all defaults, while the second overrides  
them.

---

### Props Destructuring with Rest Operator

The rest operator allows you to extract specific props while passing  
remaining props to child elements.

```jsx
function Input({ label, ...inputProps }) {
  return (
    <div className="form-group">
      <label>{label}</label>
      <input {...inputProps} />
    </div>
  );
}

function App() {
  return (
    <Input 
      label="Username" 
      type="text" 
      placeholder="Enter username"
      required
    />
  );
}

export default App;
```

This pattern is useful for wrapper components. The `label` prop is used by  
the wrapper, while `type`, `placeholder`, and `required` are passed directly  
to the input element.

---

### Function Props (Callbacks)

Props can be functions, enabling child components to communicate back to  
parent components.

```jsx
function Counter({ count, onIncrement, onDecrement }) {
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={onIncrement}>+</button>
      <button onClick={onDecrement}>-</button>
    </div>
  );
}

function App() {
  const handleIncrement = () => console.log("Increment!");
  const handleDecrement = () => console.log("Decrement!");

  return (
    <Counter 
      count={0}
      onIncrement={handleIncrement}
      onDecrement={handleDecrement}
    />
  );
}

export default App;
```

Function props (callbacks) are crucial for handling events. The child  
component calls these functions, allowing the parent to respond to user  
interactions.

---

### Children Props

The special `children` prop contains any content placed between component  
opening and closing tags.

```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

function App() {
  return (
    <Card title="Welcome">
      <p>This is card content.</p>
      <button>Learn More</button>
    </Card>
  );
}

export default App;
```

The `children` prop makes components flexible containers. Anything between  
`<Card>` and `</Card>` becomes the `children` prop, allowing dynamic content  
composition.

---

### Conditional Rendering with Props

Props can control what content is rendered within a component.

```jsx
function UserStatus({ isLoggedIn, username }) {
  if (isLoggedIn) {
    return <p>Welcome back, {username}!</p>;
  }
  return <p>Please log in to continue.</p>;
}

function App() {
  return (
    <div>
      <UserStatus isLoggedIn={true} username="Alice" />
      <UserStatus isLoggedIn={false} />
    </div>
  );
}

export default App;
```

Conditional rendering based on props allows components to display different  
content based on the application state. This pattern is fundamental for  
building dynamic UIs.

---

## Advanced Props Techniques

### Props Validation with PropTypes

PropTypes help catch bugs by validating the types of props a component  
receives.

```jsx
import PropTypes from 'prop-types';

function Article({ title, content, publishDate }) {
  return (
    <article>
      <h1>{title}</h1>
      <time>{publishDate.toLocaleDateString()}</time>
      <p>{content}</p>
    </article>
  );
}

Article.propTypes = {
  title: PropTypes.string.isRequired,
  content: PropTypes.string.isRequired,
  publishDate: PropTypes.instanceOf(Date).isRequired
};

export default Article;
```

PropTypes provide runtime validation of props. They warn you in development  
if props don't match expected types, helping prevent bugs. Note: You need  
to install the `prop-types` package.

---

### Spread Props Pattern

The spread operator can pass all object properties as individual props.

```jsx
function Profile({ name, age, city, occupation }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{age} years old</p>
      <p>Lives in {city}</p>
      <p>Works as {occupation}</p>
    </div>
  );
}

function App() {
  const user = {
    name: "Charlie",
    age: 32,
    city: "Boston",
    occupation: "Developer"
  };

  return <Profile {...user} />;
}

export default App;
```

Spread props are convenient when an object's structure matches the  
component's props. This reduces repetition and makes code more maintainable.

---

### Render Props Pattern

A render prop is a function prop that a component uses to determine what  
to render.

```jsx
function DataProvider({ data, render }) {
  return <div>{render(data)}</div>;
}

function App() {
  const users = ["Alice", "Bob", "Charlie"];

  return (
    <DataProvider 
      data={users}
      render={(users) => (
        <ul>
          {users.map((user, i) => <li key={i}>{user}</li>)}
        </ul>
      )}
    />
  );
}

export default App;
```

Render props provide a way to share code between components using a  
function prop. The parent component controls what gets rendered while the  
child component provides the data.

---

### Component Composition with Props

Components can be passed as props to create flexible, composable layouts.

```jsx
function Layout({ header, sidebar, content }) {
  return (
    <div className="layout">
      <header>{header}</header>
      <aside>{sidebar}</aside>
      <main>{content}</main>
    </div>
  );
}

function App() {
  return (
    <Layout
      header={<h1>My App</h1>}
      sidebar={<nav>Navigation</nav>}
      content={<p>Main content here</p>}
    />
  );
}

export default App;
```

Passing components as props creates highly flexible layouts. Each section  
can be independently defined and swapped without changing the layout  
structure.

---

### Props Forwarding with forwardRef

Sometimes you need to pass refs through component boundaries, which  
requires `forwardRef`.

```jsx
import { forwardRef } from 'react';

const CustomInput = forwardRef(({ label, ...props }, ref) => {
  return (
    <div>
      <label>{label}</label>
      <input ref={ref} {...props} />
    </div>
  );
});

function App() {
  const inputRef = React.useRef(null);

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <div>
      <CustomInput label="Name" ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

export default App;
```

`forwardRef` allows parent components to access DOM elements inside child  
components. This is essential for form controls and other interactive  
elements.

---

## Basic State with useState

### Simple Counter State

The `useState` Hook manages component state in functional components.

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </div>
  );
}

export default Counter;
```

`useState` returns an array with two elements: the current state value and  
a function to update it. The argument to `useState` is the initial state  
value.

---

### Toggle State

Boolean state is perfect for toggling between two states like show/hide or  
on/off.

```jsx
import { useState } from 'react';

function ToggleVisibility() {
  const [isVisible, setIsVisible] = useState(false);

  return (
    <div>
      <button onClick={() => setIsVisible(!isVisible)}>
        {isVisible ? 'Hide' : 'Show'} Content
      </button>
      {isVisible && <p>This content can be toggled!</p>}
    </div>
  );
}

export default ToggleVisibility;
```

This pattern uses the logical NOT operator (`!`) to flip the boolean state.  
The content is conditionally rendered using the `&&` operator.

---

### Input State Management

State is commonly used to control form inputs, making them "controlled  
components".

```jsx
import { useState } from 'react';

function NameForm() {
  const [name, setName] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`Submitted name: ${name}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter your name"
      />
      <button type="submit">Submit</button>
    </form>
  );
}

export default NameForm;
```

Controlled components keep React state as the "single source of truth" for  
input values. The `value` prop sets the display, while `onChange` updates  
the state.

---

### Multiple State Variables

Components can use multiple `useState` calls to manage different pieces of  
state independently.

```jsx
import { useState } from 'react';

function RegistrationForm() {
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);

  return (
    <div>
      <input 
        value={username}
        onChange={(e) => setUsername(e.target.value)}
        placeholder="Username"
      />
      <input 
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input 
        type="number"
        value={age}
        onChange={(e) => setAge(Number(e.target.value))}
        placeholder="Age"
      />
      <p>Username: {username}, Email: {email}, Age: {age}</p>
    </div>
  );
}

export default RegistrationForm;
```

Using separate state variables for unrelated data makes state updates  
simpler and more explicit. Each field has its own setter function.

---

### Functional State Updates

When new state depends on previous state, use the functional update form  
to ensure correctness.

```jsx
import { useState } from 'react';

function AsyncCounter() {
  const [count, setCount] = useState(0);

  const incrementTwice = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementTwice}>+2</button>
    </div>
  );
}

export default AsyncCounter;
```

The functional form `setCount(prevCount => prevCount + 1)` ensures you're  
working with the most current state value, preventing bugs when multiple  
updates occur.

---

## Intermediate State Management

### Object State

State can hold objects, but you must create new objects when updating to  
trigger re-renders.

```jsx
import { useState } from 'react';

function UserProfile() {
  const [user, setUser] = useState({
    name: 'Alice',
    age: 25,
    city: 'New York'
  });

  const updateCity = () => {
    setUser(prevUser => ({
      ...prevUser,
      city: 'San Francisco'
    }));
  };

  return (
    <div>
      <p>{user.name}, {user.age}, {user.city}</p>
      <button onClick={updateCity}>Move to SF</button>
    </div>
  );
}

export default UserProfile;
```

When updating object state, use the spread operator to copy existing  
properties and only override the ones you want to change. This preserves  
immutability.

---

### Array State - Adding Items

Arrays in state should be updated immutably using methods like spread or  
concat rather than push.

```jsx
import { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState(['Learn React', 'Build app']);
  const [input, setInput] = useState('');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, input]);
      setInput('');
    }
  };

  return (
    <div>
      <ul>
        {todos.map((todo, i) => <li key={i}>{todo}</li>)}
      </ul>
      <input 
        value={input}
        onChange={(e) => setInput(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>
    </div>
  );
}

export default TodoApp;
```

The spread operator `[...todos, input]` creates a new array with existing  
todos plus the new item. This immutable update pattern ensures React  
detects the change.

---

### Array State - Removing Items

Filter creates a new array without the removed item, maintaining  
immutability.

```jsx
import { useState } from 'react';

function TaskList() {
  const [tasks, setTasks] = useState([
    { id: 1, text: 'Task 1' },
    { id: 2, text: 'Task 2' },
    { id: 3, text: 'Task 3' }
  ]);

  const removeTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  return (
    <ul>
      {tasks.map(task => (
        <li key={task.id}>
          {task.text}
          <button onClick={() => removeTask(task.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}

export default TaskList;
```

The `filter` method creates a new array excluding the item with the  
specified ID. This is the standard pattern for removing items from state  
arrays.

---

### Array State - Updating Items

Map creates a new array with specific items updated based on a condition.

```jsx
import { useState } from 'react';

function ShoppingList() {
  const [items, setItems] = useState([
    { id: 1, name: 'Apples', bought: false },
    { id: 2, name: 'Bread', bought: false }
  ]);

  const toggleBought = (id) => {
    setItems(items.map(item => 
      item.id === id ? { ...item, bought: !item.bought } : item
    ));
  };

  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          <span style={{ 
            textDecoration: item.bought ? 'line-through' : 'none' 
          }}>
            {item.name}
          </span>
          <button onClick={() => toggleBought(item.id)}>Toggle</button>
        </li>
      ))}
    </ul>
  );
}

export default ShoppingList;
```

The `map` method creates a new array where matching items are updated  
with new object references, while other items remain unchanged.

---

### Derived State

Sometimes you can calculate values from existing state instead of storing  
them separately.

```jsx
import { useState } from 'react';

function PriceCalculator() {
  const [price, setPrice] = useState(100);
  const [quantity, setQuantity] = useState(1);
  
  // Derived value, not stored in state
  const total = price * quantity;
  const tax = total * 0.1;
  const finalPrice = total + tax;

  return (
    <div>
      <input 
        type="number"
        value={price}
        onChange={(e) => setPrice(Number(e.target.value))}
      />
      <input 
        type="number"
        value={quantity}
        onChange={(e) => setQuantity(Number(e.target.value))}
      />
      <p>Total: ${finalPrice.toFixed(2)}</p>
    </div>
  );
}

export default PriceCalculator;
```

Derived state is calculated from existing state during render. This avoids  
sync issues and reduces complexity compared to storing calculated values  
in state.

---

## Combining Props and State

### Controlled Component with Props

Parents can control child component state through props and callbacks.

```jsx
import { useState } from 'react';

function SearchBox({ value, onChange }) {
  return (
    <input 
      type="text"
      value={value}
      onChange={(e) => onChange(e.target.value)}
      placeholder="Search..."
    />
  );
}

function App() {
  const [searchTerm, setSearchTerm] = useState('');

  return (
    <div>
      <SearchBox value={searchTerm} onChange={setSearchTerm} />
      <p>Searching for: {searchTerm}</p>
    </div>
  );
}

export default App;
```

The parent component controls the state, while the child component handles  
the UI. This pattern enables sharing state between multiple components.

---

### Lifting State Up

When multiple components need to share state, lift it up to their common  
parent.

```jsx
import { useState } from 'react';

function TemperatureInput({ scale, temperature, onChange }) {
  return (
    <div>
      <label>Enter temperature in {scale}:</label>
      <input 
        value={temperature}
        onChange={(e) => onChange(e.target.value)}
      />
    </div>
  );
}

function App() {
  const [celsius, setCelsius] = useState('');
  const fahrenheit = celsius ? (celsius * 9/5 + 32).toFixed(1) : '';

  return (
    <div>
      <TemperatureInput 
        scale="Celsius"
        temperature={celsius}
        onChange={setCelsius}
      />
      <p>Fahrenheit: {fahrenheit}</p>
    </div>
  );
}

export default App;
```

Lifting state up allows you to synchronize components. The parent maintains  
the single source of truth and passes data down via props.

---

### Props with Initial State

Props can provide initial state values, but the component manages the  
state thereafter.

```jsx
import { useState } from 'react';

function EditableTitle({ initialTitle }) {
  const [title, setTitle] = useState(initialTitle);
  const [isEditing, setIsEditing] = useState(false);

  if (isEditing) {
    return (
      <input 
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        onBlur={() => setIsEditing(false)}
        autoFocus
      />
    );
  }

  return (
    <h2 onClick={() => setIsEditing(true)}>
      {title}
    </h2>
  );
}

function App() {
  return <EditableTitle initialTitle="Click to edit" />;
}

export default App;
```

Using props for initial state is common for components that become  
independent after initialization. The component's internal state manages  
subsequent changes.

---

### Callback Props for State Updates

Child components can call parent functions to trigger state changes in the  
parent.

```jsx
import { useState } from 'react';

function ColorPicker({ onColorChange }) {
  const colors = ['red', 'blue', 'green', 'yellow'];

  return (
    <div>
      {colors.map(color => (
        <button 
          key={color}
          onClick={() => onColorChange(color)}
          style={{ backgroundColor: color }}
        >
          {color}
        </button>
      ))}
    </div>
  );
}

function App() {
  const [selectedColor, setSelectedColor] = useState('red');

  return (
    <div>
      <ColorPicker onColorChange={setSelectedColor} />
      <div style={{ 
        width: 100, 
        height: 100, 
        backgroundColor: selectedColor 
      }} />
    </div>
  );
}

export default App;
```

Callback props enable child-to-parent communication. The child calls the  
function with data, and the parent updates its state accordingly.

---

### Props Drilling Solution

When props need to pass through many levels, consider component composition  
or Context API.

```jsx
import { useState } from 'react';

function Display({ theme }) {
  return (
    <div style={{ 
      backgroundColor: theme === 'dark' ? '#333' : '#fff',
      color: theme === 'dark' ? '#fff' : '#333',
      padding: '20px'
    }}>
      Current theme: {theme}
    </div>
  );
}

function Panel({ theme, onThemeChange }) {
  return (
    <div>
      <button onClick={() => onThemeChange('dark')}>Dark</button>
      <button onClick={() => onThemeChange('light')}>Light</button>
      <Display theme={theme} />
    </div>
  );
}

function App() {
  const [theme, setTheme] = useState('light');

  return <Panel theme={theme} onThemeChange={setTheme} />;
}

export default App;
```

This example shows prop drilling where theme passes through Panel to  
Display. For deeper nesting, Context API would be more appropriate.

---

### Form State Management

Complex forms combine multiple state variables and props for validation  
and submission.

```jsx
import { useState } from 'react';

function LoginForm({ onSubmit }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (!email || !password) {
      setError('All fields are required');
      return;
    }
    
    setError('');
    onSubmit({ email, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      {error && <p style={{ color: 'red' }}>{error}</p>}
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
      <button type="submit">Login</button>
    </form>
  );
}

function App() {
  const handleLogin = (credentials) => {
    console.log('Login attempt:', credentials);
  };

  return <LoginForm onSubmit={handleLogin} />;
}

export default App;
```

This form manages multiple input states, validation error state, and uses  
a callback prop to communicate form data to the parent component.

---

## Best Practices

### Key Principles for Props

1. **Keep props immutable** - Never modify props directly within a  
   component
2. **Use descriptive names** - Prop names should clearly indicate their  
   purpose
3. **Validate with PropTypes** - Use PropTypes in development to catch  
   type errors early
4. **Avoid prop drilling** - Use Context API when props pass through many  
   levels
5. **Provide defaults** - Use default parameters for optional props

### Key Principles for State

1. **Minimize state** - Only store values that change and affect rendering
2. **Keep state local** - Don't lift state higher than necessary
3. **Use functional updates** - When new state depends on old state
4. **Maintain immutability** - Always create new objects/arrays when  
   updating
5. **Derive when possible** - Calculate values from state instead of  
   storing them

### Common Pitfalls to Avoid

- **Don't mutate state directly** - Always use the setter function
- **Don't use indexes as keys** - Use unique IDs for list items
- **Don't store props in state** - Unless you need a different initial  
  value
- **Don't forget dependencies** - In useEffect and other Hooks
- **Don't over-optimize** - Start simple, optimize when needed

---

## Summary

Props and state are the foundation of data management in React. Props  
enable component composition and reusability by passing data down the  
component tree. State enables interactivity by allowing components to  
manage and respond to changing data.

Use props for:
- Passing data from parent to child
- Configuring component behavior
- Callback functions for child-to-parent communication
- Component composition with children and render props

Use state for:
- Managing component-specific data
- Handling user input and interactions
- Tracking UI state like toggles and selections
- Managing temporary data during component lifecycle

Understanding when to use props versus state, and how to combine them  
effectively, is essential for building maintainable React applications.  
Start with simple patterns and gradually adopt more advanced techniques  
as your application grows in complexity.
