# React Basics

## Introduction

React is a JavaScript library for building user interfaces, developed and  
maintained by Facebook (Meta). It enables developers to create interactive,  
dynamic web applications by breaking down complex UIs into reusable,  
self-contained components.  

React follows a component-based architecture where each component manages its  
own state and can be composed together to build complex user interfaces. The  
library uses a virtual DOM to efficiently update and render components,  
ensuring optimal performance.  

**Key Concepts:**

**Components** are the building blocks of React applications. They are  
reusable pieces of UI that can be either functional (using functions) or  
class-based (using ES6 classes). Modern React primarily uses functional  
components with Hooks.  

**JSX** (JavaScript XML) is a syntax extension that allows you to write  
HTML-like code within JavaScript. It makes component structure more readable  
and intuitive.  

**Props** (properties) are read-only data passed from parent components to  
child components. They enable data flow and component communication.  

**State** is mutable data that belongs to a component. When state changes,  
React re-renders the component to reflect the new data.  

**Hooks** are special functions that let you use state and other React  
features in functional components. The most common hooks are `useState` for  
managing state and `useEffect` for handling side effects.  

**Side Effects** are operations that affect things outside the component's  
scope, such as data fetching, subscriptions, timers, or manual DOM  
manipulation. The `useEffect` hook manages these operations.  

This tutorial covers React basics through 50 progressive examples, starting  
with simple concepts and gradually introducing more complex patterns. All  
examples use React 19.2 with modern functional components and hooks.  

---

## Hello Component

A basic functional component that renders a simple greeting.  

```jsx
function Hello() {
  return <h1>Hello there!</h1>;
}

export default Hello;
```

This is the simplest form of a React component. It's a JavaScript function  
that returns JSX. The component name must start with a capital letter, which  
distinguishes it from regular HTML elements. The `export default` statement  
makes the component available for import in other files.  

---

## Component with Expression

A component demonstrating how to use JavaScript expressions within JSX.  

```jsx
function Greeting() {
  const name = "React";
  const version = "19.2";
  
  return (
    <div>
      <h1>Welcome to {name}</h1>
      <p>Version: {version}</p>
    </div>
  );
}

export default Greeting;
```

JSX allows embedding JavaScript expressions using curly braces `{}`. You can  
include variables, calculations, function calls, or any valid JavaScript  
expression. The expressions are evaluated and their results are rendered in  
the UI.  

---

## Multiple Elements with Fragment

Returning multiple elements using React Fragment to avoid extra DOM nodes.  

```jsx
function UserCard() {
  return (
    <>
      <h2>John Smith</h2>
      <p>Software Developer</p>
      <p>Location: New York</p>
    </>
  );
}

export default UserCard;
```

React components must return a single root element. Using `<>` and `</>`  
(Fragment shorthand) allows you to group multiple elements without adding an  
extra `<div>` to the DOM. This is useful for maintaining clean HTML structure  
and avoiding unnecessary wrapper elements.  

---

## Component with Props

Passing data to components using props for reusability.  

```jsx
function Welcome({ name, role }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Role: {role}</p>
    </div>
  );
}

export default Welcome;
```

Props make components reusable by allowing parent components to pass data to  
children. Here, we use destructuring to extract `name` and `role` from the  
props object. The same component can be used multiple times with different  
data: `<Welcome name="Alice" role="Developer" />`.  

---

## Props with Default Values

Setting default prop values for components.  

```jsx
function Button({ label = "Click Me", variant = "primary" }) {
  return (
    <button className={`btn btn-${variant}`}>
      {label}
    </button>
  );
}

export default Button;
```

Default parameter values in JavaScript can be used to provide fallback values  
when props are not passed. If `label` or `variant` are not provided, the  
component uses "Click Me" and "primary" respectively. This makes components  
more robust and easier to use.  

---

## Props Children

Using the special `children` prop to pass content between component tags.  

```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-body">
        {children}
      </div>
    </div>
  );
}

export default Card;
```

The `children` prop is a special prop that contains the content passed  
between the opening and closing tags of a component. For example:  
`<Card title="Info"><p>Content here</p></Card>`. This pattern is useful for  
creating wrapper or container components.  

---

## Conditional Rendering with Ternary

Conditionally rendering content based on a condition.  

```jsx
function LoginStatus({ isLoggedIn, username }) {
  return (
    <div>
      {isLoggedIn ? (
        <p>Welcome back, {username}!</p>
      ) : (
        <p>Please log in to continue.</p>
      )}
    </div>
  );
}

export default LoginStatus;
```

The ternary operator `condition ? trueValue : falseValue` is commonly used  
for conditional rendering in JSX. When `isLoggedIn` is true, it shows a  
welcome message; otherwise, it prompts the user to log in. This is more  
concise than using if-else statements outside the JSX.  

---

## Conditional Rendering with AND

Using the logical AND operator for conditional rendering.  

```jsx
function Notification({ message, show }) {
  return (
    <div>
      {show && (
        <div className="notification">
          {message}
        </div>
      )}
    </div>
  );
}

export default Notification;
```

The `&&` operator provides a shorter syntax when you only need to render  
something if a condition is true. If `show` is true, the notification is  
rendered; if false, nothing is rendered. This is cleaner than a ternary  
operator when there's no else condition.  

---

