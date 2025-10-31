# Conditional Rendering

Conditional rendering in React allows you to display different UI elements  
based on certain conditions, similar to how conditional statements work in  
JavaScript. React components can decide what to render based on the  
application state, props, or any other logic. This capability is fundamental  
to creating dynamic and interactive user interfaces.  

In React, you can use JavaScript's conditional operators to create elements  
that represent the current state, and React will update the UI to match them.  
The most common approaches for conditional rendering include:  

- **Logical AND (`&&`) operator**: Renders content when a condition is true  
- **Ternary operator (`? :`)**: Chooses between two rendering options  
- **If-else statements**: Used outside JSX for more complex logic  
- **Inline conditionals**: Embedded directly within JSX  

This document focuses on the two most common and React-friendly approaches:  
the logical `&&` operator and the ternary operator.  

## Basic Conditional Rendering with `&&`

The logical AND (`&&`) operator provides a concise way to conditionally  
render elements. If the condition before `&&` is true, the element after  
it will be rendered. If the condition is false, React ignores and skips it.  

```jsx
import { useState } from 'react';

function NotificationBanner() {
  const [hasNotification, setHasNotification] = useState(true);

  return (
    <div>
      <h1>Welcome to Your Dashboard</h1>
      {hasNotification && (
        <div style={{ padding: '10px', backgroundColor: '#ffeb3b' }}>
          You have new notifications!
        </div>
      )}
      <button onClick={() => setHasNotification(!hasNotification)}>
        Toggle Notification
      </button>
    </div>
  );
}

export default NotificationBanner;
```

This example demonstrates a notification banner that appears only when  
`hasNotification` is `true`. The `&&` operator evaluates the left side  
first: if it's truthy, React renders the element on the right side. When  
you click the toggle button, the state changes and the banner appears or  
disappears accordingly. This pattern is ideal for showing optional UI  
elements like notifications, warnings, or additional content.  

## Conditional Rendering with Ternary Operator

The ternary operator (`condition ? trueValue : falseValue`) allows you to  
choose between two different rendering outcomes. This is useful when you  
need to render one thing or another, rather than something or nothing.  

```jsx
import { useState } from 'react';

function LoginStatus() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      <h1>User Authentication</h1>
      {isLoggedIn ? (
        <p style={{ color: 'green' }}>Welcome back! You are logged in.</p>
      ) : (
        <p style={{ color: 'red' }}>Please log in to continue.</p>
      )}
      <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
        {isLoggedIn ? 'Log Out' : 'Log In'}
      </button>
    </div>
  );
}

export default LoginStatus;
```

This component shows different messages based on the login status. When  
`isLoggedIn` is `true`, it displays a welcome message; otherwise, it prompts  
the user to log in. Notice how the ternary operator is also used within the  
button text to change the label dynamically. The ternary operator provides  
a clean, readable way to switch between two distinct UI states.  

## Combining Multiple Conditions

You can combine multiple conditional rendering techniques to handle more  
complex scenarios. This example shows how to use both `&&` and ternary  
operators together for sophisticated UI logic.  

```jsx
import { useState } from 'react';

function ShoppingCart() {
  const [items, setItems] = useState([]);
  const [isCheckingOut, setIsCheckingOut] = useState(false);

  const addItem = () => {
    setItems([...items, `Item ${items.length + 1}`]);
  };

  const clearCart = () => {
    setItems([]);
    setIsCheckingOut(false);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h1>Shopping Cart</h1>
      
      {items.length === 0 ? (
        <p style={{ color: '#666' }}>Your cart is empty. Add items to get started!</p>
      ) : (
        <div>
          <p>You have {items.length} item(s) in your cart</p>
          <ul>
            {items.map((item, index) => (
              <li key={index}>{item}</li>
            ))}
          </ul>
        </div>
      )}

      {items.length > 0 && !isCheckingOut && (
        <button 
          onClick={() => setIsCheckingOut(true)}
          style={{ marginRight: '10px' }}
        >
          Proceed to Checkout
        </button>
      )}

      {isCheckingOut && (
        <div style={{ marginTop: '20px', padding: '10px', border: '2px solid green' }}>
          <h2>Checkout</h2>
          <p>Processing {items.length} item(s)...</p>
          <button onClick={clearCart}>Complete Purchase</button>
        </div>
      )}

      <button onClick={addItem} style={{ marginRight: '10px' }}>
        Add Item
      </button>
      
      {items.length > 0 && (
        <button onClick={clearCart}>Clear Cart</button>
      )}
    </div>
  );
}

export default ShoppingCart;
```

