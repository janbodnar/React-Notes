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

```tsx
import React from 'react';

type GreetingProps = {
  name: string;
};

function Greeting({ name }: GreetingProps): JSX.Element {
  return <h1>Hello there, {name}!</h1>;
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type UserCardProps = {
  name: string;
  age: number;
  email: string;
};

function UserCard({ name, age, email }: UserCardProps): JSX.Element {
  return (
    <div className="user-card">
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type Product = {
  name: string;
  price: number;
  category: string;
};

type ProductCardProps = {
  product: Product;
};

function ProductCard({ product }: ProductCardProps): JSX.Element {
  return (
    <div className="product">
      <h3>{product.name}</h3>
      <p>Price: ${product.price}</p>
      <p>Category: {product.category}</p>
    </div>
  );
}

function App(): JSX.Element {
  const product: Product = {
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

```tsx
import React from 'react';

type TodoListProps = {
  tasks: string[];
};

function TodoList({ tasks }: TodoListProps): JSX.Element {
  return (
    <ul>
      {tasks.map((task, index) => (
        <li key={index}>{task}</li>
      ))}
    </ul>
  );
}

function App(): JSX.Element {
  const myTasks: string[] = ["Learn React", "Build a project", "Deploy app"];

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

```tsx
import React from 'react';

type AlertProps = {
  message: string;
  isError: boolean;
};

function Alert({ message, isError }: AlertProps): JSX.Element {
  const className = isError ? "alert-error" : "alert-success";
  
  return (
    <div className={className}>
      {message}
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type ButtonProps = {
  text?: string;
  variant?: 'primary' | 'secondary' | 'success';
};

function Button({ text = "Click me", variant = "primary" }: ButtonProps): JSX.Element {
  return (
    <button className={`btn btn-${variant}`}>
      {text}
    </button>
  );
}

function App(): JSX.Element {
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

```tsx
import React, { InputHTMLAttributes } from 'react';

type InputProps = {
  label: string;
} & InputHTMLAttributes<HTMLInputElement>;

function Input({ label, ...inputProps }: InputProps): JSX.Element {
  return (
    <div className="form-group">
      <label>{label}</label>
      <input {...inputProps} />
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type CounterProps = {
  count: number;
  onIncrement: () => void;
  onDecrement: () => void;
};

function Counter({ count, onIncrement, onDecrement }: CounterProps): JSX.Element {
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={onIncrement}>+</button>
      <button onClick={onDecrement}>-</button>
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React, { ReactNode } from 'react';

type CardProps = {
  children: ReactNode;
  title: string;
};

function Card({ children, title }: CardProps): JSX.Element {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type UserStatusProps = {
  isLoggedIn: boolean;
  username?: string;
};

function UserStatus({ isLoggedIn, username }: UserStatusProps): JSX.Element {
  if (isLoggedIn) {
    return <p>Welcome back, {username}!</p>;
  }
  return <p>Please log in to continue.</p>;
}

function App(): JSX.Element {
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

```tsx
import React from 'react';

type ArticleProps = {
  title: string;
  content: string;
  publishDate: Date;
};

function Article({ title, content, publishDate }: ArticleProps): JSX.Element {
  return (
    <article>
      <h1>{title}</h1>
      <time>{publishDate.toLocaleDateString()}</time>
      <p>{content}</p>
    </article>
  );
}

export default Article;
```

PropTypes provide runtime validation of props. They warn you in development  
if props don't match expected types, helping prevent bugs. Note: You need  
to install the `prop-types` package.

---

### Spread Props Pattern

The spread operator can pass all object properties as individual props.

```tsx
import React from 'react';

type ProfileProps = {
  name: string;
  age: number;
  city: string;
  occupation: string;
};

function Profile({ name, age, city, occupation }: ProfileProps): JSX.Element {
  return (
    <div>
      <h2>{name}</h2>
      <p>{age} years old</p>
      <p>Lives in {city}</p>
      <p>Works as {occupation}</p>
    </div>
  );
}

function App(): JSX.Element {
  const user: ProfileProps = {
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

```tsx
import React, { ReactNode } from 'react';

type DataProviderProps<T> = {
  data: T[];
  render: (data: T[]) => ReactNode;
};

function DataProvider<T>({ data, render }: DataProviderProps<T>): JSX.Element {
  return <div>{render(data)}</div>;
}

function App(): JSX.Element {
  const users: string[] = ["Alice", "Bob", "Charlie"];

  return (
    <DataProvider
      data={users}
      render={(data) => (
        <ul>
          {data.map((user, i) => <li key={i}>{user}</li>)}
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

```tsx
import React, { ReactNode } from 'react';

type LayoutProps = {
  header: ReactNode;
  sidebar: ReactNode;
  content: ReactNode;
};

function Layout({ header, sidebar, content }: LayoutProps): JSX.Element {
  return (
    <div className="layout">
      <header>{header}</header>
      <aside>{sidebar}</aside>
      <main>{content}</main>
    </div>
  );
}

function App(): JSX.Element {
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

```tsx
import React, { forwardRef, useRef } from 'react';

type CustomInputProps = {
  label: string;
};

const CustomInput = forwardRef<HTMLInputElement, CustomInputProps>(({ label, ...props }, ref) => {
  return (
    <div>
      <label>{label}</label>
      <input ref={ref} {...props} />
    </div>
  );
});

function App(): JSX.Element {
  const inputRef = useRef<HTMLInputElement>(null);

  const focusInput = (): void => {
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

```tsx
import React, { useState } from 'react';

function Counter(): JSX.Element {
  const [count, setCount] = useState<number>(0);

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

```tsx
import React, { useState } from 'react';

function ToggleVisibility(): JSX.Element {
  const [isVisible, setIsVisible] = useState<boolean>(false);

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

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

function NameForm(): JSX.Element {
  const [name, setName] = useState<string>('');

  const handleSubmit = (e: FormEvent<HTMLFormElement>): void => {
    e.preventDefault();
    alert(`Submitted name: ${name}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text"
        value={name}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setName(e.target.value)}
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

```tsx
import React, { useState, ChangeEvent } from 'react';

function RegistrationForm(): JSX.Element {
  const [username, setUsername] = useState<string>('');
  const [email, setEmail] = useState<string>('');
  const [age, setAge] = useState<number>(0);

  return (
    <div>
      <input 
        value={username}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setUsername(e.target.value)}
        placeholder="Username"
      />
      <input 
        value={email}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input 
        type="number"
        value={age}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setAge(Number(e.target.value))}
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

```tsx
import React, { useState } from 'react';

function AsyncCounter(): JSX.Element {
  const [count, setCount] = useState<number>(0);

  const incrementTwice = (): void => {
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

```tsx
import React, { useState } from 'react';

type User = {
  name: string;
  age: number;
  city: string;
};

function UserProfile(): JSX.Element {
  const [user, setUser] = useState<User>({
    name: 'Alice',
    age: 25,
    city: 'New York'
  });

  const updateCity = (): void => {
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

```tsx
import React, { useState, ChangeEvent } from 'react';

function TodoApp(): JSX.Element {
  const [todos, setTodos] = useState<string[]>(['Learn React', 'Build app']);
  const [input, setInput] = useState<string>('');

  const addTodo = (): void => {
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
        onChange={(e: ChangeEvent<HTMLInputElement>) => setInput(e.target.value)}
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

```tsx
import React, { useState } from 'react';

type Task = {
  id: number;
  text: string;
};

function TaskList(): JSX.Element {
  const [tasks, setTasks] = useState<Task[]>([
    { id: 1, text: 'Task 1' },
    { id: 2, text: 'Task 2' },
    { id: 3, text: 'Task 3' }
  ]);

  const removeTask = (id: number): void => {
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

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  name: string;
  bought: boolean;
};

function ShoppingList(): JSX.Element {
  const [items, setItems] = useState<Item[]>([
    { id: 1, name: 'Apples', bought: false },
    { id: 2, name: 'Bread', bought: false }
  ]);

  const toggleBought = (id: number): void => {
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

```tsx
import React, { useState, ChangeEvent } from 'react';

function PriceCalculator(): JSX.Element {
  const [price, setPrice] = useState<number>(100);
  const [quantity, setQuantity] = useState<number>(1);
  
  const total = price * quantity;
  const tax = total * 0.1;
  const finalPrice = total + tax;

  return (
    <div>
      <input 
        type="number"
        value={price}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setPrice(Number(e.target.value))}
      />
      <input 
        type="number"
        value={quantity}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setQuantity(Number(e.target.value))}
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

```tsx
import React, { useState } from 'react';

type SearchBoxProps = {
  value: string;
  onChange: (value: string) => void;
};

function SearchBox({ value, onChange }: SearchBoxProps): JSX.Element {
  return (
    <input 
      type="text"
      value={value}
      onChange={(e) => onChange(e.target.value)}
      placeholder="Search..."
    />
  );
}

function App(): JSX.Element {
  const [searchTerm, setSearchTerm] = useState<string>('');

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

```tsx
import React, { useState, ChangeEvent } from 'react';

type TemperatureInputProps = {
  scale: 'Celsius' | 'Fahrenheit';
  temperature: string;
  onChange: (value: string) => void;
};

function TemperatureInput({ scale, temperature, onChange }: TemperatureInputProps): JSX.Element {
  return (
    <div>
      <label>Enter temperature in {scale}:</label>
      <input 
        value={temperature}
        onChange={(e: ChangeEvent<HTMLInputElement>) => onChange(e.target.value)}
      />
    </div>
  );
}

function App(): JSX.Element {
  const [celsius, setCelsius] = useState<string>('');
  const fahrenheit = celsius ? (parseFloat(celsius) * 9/5 + 32).toFixed(1) : '';

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

```tsx
import React, { useState, ChangeEvent } from 'react';

type EditableTitleProps = {
  initialTitle: string;
};

function EditableTitle({ initialTitle }: EditableTitleProps): JSX.Element {
  const [title, setTitle] = useState<string>(initialTitle);
  const [isEditing, setIsEditing] = useState<boolean>(false);

  if (isEditing) {
    return (
      <input 
        value={title}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setTitle(e.target.value)}
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

function App(): JSX.Element {
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

```tsx
import React, { useState } from 'react';

type ColorPickerProps = {
  onColorChange: (color: string) => void;
};

function ColorPicker({ onColorChange }: ColorPickerProps): JSX.Element {
  const colors: string[] = ['red', 'blue', 'green', 'yellow'];

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

function App(): JSX.Element {
  const [selectedColor, setSelectedColor] = useState<string>('red');

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

```tsx
import React, { useState, Dispatch, SetStateAction } from 'react';

type Theme = 'light' | 'dark';

type DisplayProps = {
  theme: Theme;
};

function Display({ theme }: DisplayProps): JSX.Element {
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

type PanelProps = {
  theme: Theme;
  onThemeChange: Dispatch<SetStateAction<Theme>>;
};

function Panel({ theme, onThemeChange }: PanelProps): JSX.Element {
  return (
    <div>
      <button onClick={() => onThemeChange('dark')}>Dark</button>
      <button onClick={() => onThemeChange('light')}>Light</button>
      <Display theme={theme} />
    </div>
  );
}

function App(): JSX.Element {
  const [theme, setTheme] = useState<Theme>('light');

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

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type Credentials = {
  email: string;
  password: string;
};

type LoginFormProps = {
  onSubmit: (credentials: Credentials) => void;
};

function LoginForm({ onSubmit }: LoginFormProps): JSX.Element {
  const [email, setEmail] = useState<string>('');
  const [password, setPassword] = useState<string>('');
  const [error, setError] = useState<string>('');

  const handleSubmit = (e: FormEvent<HTMLFormElement>): void => {
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
        onChange={(e: ChangeEvent<HTMLInputElement>) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input 
        type="password"
        value={password}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setPassword(e.target.value)}
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}

function App(): JSX.Element {
  const handleLogin = (credentials: Credentials): void => {
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