## Rendering Lists

Rendering an array of items using the `map` function.  

```jsx
function FruitList() {
  const fruits = ["Apple", "Banana", "Orange", "Mango", "Grape"];
  
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}

export default FruitList;
```

The `map` function transforms each array element into a JSX element. Each  
item in the list requires a unique `key` prop to help React identify which  
items have changed, been added, or removed. While using array indices as keys  
works for static lists, it's better to use unique IDs for dynamic data.  

---

## Rendering Object Lists

Mapping over an array of objects to render complex list items.  

```jsx
function UserList() {
  const users = [
    { id: 1, name: "Alice Johnson", email: "alice@example.com" },
    { id: 2, name: "Bob Smith", email: "bob@example.com" },
    { id: 3, name: "Carol White", email: "carol@example.com" }
  ];
  
  return (
    <div>
      {users.map(user => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}

export default UserList;
```

When working with arrays of objects, use a unique property (like `id`) as the  
key instead of the array index. This ensures proper component identity and  
prevents rendering issues when the list changes. Each object property can be  
accessed using dot notation within the JSX.  

---

## Basic State with useState

Managing component state with the `useState` hook.  

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

The `useState` hook returns an array with two elements: the current state  
value and a function to update it. We use array destructuring to name these.  
The argument to `useState(0)` is the initial state value. When `setCount` is  
called, React re-renders the component with the new state.  

---

## Multiple State Variables

Using multiple `useState` hooks in a single component.  

```jsx
import { useState } from 'react';

function UserProfile() {
  const [name, setName] = useState("Guest");
  const [age, setAge] = useState(0);
  const [email, setEmail] = useState("");
  
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Email: {email}</p>
      <button onClick={() => setName("John Doe")}>Set Name</button>
      <button onClick={() => setAge(30)}>Set Age</button>
    </div>
  );
}

export default UserProfile;
```

You can use multiple `useState` hooks to manage different pieces of state  
independently. Each state variable has its own setter function. This approach  
is simpler than managing a single complex state object when the state pieces  
are unrelated.  

---

## State with Objects

Managing object state and updating it immutably.  

```jsx
import { useState } from 'react';

function PersonForm() {
  const [person, setPerson] = useState({
    firstName: "",
    lastName: "",
    age: 0
  });
  
  const updateFirstName = () => {
    setPerson({ ...person, firstName: "Alice" });
  };
  
  return (
    <div>
      <p>{person.firstName} {person.lastName}</p>
      <button onClick={updateFirstName}>Update Name</button>
    </div>
  );
}

export default PersonForm;
```

When state is an object, you must update it immutably using the spread  
operator `...`. This creates a new object with all previous properties plus  
the updated ones. React relies on object reference changes to detect state  
updates, so mutating the existing object won't trigger a re-render.  

---

## State with Arrays

Managing array state and adding items immutably.  

```jsx
import { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState(["Learn React", "Build a project"]);
  
  const addTodo = () => {
    setTodos([...todos, "New task"]);
  };
  
  return (
    <div>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
      <button onClick={addTodo}>Add Todo</button>
    </div>
  );
}

export default TodoList;
```

Arrays must also be updated immutably. The spread operator `[...todos, item]`  
creates a new array with all existing items plus the new one. Other immutable  
operations include `filter()` for removal and `map()` for updates. Never use  
methods like `push()`, `pop()`, or `splice()` directly on state arrays.  

---

## Toggle State

Creating a boolean toggle with state.  

```jsx
import { useState } from 'react';

function ToggleSwitch() {
  const [isOn, setIsOn] = useState(false);
  
  const toggle = () => {
    setIsOn(!isOn);
  };
  
  return (
    <div>
      <p>The switch is {isOn ? "ON" : "OFF"}</p>
      <button onClick={toggle}>Toggle</button>
    </div>
  );
}

export default ToggleSwitch;
```

Boolean state is perfect for toggle functionality. The `!` operator flips the  
boolean value. This pattern is common for showing/hiding elements, enabling/  
disabling features, or switching between two states. You could also use  
`setIsOn(prev => !prev)` to ensure you're working with the latest state.  

---

## Functional State Updates

Using functional updates to ensure state consistency.  

```jsx
import { useState } from 'react';

function AdvancedCounter() {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };
  
  const incrementMultiple = () => {
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
    setCount(prev => prev + 1);
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+1</button>
      <button onClick={incrementMultiple}>+3</button>
    </div>
  );
}

export default AdvancedCounter;
```

Functional updates `setCount(prev => prev + 1)` ensure you're working with  
the most current state value. This is crucial when multiple updates happen in  
quick succession or when the new state depends on the previous state. The  
function receives the previous state and returns the new state.  

---

## Input Handling

Handling user input with controlled components.  

```jsx
import { useState } from 'react';

function NameInput() {
  const [name, setName] = useState("");
  
  const handleChange = (event) => {
    setName(event.target.value);
  };
  
  return (
    <div>
      <input 
        type="text" 
        value={name} 
        onChange={handleChange}
        placeholder="Enter your name"
      />
      <p>Hello, {name}!</p>
    </div>
  );
}

export default NameInput;
```

Controlled components are inputs where React controls the value via state.  
The input's value is bound to state, and the `onChange` handler updates the  
state when the user types. This pattern gives you full control over the input  
and makes it easy to validate or transform user input.  