This shopping cart component demonstrates multiple conditional rendering  
patterns working together. The ternary operator switches between an empty  
cart message and the cart contents. The `&&` operator shows the checkout  
button only when there are items and checkout hasn't started. Another `&&`  
conditional displays the checkout section when the user begins checking out.  
The clear button appears only when the cart contains items. This pattern  
allows you to build complex, state-dependent UIs while keeping the code  
readable and maintainable.  

## Conditional Rendering with Arrays

When working with lists, you can use conditional rendering to handle empty  
states, loading states, or filter results. This example demonstrates  
rendering different UI based on array content.  

```jsx
import { useState } from 'react';

function TaskList() {
  const [tasks, setTasks] = useState([
    { id: 1, text: 'Review pull requests', completed: false },
    { id: 2, text: 'Write documentation', completed: true },
    { id: 3, text: 'Fix bug #123', completed: false }
  ]);
  const [filter, setFilter] = useState('all');

  const toggleTask = (id) => {
    setTasks(tasks.map(task =>
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  };

  const filteredTasks = tasks.filter(task => {
    if (filter === 'active') return !task.completed;
    if (filter === 'completed') return task.completed;
    return true;
  });

  return (
    <div style={{ padding: '20px' }}>
      <h1>Task Manager</h1>
      
      <div style={{ marginBottom: '20px' }}>
        <button onClick={() => setFilter('all')}>All</button>
        <button onClick={() => setFilter('active')}>Active</button>
        <button onClick={() => setFilter('completed')}>Completed</button>
      </div>

      {filteredTasks.length === 0 ? (
        <p style={{ fontStyle: 'italic', color: '#888' }}>
          No tasks found. {filter !== 'all' && `Try a different filter.`}
        </p>
      ) : (
        <ul style={{ listStyle: 'none', padding: 0 }}>
          {filteredTasks.map(task => (
            <li 
              key={task.id} 
              style={{ 
                padding: '10px', 
                marginBottom: '5px',
                backgroundColor: task.completed ? '#d4edda' : '#f8f9fa',
                textDecoration: task.completed ? 'line-through' : 'none',
                cursor: 'pointer'
              }}
              onClick={() => toggleTask(task.id)}
            >
              {task.text}
              {task.completed && ' ✓'}
            </li>
          ))}
        </ul>
      )}

      <p style={{ marginTop: '20px', fontSize: '14px', color: '#666' }}>
        Showing {filteredTasks.length} of {tasks.length} task(s)
      </p>
    </div>
  );
}

export default TaskList;
```

This task manager uses conditional rendering to show different content based  
on the filtered task list. The ternary operator displays an empty state  
message when no tasks match the current filter, or renders the task list  
when tasks are present. Within the empty message, the `&&` operator adds  
helpful text suggesting to try a different filter (but only when a filter  
is active). Each task item uses the `&&` operator to show a checkmark only  
for completed tasks. This approach creates an informative, dynamic UI that  
responds to both user interactions and data states.  

## Advanced Pattern: Conditional Styling

Conditional rendering isn't limited to showing or hiding elements. You can  
also conditionally apply styles, classes, or attributes based on state.  

```jsx
import { useState } from 'react';

function ThemeToggle() {
  const [isDarkMode, setIsDarkMode] = useState(false);
  const [fontSize, setFontSize] = useState('medium');

  const containerStyle = {
    padding: '30px',
    backgroundColor: isDarkMode ? '#1e1e1e' : '#ffffff',
    color: isDarkMode ? '#ffffff' : '#000000',
    minHeight: '300px',
    transition: 'all 0.3s ease'
  };

  const textSizeMap = {
    small: '14px',
    medium: '16px',
    large: '20px'
  };

  return (
    <div style={containerStyle}>
      <h1 style={{ fontSize: textSizeMap[fontSize] }}>
        {isDarkMode ? '🌙 Dark Mode Active' : '☀️ Light Mode Active'}
      </h1>
      
      <p style={{ fontSize: textSizeMap[fontSize] }}>
        This text adjusts its appearance based on your preferences.
      </p>

      <div style={{ marginTop: '20px' }}>
        <button 
          onClick={() => setIsDarkMode(!isDarkMode)}
          style={{
            padding: '10px 20px',
            marginRight: '10px',
            backgroundColor: isDarkMode ? '#4CAF50' : '#2196F3',
            color: 'white',
            border: 'none',
            cursor: 'pointer'
          }}
        >
          {isDarkMode ? 'Switch to Light' : 'Switch to Dark'}
        </button>
        
        <select 
          value={fontSize} 
          onChange={(e) => setFontSize(e.target.value)}
          style={{
            padding: '10px',
            backgroundColor: isDarkMode ? '#333' : '#f5f5f5',
            color: isDarkMode ? '#fff' : '#000',
            border: `1px solid ${isDarkMode ? '#555' : '#ddd'}`
          }}
        >
          <option value="small">Small Text</option>
          <option value="medium">Medium Text</option>
          <option value="large">Large Text</option>
        </select>
      </div>

      {fontSize === 'large' && (
        <p style={{ marginTop: '20px', color: isDarkMode ? '#ffd700' : '#ff9800' }}>
          Large text size selected for better readability!
        </p>
      )}
    </div>
  );
}

export default ThemeToggle;
```