---

## Multiple Input Fields

Managing multiple controlled inputs in a form.  

```jsx
import { useState } from 'react';

function ContactForm() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
    message: ""
  });
  
  const handleChange = (event) => {
    const { name, value } = event.target;
    setFormData({ ...formData, [name]: value });
  };
  
  return (
    <form>
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
    </form>
  );
}

export default ContactForm;
```

A single `handleChange` function can manage multiple inputs using the input's  
`name` attribute. The computed property name `[name]` dynamically sets the  
correct field in the state object. This approach scales well for forms with  
many fields without creating separate handlers for each.  

---

## Form Submission

Handling form submission and preventing default behavior.  

```jsx
import { useState } from 'react';

function LoginForm() {
  const [credentials, setCredentials] = useState({
    username: "",
    password: ""
  });
  
  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Logging in with:", credentials);
    // Add authentication logic here
  };
  
  const handleChange = (event) => {
    const { name, value } = event.target;
    setCredentials({ ...credentials, [name]: value });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input 
        name="username"
        value={credentials.username}
        onChange={handleChange}
        placeholder="Username"
      />
      <input 
        name="password"
        type="password"
        value={credentials.password}
        onChange={handleChange}
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}

export default LoginForm;
```

The `onSubmit` event fires when a form is submitted. Always call  
`event.preventDefault()` to prevent the browser's default form submission  
(which would reload the page). After preventing default, you can handle the  
data as needed, such as sending it to an API or validating it.  

---

## Button Click Events

Handling click events with different patterns.  

```jsx
import { useState } from 'react';

function ClickHandler() {
  const [message, setMessage] = useState("");
  
  const handleClick = () => {
    setMessage("Button clicked!");
  };
  
  const handleClickWithParam = (text) => {
    setMessage(text);
  };
  
  return (
    <div>
      <p>{message}</p>
      <button onClick={handleClick}>Click Me</button>
      <button onClick={() => handleClickWithParam("Custom message")}>
        Click for Custom
      </button>
    </div>
  );
}

export default ClickHandler;
```

Event handlers can be defined as functions and passed to the `onClick` prop.  
When you need to pass parameters to a handler, wrap it in an arrow function  
`() => handler(param)`. Without the arrow function, the handler would execute  
immediately during render instead of on click.  

---

## Event Object

Accessing event properties and target information.  

```jsx
import { useState } from 'react';

function EventInfo() {
  const [info, setInfo] = useState("");
  
  const handleClick = (event) => {
    const { type, clientX, clientY } = event;
    setInfo(`Event: ${type} at (${clientX}, ${clientY})`);
  };
  
  return (
    <div>
      <button onClick={handleClick}>Click to Get Event Info</button>
      <p>{info}</p>
    </div>
  );
}

export default EventInfo;
```

Event handlers receive an event object containing information about the  
event. This synthetic event is normalized across browsers and provides  
properties like `type`, `target`, `clientX`, `clientY`, and methods like  
`preventDefault()` and `stopPropagation()`. You can access these properties  
to get detailed event information.  

---

## Checkbox State

Managing checkbox state in React.  

```jsx
import { useState } from 'react';

function CheckboxExample() {
  const [isChecked, setIsChecked] = useState(false);
  
  const handleChange = (event) => {
    setIsChecked(event.target.checked);
  };
  
  return (
    <div>
      <label>
        <input 
          type="checkbox"
          checked={isChecked}
          onChange={handleChange}
        />
        Accept terms and conditions
      </label>
      <p>Status: {isChecked ? "Accepted" : "Not accepted"}</p>
    </div>
  );
}

export default CheckboxExample;
```

For checkboxes, use the `checked` prop instead of `value` to control the  
state. The `event.target.checked` property (not `value`) contains the  
checkbox's boolean state. This creates a controlled checkbox component where  
React manages the checked state.  

---

## Radio Buttons

Handling radio button groups with state.  

```jsx
import { useState } from 'react';

function RadioButtons() {
  const [selected, setSelected] = useState("option1");
  
  const handleChange = (event) => {
    setSelected(event.target.value);
  };
  
  return (
    <div>
      <label>
        <input 
          type="radio"
          value="option1"
          checked={selected === "option1"}
          onChange={handleChange}
        />
        Option 1
      </label>
      <label>
        <input 
          type="radio"
          value="option2"
          checked={selected === "option2"}
          onChange={handleChange}
        />
        Option 2
      </label>
      <p>Selected: {selected}</p>
    </div>
  );
}

export default RadioButtons;
```

Radio buttons in a group share the same `name` attribute (if using HTML  
name) or the same state variable in React. Each radio button's `checked`  
prop compares the current state with its value. When a radio is selected,  
the state updates to that radio's value.  

---

## Select Dropdown

Creating controlled select dropdowns.  

```jsx
import { useState } from 'react';

function SelectExample() {
  const [country, setCountry] = useState("usa");
  
  const handleChange = (event) => {
    setCountry(event.target.value);
  };
  
  return (
    <div>
      <select value={country} onChange={handleChange}>
        <option value="usa">United States</option>
        <option value="canada">Canada</option>
        <option value="uk">United Kingdom</option>
        <option value="germany">Germany</option>
      </select>
      <p>Selected country: {country}</p>
    </div>
  );
}

export default SelectExample;
```

Select elements are controlled similarly to input fields. The `value` prop on  
the `<select>` element controls which option is selected. Unlike traditional  
HTML where you'd use `selected` on an option, React manages this through the  
select's value prop.  

---

## Basic useEffect

Using `useEffect` to run code after component renders.  

```jsx
import { useState, useEffect } from 'react';

function DocumentTitle() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    document.title = `Count: ${count}`;
  });
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default DocumentTitle;
```

`useEffect` runs after every render by default. It's perfect for side effects  
like updating the document title, logging, or any operation that shouldn't  
happen during render. The effect runs after the DOM has been updated, ensuring  
you're working with the latest component state.  

---

## useEffect with Dependency Array

Controlling when effects run with dependencies.  

```jsx
import { useState, useEffect } from 'react';

function DataLogger() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("");
  
  useEffect(() => {
    console.log(`Count changed to: ${count}`);
  }, [count]);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name (won't log)"
      />
    </div>
  );
}

export default DataLogger;
```

The dependency array `[count]` tells React to only run the effect when  
`count` changes. This optimizes performance by preventing unnecessary effect  
executions. The effect won't run when `name` changes because it's not in the  
dependency array. Always include all values from the component scope that the  
effect uses.  

---

## useEffect Cleanup

Cleaning up side effects to prevent memory leaks.  

```jsx
import { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    
    return () => {
      clearInterval(interval);
    };
  }, []);
  
  return <div>Seconds: {seconds}</div>;
}

export default Timer;
```

Return a cleanup function from `useEffect` to clean up side effects like  
intervals, timeouts, subscriptions, or event listeners. The cleanup runs  
before the component unmounts and before the effect runs again. The empty  
dependency array `[]` ensures the effect runs only once on mount.  

---

## Fetching Data with useEffect

Loading data from an API using useEffect.  

```jsx
import { useState, useEffect } from 'react';

function UserData() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const fetchUser = async () => {
      try {
        const response = await fetch('/api/user');
        const data = await response.json();
        setUser(data);
      } catch (error) {
        console.error('Error fetching user:', error);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, []);
  
  if (loading) return <p>Loading...</p>;
  
  return (
    <div>
      <h2>{user?.name}</h2>
      <p>{user?.email}</p>
    </div>
  );
}

export default UserData;
```

Data fetching is a common use case for `useEffect`. The effect runs once on  
mount (empty dependency array) to fetch data. Use separate state variables  
for the data and loading status. The optional chaining operator `?.` safely  
accesses properties that might not exist while loading.  

---

## Local Storage with useEffect

Persisting state to localStorage.  

```jsx
import { useState, useEffect } from 'react';

function PersistentCounter() {
  const [count, setCount] = useState(() => {
    const saved = localStorage.getItem('count');
    return saved !== null ? parseInt(saved) : 0;
  });
  
  useEffect(() => {
    localStorage.setItem('count', count.toString());
  }, [count]);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}

export default PersistentCounter;
```

Initialize state from localStorage using a function (lazy initialization).  
The effect syncs state changes to localStorage whenever `count` changes.  
This creates persistent state that survives page refreshes. Always check if  
the value exists before parsing to avoid errors.  

---

## Window Event Listener

Adding and removing window event listeners.  

```jsx
import { useState, useEffect } from 'react';

function WindowSize() {
  const [windowWidth, setWindowWidth] = useState(window.innerWidth);
  
  useEffect(() => {
    const handleResize = () => {
      setWindowWidth(window.innerWidth);
    };
    
    window.addEventListener('resize', handleResize);
    
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);
  
  return <div>Window width: {windowWidth}px</div>;
}

export default WindowSize;
```

Event listeners added to window or document should be cleaned up to prevent  
memory leaks. Add the listener in the effect and return a cleanup function  
that removes it. The empty dependency array ensures the listener is added  
once on mount and removed on unmount.  

---

## Conditional Effects

Running effects conditionally based on state.  

```jsx
import { useState, useEffect } from 'react';

function SearchBox() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  
  useEffect(() => {
    if (query.length >= 3) {
      // Simulate API search
      const searchResults = [
        `Result 1 for "${query}"`,
        `Result 2 for "${query}"`,
        `Result 3 for "${query}"`
      ];
      setResults(searchResults);
    } else {
      setResults([]);
    }
  }, [query]);
  
  return (
    <div>
      <input 
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search (min 3 chars)"
      />
      <ul>
        {results.map((result, index) => (
          <li key={index}>{result}</li>
        ))}
      </ul>
    </div>
  );
}

export default SearchBox;
```

Effects can contain conditional logic to run only when certain criteria are  
met. This example only searches when the query is at least 3 characters long,  
preventing unnecessary API calls for short queries. The effect still runs on  
every query change but the search logic is conditional.  

---

## Component Composition

Building larger components from smaller reusable ones.  

```jsx
function Avatar({ src, alt }) {
  return <img src={src} alt={alt} className="avatar" />;
}

function UserInfo({ name, email }) {
  return (
    <div>
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}

function UserProfile({ user }) {
  return (
    <div className="profile">
      <Avatar src={user.avatarUrl} alt={user.name} />
      <UserInfo name={user.name} email={user.email} />
    </div>
  );
}

export default UserProfile;
```