This theme toggle component demonstrates conditional styling techniques. The  
ternary operator determines background colors, text colors, and button  
styles based on the `isDarkMode` state. The heading text and icon change  
conditionally to reflect the current theme. Multiple style properties are  
computed using conditional expressions, creating a cohesive visual experience.  
The `&&` operator shows an additional message when large text is selected.  
This pattern allows you to create highly dynamic, user-customizable  
interfaces where both content and presentation adapt to state changes.  

## Best Practices

When using conditional rendering in React, keep these guidelines in mind:  

**Use `&&` for simple show/hide logic**: When you only need to render  
something or nothing, the `&&` operator is the cleanest choice. Avoid using  
it with numbers (e.g., `count && <Component />`) as it might render `0`.  

**Use ternary for either/or scenarios**: When you need to render one of two  
different elements, the ternary operator provides clarity and readability.  

**Avoid deeply nested conditionals**: If you find yourself nesting multiple  
ternary operators, consider extracting the logic into a separate function  
or component. Readability should be a priority.  

**Be cautious with falsy values**: The `&&` operator will render falsy values  
like `0` or `NaN`. Always ensure your condition evaluates to a boolean when  
necessary. Use `!!` or explicit comparison (`value > 0`) to be safe.  

**Extract complex conditions**: If a conditional expression becomes complex,  
extract it to a variable or function with a descriptive name. This improves  
code readability and makes your intent clear.  

**Consider component composition**: For very complex conditional logic,  
breaking your component into smaller, focused components can be cleaner  
than extensive inline conditionals.  

## Common Pitfalls to Avoid

```jsx
import { useState } from 'react';

function CommonMistakes() {
  const [count, setCount] = useState(0);
  const [items, setItems] = useState([]);

  return (
    <div style={{ padding: '20px' }}>
      <h2>Common Pitfalls</h2>
      
      {/* ❌ Wrong: May render '0' instead of nothing */}
      <div>
        <h3>Count Display (Problematic)</h3>
        {count && <p>Count: {count}</p>}
      </div>

      {/* ✅ Correct: Always renders boolean */}
      <div>
        <h3>Count Display (Fixed)</h3>
        {count > 0 && <p>Count: {count}</p>}
      </div>

      {/* ❌ Wrong: Unnecessarily complex */}
      <div>
        <h3>Status (Overcomplicated)</h3>
        {items.length > 0 ? (
          items.length === 1 ? (
            <p>One item</p>
          ) : (
            <p>Multiple items</p>
          )
        ) : (
          <p>No items</p>
        )}
      </div>

      {/* ✅ Correct: Extracted to function for clarity */}
      <div>
        <h3>Status (Clean)</h3>
        <p>{getStatusMessage(items.length)}</p>
      </div>

      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setItems([...items, 'item'])}>Add Item</button>
    </div>
  );
}

function getStatusMessage(length) {
  if (length === 0) return 'No items';
  if (length === 1) return 'One item';
  return 'Multiple items';
}

export default CommonMistakes;
```

This example highlights common mistakes in conditional rendering. The first  
pitfall shows how using `&&` with a number can accidentally render `0` when  
the count is zero, instead of rendering nothing. The fix uses an explicit  
comparison (`count > 0`) to ensure a boolean result. The second pitfall  
demonstrates nested ternary operators creating hard-to-read code. The  
solution extracts this logic into a separate function with a clear name,  
making the code more maintainable and testable. Always prioritize code  
clarity—future you (and your teammates) will appreciate it.  