Component composition is a fundamental React pattern. Break complex UIs into  
smaller, focused components that each handle a single responsibility. The  
`UserProfile` composes `Avatar` and `UserInfo` components, making the code  
more maintainable and the components more reusable.  

---

## Lifting State Up

Sharing state between sibling components by lifting it to a common parent.  

```jsx
import { useState } from 'react';

function TemperatureInput({ scale, temperature, onTemperatureChange }) {
  return (
    <input 
      value={temperature}
      onChange={(e) => onTemperatureChange(e.target.value)}
      placeholder={`Temperature in ${scale}`}
    />
  );
}

function Calculator() {
  const [celsius, setCelsius] = useState("");
  const [fahrenheit, setFahrenheit] = useState("");
  
  const handleCelsiusChange = (value) => {
    setCelsius(value);
    setFahrenheit(value ? (parseFloat(value) * 9/5 + 32).toFixed(1) : "");
  };
  
  const handleFahrenheitChange = (value) => {
    setFahrenheit(value);
    setCelsius(value ? ((parseFloat(value) - 32) * 5/9).toFixed(1) : "");
  };
  
  return (
    <div>
      <TemperatureInput 
        scale="Celsius"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />
      <TemperatureInput 
        scale="Fahrenheit"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />
    </div>
  );
}

export default Calculator;
```

When two components need to share state, move the state to their closest  
common ancestor and pass it down as props along with update functions. This  
ensures a single source of truth. The parent `Calculator` manages the state  
and keeps both temperature inputs synchronized.  

---

## Derived State

Computing values from existing state instead of storing them separately.  

```jsx
import { useState } from 'react';

function ShoppingCart() {
  const [items, setItems] = useState([
    { id: 1, name: "Book", price: 12.99, quantity: 2 },
    { id: 2, name: "Pen", price: 1.99, quantity: 5 }
  ]);
  
  const totalItems = items.reduce((sum, item) => sum + item.quantity, 0);
  const totalPrice = items.reduce((sum, item) => 
    sum + (item.price * item.quantity), 0
  ).toFixed(2);
  
  return (
    <div>
      <h2>Shopping Cart</h2>
      {items.map(item => (
        <div key={item.id}>
          {item.name} - ${item.price} x {item.quantity}
        </div>
      ))}
      <p>Total Items: {totalItems}</p>
      <p>Total Price: ${totalPrice}</p>
    </div>
  );
}

export default ShoppingCart;
```

Avoid storing derived values in state. Instead, calculate them during render  
from existing state. Here, `totalItems` and `totalPrice` are computed from  
the `items` array. This prevents sync issues and reduces complexity. Values  
are automatically updated whenever the source state changes.  

---

## Form Validation

Validating form inputs and displaying error messages.  

```jsx
import { useState } from 'react';

function RegistrationForm() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [errors, setErrors] = useState({});
  
  const validate = () => {
    const newErrors = {};
    
    if (!email.includes('@')) {
      newErrors.email = "Invalid email address";
    }
    
    if (password.length < 8) {
      newErrors.password = "Password must be at least 8 characters";
    }
    
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      console.log("Form is valid!");
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
        {errors.email && <p className="error">{errors.email}</p>}
      </div>
      <div>
        <input 
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          placeholder="Password"
        />
        {errors.password && <p className="error">{errors.password}</p>}
      </div>
      <button type="submit">Register</button>
    </form>
  );
}

export default RegistrationForm;
```

Form validation typically involves checking inputs on submit and displaying  
errors. Store errors in state as an object keyed by field name. The  
validation function returns true if there are no errors. Display error  
messages conditionally next to each field using the `&&` operator.  

---

## Dynamic Styling

Applying dynamic styles based on state or props.  

```jsx
import { useState } from 'react';

function StyledButton() {
  const [isActive, setIsActive] = useState(false);
  
  const buttonStyle = {
    backgroundColor: isActive ? '#4CAF50' : '#f44336',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '4px',
    cursor: 'pointer',
    transition: 'background-color 0.3s'
  };
  
  return (
    <button 
      style={buttonStyle}
      onClick={() => setIsActive(!isActive)}
    >
      {isActive ? 'Active' : 'Inactive'}
    </button>
  );
}

export default StyledButton;
```

Inline styles in React are objects with camelCased property names. You can  
dynamically compute style values based on state or props. The `style` prop  
accepts a JavaScript object. This approach is useful for dynamic styles but  
CSS classes are generally preferred for static styles.  

---

## Conditional CSS Classes

Dynamically applying CSS classes based on state.  

```jsx
import { useState } from 'react';

function Message() {
  const [type, setType] = useState('info');
  
  const getClassName = () => {
    return `message message-${type}`;
  };
  
  return (
    <div>
      <div className={getClassName()}>
        This is a {type} message
      </div>
      <button onClick={() => setType('success')}>Success</button>
      <button onClick={() => setType('warning')}>Warning</button>
      <button onClick={() => setType('error')}>Error</button>
    </div>
  );
}

export default Message;
```

Build dynamic class names using template literals or string concatenation.  
The `className` prop accepts a string that can be computed from state or  
props. This allows you to apply different CSS classes based on component  
state, enabling dynamic styling while keeping styles in CSS files.  

---

## Hover State

Managing hover state for interactive elements.  

```jsx
import { useState } from 'react';

function HoverCard() {
  const [isHovered, setIsHovered] = useState(false);
  
  const cardStyle = {
    padding: '20px',
    border: '2px solid',
    borderColor: isHovered ? '#2196F3' : '#ccc',
    transform: isHovered ? 'scale(1.05)' : 'scale(1)',
    transition: 'all 0.3s ease',
    cursor: 'pointer'
  };
  
  return (
    <div 
      style={cardStyle}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <h3>Hover over me!</h3>
      <p>I change when you hover</p>
    </div>
  );
}

export default HoverCard;
```

Track hover state using `onMouseEnter` and `onMouseLeave` events. Set state  
to true when the mouse enters and false when it leaves. This pattern is  
useful for creating interactive hover effects that go beyond what CSS  
`:hover` can do, such as triggering other component updates.  

---

## Modal Component

Creating a reusable modal dialog component.  

```jsx
import { useState } from 'react';

function Modal({ isOpen, onClose, title, children }) {
  if (!isOpen) return null;
  
  return (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={(e) => e.stopPropagation()}>
        <div className="modal-header">
          <h2>{title}</h2>
          <button onClick={onClose}>×</button>
        </div>
        <div className="modal-body">
          {children}
        </div>
      </div>
    </div>
  );
}

function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);
  
  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>Open Modal</button>
      <Modal 
        isOpen={isModalOpen}
        onClose={() => setIsModalOpen(false)}
        title="My Modal"
      >
        <p>This is the modal content!</p>
      </Modal>
    </div>
  );
}

export default App;
```

Modal components demonstrate several React patterns: conditional rendering,  
composition with children, event handling, and event propagation control.  
The `stopPropagation()` prevents clicks inside the modal from closing it.  
The parent component controls when the modal is visible via state.  

---

## Accordion Component

Building an expandable accordion component.  

```jsx
import { useState } from 'react';

function AccordionItem({ title, children, isOpen, onToggle }) {
  return (
    <div className="accordion-item">
      <div className="accordion-header" onClick={onToggle}>
        <h3>{title}</h3>
        <span>{isOpen ? '−' : '+'}</span>
      </div>
      {isOpen && (
        <div className="accordion-content">
          {children}
        </div>
      )}
    </div>
  );
}

function Accordion() {
  const [openIndex, setOpenIndex] = useState(null);
  
  const sections = [
    { title: "Section 1", content: "Content for section 1" },
    { title: "Section 2", content: "Content for section 2" },
    { title: "Section 3", content: "Content for section 3" }
  ];
  
  return (
    <div className="accordion">
      {sections.map((section, index) => (
        <AccordionItem 
          key={index}
          title={section.title}
          isOpen={openIndex === index}
          onToggle={() => setOpenIndex(openIndex === index ? null : index)}
        >
          <p>{section.content}</p>
        </AccordionItem>
      ))}
    </div>
  );
}

export default Accordion;
```

The accordion maintains which section is open in parent state. Each item  
receives its open state and a toggle handler. Clicking an open item closes  
it (sets index to null), clicking a closed item opens it. This pattern  
ensures only one section is open at a time.  

---

## Tabs Component

Creating a tabbed interface component.  

```jsx
import { useState } from 'react';

function Tabs({ tabs }) {
  const [activeTab, setActiveTab] = useState(0);
  
  return (
    <div className="tabs">
      <div className="tab-headers">
        {tabs.map((tab, index) => (
          <button 
            key={index}
            className={activeTab === index ? 'tab active' : 'tab'}
            onClick={() => setActiveTab(index)}
          >
            {tab.label}
          </button>
        ))}
      </div>
      <div className="tab-content">
        {tabs[activeTab].content}
      </div>
    </div>
  );
}

function App() {
  const tabs = [
    { label: "Home", content: <div>Home content</div> },
    { label: "Profile", content: <div>Profile content</div> },
    { label: "Settings", content: <div>Settings content</div> }
  ];
  
  return <Tabs tabs={tabs} />;
}

export default App;
```

Tabs track the active tab index in state. Tab headers map over the tabs  
array, rendering buttons with conditional classes. Only the active tab's  
content is displayed. This component is highly reusable - pass different tab  
configurations to create various tabbed interfaces.  

---

## Dropdown Menu

Implementing a dropdown menu with click-outside-to-close functionality.  

```jsx
import { useState, useEffect, useRef } from 'react';

function Dropdown({ trigger, items }) {
  const [isOpen, setIsOpen] = useState(false);
  const dropdownRef = useRef(null);
  
  useEffect(() => {
    const handleClickOutside = (event) => {
      if (dropdownRef.current && 
          !dropdownRef.current.contains(event.target)) {
        setIsOpen(false);
      }
    };
    
    document.addEventListener('mousedown', handleClickOutside);
    return () => {
      document.removeEventListener('mousedown', handleClickOutside);
    };
  }, []);
  
  return (
    <div className="dropdown" ref={dropdownRef}>
      <button onClick={() => setIsOpen(!isOpen)}>
        {trigger}
      </button>
      {isOpen && (
        <div className="dropdown-menu">
          {items.map((item, index) => (
            <div 
              key={index}
              className="dropdown-item"
              onClick={() => {
                item.onClick();
                setIsOpen(false);
              }}
            >
              {item.label}
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default Dropdown;
```

This dropdown uses a `ref` to track the dropdown element and an effect to  
listen for clicks outside. When a click occurs outside the dropdown, it  
closes. The `contains()` method checks if the click target is within the  
dropdown. This pattern is common for dismissible overlays and menus.  

---

## Pagination Component

Creating a pagination control for lists.  

```jsx
import { useState } from 'react';

function Pagination({ items, itemsPerPage }) {
  const [currentPage, setCurrentPage] = useState(1);
  
  const totalPages = Math.ceil(items.length / itemsPerPage);
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const currentItems = items.slice(startIndex, endIndex);
  
  return (
    <div>
      <div className="items">
        {currentItems.map((item, index) => (
          <div key={index}>{item}</div>
        ))}
      </div>
      <div className="pagination">
        <button 
          onClick={() => setCurrentPage(prev => Math.max(prev - 1, 1))}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        <span>Page {currentPage} of {totalPages}</span>
        <button 
          onClick={() => setCurrentPage(prev => Math.min(prev + 1, totalPages))}
          disabled={currentPage === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}

export default Pagination;
```

Pagination divides a list into pages. Calculate which slice of items to show  
based on current page and items per page. Use `Math.max` and `Math.min` to  
prevent going below page 1 or above the total pages. The disabled buttons  
provide visual feedback when at the first or last page.  

---

## Search Filter

Filtering a list based on search input.  

```jsx
import { useState } from 'react';

function SearchFilter() {
  const [searchTerm, setSearchTerm] = useState("");
  
  const allItems = [
    "Apple", "Banana", "Cherry", "Date", "Elderberry",
    "Fig", "Grape", "Honeydew", "Kiwi", "Lemon"
  ];
  
  const filteredItems = allItems.filter(item =>
    item.toLowerCase().includes(searchTerm.toLowerCase())
  );
  
  return (
    <div>
      <input 
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search fruits..."
      />
      <ul>
        {filteredItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
      <p>Found {filteredItems.length} items</p>
    </div>
  );
}

export default SearchFilter;
```

Filter arrays by computing the filtered result during render. Use the array  
`filter()` method with a condition that checks if the search term is included  
in each item. Convert both to lowercase for case-insensitive searching. The  
filtered array updates automatically whenever the search term changes.  

---

## Sorting List

Implementing sortable lists with different criteria.  

```jsx
import { useState } from 'react';

function SortableList() {
  const [sortOrder, setSortOrder] = useState('asc');
  
  const users = [
    { id: 1, name: "Charlie", age: 35 },
    { id: 2, name: "Alice", age: 28 },
    { id: 3, name: "Bob", age: 42 }
  ];
  
  const sortedUsers = [...users].sort((a, b) => {
    if (sortOrder === 'asc') {
      return a.name.localeCompare(b.name);
    } else {
      return b.name.localeCompare(a.name);
    }
  });
  
  return (
    <div>
      <button onClick={() => setSortOrder(sortOrder === 'asc' ? 'desc' : 'asc')}>
        Sort: {sortOrder === 'asc' ? '↑' : '↓'}
      </button>
      <ul>
        {sortedUsers.map(user => (
          <li key={user.id}>{user.name} - {user.age} years</li>
        ))}
      </ul>
    </div>
  );
}

export default SortableList;
```

Create a copy of the array with spread `[...users]` before sorting to avoid  
mutating the original. Use `localeCompare()` for string comparison which  
handles sorting correctly for different locales. Toggle between ascending and  
descending order by reversing the comparison logic based on state.  

---

## Debounced Search

Implementing search with debouncing to reduce API calls.  

```jsx
import { useState, useEffect } from 'react';

function DebouncedSearch() {
  const [searchTerm, setSearchTerm] = useState("");
  const [debouncedTerm, setDebouncedTerm] = useState("");
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedTerm(searchTerm);
    }, 500);
    
    return () => {
      clearTimeout(timer);
    };
  }, [searchTerm]);
  
  useEffect(() => {
    if (debouncedTerm) {
      console.log("Searching for:", debouncedTerm);
      // Perform API call here
    }
  }, [debouncedTerm]);
  
  return (
    <div>
      <input 
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search (debounced)..."
      />
      <p>Searching for: {debouncedTerm}</p>
    </div>
  );
}

export default DebouncedSearch;
```

Debouncing delays executing a function until after a period of inactivity.  
The first effect sets a timer that updates `debouncedTerm` after 500ms. If  
the user types again before 500ms, the cleanup function clears the previous  
timer. The second effect performs the actual search when `debouncedTerm`  
changes, reducing API calls dramatically.  

---

## Loading State Pattern

Managing loading, error, and success states for async operations.  

```jsx
import { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        
        // Simulate API call
        await new Promise(resolve => setTimeout(resolve, 2000));
        const result = { message: "Data loaded successfully!" };
        
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return <div>{data?.message}</div>;
}

export default DataFetcher;
```

The loading-error-data pattern is standard for async operations. Track three  
states: loading (boolean), error (error object or null), and data (the  
result). Use conditional rendering to show appropriate UI for each state.  
The `finally` block ensures loading is set to false whether the request  
succeeds or fails.  

---

## Custom Hook for Form

Creating a custom hook to manage form state.  

```jsx
import { useState } from 'react';

function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);
  
  const handleChange = (event) => {
    const { name, value } = event.target;
    setValues(prev => ({ ...prev, [name]: value }));
  };
  
  const resetForm = () => {
    setValues(initialValues);
  };
  
  return { values, handleChange, resetForm };
}

function ContactForm() {
  const { values, handleChange, resetForm } = useForm({
    name: "",
    email: "",
    message: ""
  });
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Submitted:", values);
    resetForm();
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={values.name} onChange={handleChange} />
      <input name="email" value={values.email} onChange={handleChange} />
      <textarea name="message" value={values.message} onChange={handleChange} />
      <button type="submit">Submit</button>
      <button type="button" onClick={resetForm}>Reset</button>
    </form>
  );
}

export default ContactForm;
```

Custom hooks extract reusable logic from components. This `useForm` hook  
manages form state and provides handlers for common operations. Custom hooks  
must start with "use" and can call other hooks. They return values and  
functions that components can use, promoting code reuse across multiple  
forms.  

---

## Custom Hook for LocalStorage

Creating a hook that syncs state with localStorage.  

```jsx
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });
  
  const setValue = (value) => {
    try {
      const valueToStore = value instanceof Function 
        ? value(storedValue) 
        : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}

function App() {
  const [name, setName] = useLocalStorage('name', '');
  
  return (
    <div>
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Your name (persisted)"
      />
      <p>Hello, {name}!</p>
    </div>
  );
}

export default App;
```

This custom hook provides a localStorage-synced state that works like  
`useState`. It initializes from localStorage if available, otherwise uses the  
initial value. The custom setter updates both state and localStorage. The  
hook handles JSON serialization/deserialization and error cases, providing a  
clean API for persistent state.  

---

## Error Boundary Simulation

Demonstrating error handling patterns in React components.  

```jsx
import { useState } from 'react';

function ErrorProneComponent({ shouldError }) {
  if (shouldError) {
    throw new Error("Component crashed!");
  }
  return <div>Component is working fine</div>;
}

function SafeComponent() {
  const [hasError, setHasError] = useState(false);
  const [showError, setShowError] = useState(false);
  
  if (hasError) {
    return (
      <div>
        <h2>Something went wrong!</h2>
        <button onClick={() => setHasError(false)}>Try Again</button>
      </div>
    );
  }
  
  const handleClick = () => {
    try {
      setShowError(true);
    } catch (error) {
      setHasError(true);
    }
  };
  
  return (
    <div>
      <button onClick={handleClick}>Trigger Error</button>
      {showError && <ErrorProneComponent shouldError={showError} />}
    </div>
  );
}

export default SafeComponent;
```

Error boundaries in React catch errors during rendering. While functional  
components can't be error boundaries directly (only class components can),  
this example shows error handling patterns. In production, wrap parts of your  
app in error boundary components to prevent the entire app from crashing when  
a component errors.  

---

## Controlled vs Uncontrolled Input

Comparing controlled and uncontrolled input approaches.  

```jsx
import { useState, useRef } from 'react';

function InputComparison() {
  // Controlled input
  const [controlledValue, setControlledValue] = useState("");
  
  // Uncontrolled input
  const uncontrolledRef = useRef(null);
  
  const handleControlledSubmit = (e) => {
    e.preventDefault();
    console.log("Controlled value:", controlledValue);
  };
  
  const handleUncontrolledSubmit = (e) => {
    e.preventDefault();
    console.log("Uncontrolled value:", uncontrolledRef.current.value);
  };
  
  return (
    <div>
      <form onSubmit={handleControlledSubmit}>
        <h3>Controlled Input</h3>
        <input 
          value={controlledValue}
          onChange={(e) => setControlledValue(e.target.value)}
        />
        <button type="submit">Submit</button>
      </form>
      
      <form onSubmit={handleUncontrolledSubmit}>
        <h3>Uncontrolled Input</h3>
        <input ref={uncontrolledRef} defaultValue="" />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default InputComparison;
```

Controlled inputs have their value managed by React state, giving you full  
control and the ability to validate or transform input in real-time.  
Uncontrolled inputs manage their own state internally; you access the value  
via a ref when needed. Controlled inputs are preferred in React for their  
predictability and testing advantages.  

---

## Context-like State Sharing

Demonstrating prop drilling and motivating the need for context.  

```jsx
import { useState } from 'react';

function Grandchild({ theme, setTheme }) {
  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}

function Child({ theme, setTheme }) {
  return <Grandchild theme={theme} setTheme={setTheme} />;
}

function Parent() {
  const [theme, setTheme] = useState('light');
  
  return (
    <div className={`app ${theme}`}>
      <h1>Theme Example</h1>
      <Child theme={theme} setTheme={setTheme} />
    </div>
  );
}

export default Parent;
```

Prop drilling occurs when you pass props through intermediate components that  
don't use them. Here, `Child` receives `theme` and `setTheme` only to pass  
them to `Grandchild`. For deeply nested components, this becomes cumbersome.  
React Context API solves this by allowing components to access shared state  
without prop drilling.  

---

This completes the 50 examples covering React basics from simple concepts to  
more advanced patterns. Each example builds on previous knowledge and  
demonstrates practical React development patterns you'll use in real  
applications.  
