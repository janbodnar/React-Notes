# Side effects

## Introduction to Side Effects and useEffect

Side effects are operations that interact with the outside world or affect  
something beyond the component's scope. In React, side effects include data  
fetching from APIs, setting up subscriptions, manually changing the DOM,  
timers, and logging. These operations don't fit into the normal render flow  
and need special handling.  

The `useEffect` Hook is React's built-in solution for managing side effects  
in functional components. It allows you to perform side effects after the  
component renders, and optionally clean them up when the component unmounts  
or when dependencies change.  

**Key Concepts:**

- **Hooks** are special functions that let you use React features in  
  functional components
- **Side Effects** are operations that interact with systems outside the  
  component
- **Functional Components** are JavaScript functions that return JSX and can  
  use Hooks
- **Dependency Array** controls when the effect runs based on value changes  
- **Cleanup Functions** prevent memory leaks by cleaning up resources  

Understanding `useEffect` is essential for building React applications that  
interact with APIs, handle user events, and manage resources efficiently.  

---

## Basic useEffect Usage

The `useEffect` Hook accepts two parameters: a function containing the side  
effect logic, and an optional dependency array that controls when the effect  
runs.  

```javascript
import { useEffect, useState } from 'react';

function BasicEffect() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Current count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default BasicEffect;
```

This example demonstrates the fundamental `useEffect` pattern. The effect  
updates the browser's document title whenever the `count` state changes. The  
dependency array `[count]` tells React to run this effect only when `count`  
changes, preventing unnecessary updates.  

---

## Data Fetching with useEffect

Data fetching is one of the most common use cases for `useEffect`. When  
fetching data from an API, you typically want to make the request after the  
component mounts.  

```javascript
import { useEffect, useState } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        if (!response.ok) {
          throw new Error('Failed to fetch users');
        }
        const data = await response.json();
        setUsers(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

  if (loading) return <div>Loading users...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <h2>User List</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

This example shows a complete data fetching pattern with loading and error  
states. The empty dependency array `[]` ensures the fetch happens only once  
when the component mounts. The async function is defined inside `useEffect`  
to handle the asynchronous operation properly. Error handling provides a  
better user experience by catching and displaying network issues.  

---

## Fetching Data with Dependencies

Sometimes you need to refetch data when certain values change, such as  
search queries or filter parameters.  

```javascript
import { useEffect, useState } from 'react';

function UserSearch() {
  const [searchTerm, setSearchTerm] = useState('');
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (!searchTerm) {
      setUsers([]);
      return;
    }

    const fetchUsers = async () => {
      setLoading(true);
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users?name_like=${searchTerm}`
        );
        const data = await response.json();
        setUsers(data);
      } catch (error) {
        console.error('Error fetching users:', error);
      } finally {
        setLoading(false);
      }
    };

    const timeoutId = setTimeout(fetchUsers, 300);
    return () => clearTimeout(timeoutId);
  }, [searchTerm]);

  return (
    <div>
      <input
        type="text"
        placeholder="Search users..."
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
      />
      {loading && <div>Searching...</div>}
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default UserSearch;
```

This example demonstrates dependent data fetching with debouncing. The  
effect runs whenever `searchTerm` changes, preventing excessive API calls  
by using a timeout. The cleanup function clears the timeout if the user  
types again before the delay completes, implementing a debounce pattern.  

---

## Subscriptions with Event Listeners

Event listeners are a common subscription pattern that requires proper  
cleanup to prevent memory leaks.  

```javascript
import { useEffect, useState } from 'react';

function WindowResize() {
  const [windowWidth, setWindowWidth] = useState(window.innerWidth);
  const [windowHeight, setWindowHeight] = useState(window.innerHeight);

  useEffect(() => {
    const handleResize = () => {
      setWindowWidth(window.innerWidth);
      setWindowHeight(window.innerHeight);
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return (
    <div>
      <h2>Window Dimensions</h2>
      <p>Width: {windowWidth}px</p>
      <p>Height: {windowHeight}px</p>
    </div>
  );
}

export default WindowResize;
```

This example shows how to subscribe to window resize events. The cleanup  
function removes the event listener when the component unmounts, preventing  
memory leaks. The empty dependency array ensures the subscription is set up  
only once.  

---

## Timer Subscriptions

Timers like `setInterval` and `setTimeout` need careful cleanup to avoid  
unexpected behavior and memory issues.  

```javascript
import { useEffect, useState } from 'react';

function CountdownTimer({ initialSeconds = 60 }) {
  const [seconds, setSeconds] = useState(initialSeconds);
  const [isActive, setIsActive] = useState(false);

  useEffect(() => {
    let intervalId = null;

    if (isActive && seconds > 0) {
      intervalId = setInterval(() => {
        setSeconds(prevSeconds => prevSeconds - 1);
      }, 1000);
    }

    return () => {
      if (intervalId) {
        clearInterval(intervalId);
      }
    };
  }, [isActive, seconds]);

  const toggle = () => setIsActive(!isActive);
  const reset = () => {
    setSeconds(initialSeconds);
    setIsActive(false);
  };

  return (
    <div>
      <h2>Countdown: {seconds}s</h2>
      <button onClick={toggle}>
        {isActive ? 'Pause' : 'Start'}
      </button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

export default CountdownTimer;
```

This countdown timer demonstrates interval management with `useEffect`. The  
cleanup function clears the interval to prevent it from running after the  
component unmounts or when the dependencies change. Using the functional  
update form `prevSeconds => prevSeconds - 1` ensures accurate counting even  
if the component re-renders frequently.  

---

## Manual DOM Manipulation

While React's declarative approach handles most DOM updates, sometimes you  
need direct DOM access for third-party libraries or specific requirements.  

```javascript
import { useEffect, useRef, useState } from 'react';

function FocusInput() {
  const inputRef = useRef(null);
  const [value, setValue] = useState('');

  useEffect(() => {
    if (inputRef.current) {
      inputRef.current.focus();
    }
  }, []);

  return (
    <div>
      <h2>Auto-focused Input</h2>
      <input
        ref={inputRef}
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="This input is automatically focused"
      />
    </div>
  );
}

export default FocusInput;
```

This example uses `useRef` to access a DOM element and `useEffect` to focus  
it when the component mounts. The `ref` object provides direct access to the  
underlying DOM node, while `useEffect` ensures the focus happens after the  
element is rendered.  

---

## Advanced DOM Manipulation with Canvas

For more complex DOM manipulation like working with canvas or integrating  
third-party visualization libraries:  

```javascript
import { useEffect, useRef } from 'react';

function CanvasDrawing() {
  const canvasRef = useRef(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext('2d');
    
    ctx.fillStyle = '#4A90E2';
    ctx.fillRect(10, 10, 150, 100);
    
    ctx.strokeStyle = '#E94B3C';
    ctx.lineWidth = 3;
    ctx.strokeRect(180, 10, 150, 100);
    
    ctx.beginPath();
    ctx.arc(85, 180, 50, 0, 2 * Math.PI);
    ctx.fillStyle = '#50C878';
    ctx.fill();
  }, []);

  return (
    <div>
      <h2>Canvas Drawing</h2>
      <canvas
        ref={canvasRef}
        width={400}
        height={300}
        style={{ border: '1px solid #ccc' }}
      />
    </div>
  );
}

export default CanvasDrawing;
```

This example demonstrates manual DOM manipulation with a canvas element. The  
effect runs once after mount to draw shapes on the canvas. Direct DOM access  
through refs is necessary here because React's declarative rendering doesn't  
directly support canvas drawing operations.  

---

## Understanding the Dependency Array

The dependency array is the second argument to `useEffect` and is crucial  
for controlling when effects run. It determines whether the effect should  
re-run based on value changes.  

### Empty Dependency Array

```javascript
useEffect(() => {
  console.log('Runs once after initial render');
}, []);
```

An empty array `[]` means the effect runs only once after the component  
mounts, similar to `componentDidMount` in class components.  

### With Dependencies

```javascript
useEffect(() => {
  console.log('Runs when count changes');
}, [count]);
```

Including variables in the array makes the effect run whenever those  
variables change. React performs a shallow comparison to detect changes.  

### No Dependency Array

```javascript
useEffect(() => {
  console.log('Runs after every render');
});
```

Omitting the dependency array causes the effect to run after every render,  
which can lead to performance issues and infinite loops if not used  
carefully.  

### Multiple Dependencies

```javascript
import { useEffect, useState } from 'react';

function MultiDependencyExample() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  const [fullName, setFullName] = useState('');

  useEffect(() => {
    setFullName(`${firstName} ${lastName}`.trim());
  }, [firstName, lastName]);

  return (
    <div>
      <input
        type="text"
        placeholder="First name"
        value={firstName}
        onChange={(e) => setFirstName(e.target.value)}
      />
      <input
        type="text"
        placeholder="Last name"
        value={lastName}
        onChange={(e) => setLastName(e.target.value)}
      />
      <p>Full name: {fullName}</p>
    </div>
  );
}

export default MultiDependencyExample;
```

This example shows an effect with multiple dependencies. The effect runs  
whenever `firstName` or `lastName` changes, updating the `fullName` state.  
Including both dependencies ensures the full name updates correctly  
regardless of which input changes.  

---

## Importance of Dependencies: Common Pitfalls

### Stale Closures

```javascript
import { useEffect, useState } from 'react';

function StaleClosureExample() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      console.log('Count:', count);
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(intervalId);
  }, [count]);

  return <div>Count: {count}</div>;
}

export default StaleClosureExample;
```

This example correctly includes `count` in the dependency array. Without it,  
the effect would capture the initial value of `count` (0) and always log 0,  
creating a stale closure. Including `count` ensures the effect sees the  
current value.  

### Missing Dependencies Warning

React's ESLint plugin warns about missing dependencies. Always include all  
values from the component scope that the effect uses to avoid bugs.  

```javascript
// ❌ Bad: Missing dependency
useEffect(() => {
  console.log(count);
}, []);

// ✅ Good: All dependencies included
useEffect(() => {
  console.log(count);
}, [count]);
```

---

## Cleanup Functions

Cleanup functions prevent memory leaks and unexpected behavior by removing  
subscriptions, canceling requests, and clearing timers when components  
unmount or before effects re-run.  

### Basic Cleanup Pattern

```javascript
useEffect(() => {
  // Setup code
  const subscription = subscribeToData();

  return () => {
    // Cleanup code
    subscription.unsubscribe();
  };
}, [dependencies]);
```

The cleanup function (returned from the effect) runs before the component  
unmounts and before the effect re-runs if dependencies change.  

### Cleanup with Async Operations

```javascript
import { useEffect, useState } from 'react';

function DataFetchWithCleanup({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let isCancelled = false;

    const fetchUser = async () => {
      setLoading(true);
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/users/${userId}`
        );
        const data = await response.json();
        
        if (!isCancelled) {
          setUser(data);
        }
      } catch (error) {
        if (!isCancelled) {
          console.error('Error:', error);
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    };

    fetchUser();

    return () => {
      isCancelled = true;
    };
  }, [userId]);

  if (loading) return <div>Loading...</div>;
  if (!user) return <div>No user found</div>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>Email: {user.email}</p>
    </div>
  );
}

export default DataFetchWithCleanup;
```

This example prevents state updates on unmounted components using a  
cancellation flag. When the component unmounts or `userId` changes, the  
cleanup function sets `isCancelled` to true, preventing state updates from  
completing async operations.  

---

## Complex Subscription Example

Combining multiple concepts for a real-world subscription scenario:  

```javascript
import { useEffect, useState } from 'react';

function LiveDataMonitor({ endpoint }) {
  const [data, setData] = useState([]);
  const [isConnected, setIsConnected] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    let intervalId = null;
    let isCancelled = false;

    const fetchData = async () => {
      try {
        const response = await fetch(endpoint);
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        const result = await response.json();
        
        if (!isCancelled) {
          setData(result);
          setIsConnected(true);
          setError(null);
        }
      } catch (err) {
        if (!isCancelled) {
          setError(err.message);
          setIsConnected(false);
        }
      }
    };

    fetchData();
    intervalId = setInterval(fetchData, 5000);

    return () => {
      isCancelled = true;
      if (intervalId) {
        clearInterval(intervalId);
      }
    };
  }, [endpoint]);

  return (
    <div>
      <h2>Live Data Monitor</h2>
      <p>Status: {isConnected ? '🟢 Connected' : '🔴 Disconnected'}</p>
      {error && <p>Error: {error}</p>}
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default LiveDataMonitor;
```

This comprehensive example combines data fetching, polling, cleanup, and  
error handling. The effect fetches data immediately and then every 5  
seconds. The cleanup function cancels ongoing operations and clears the  
interval when the component unmounts or the endpoint changes.  

---

## Best Practices

### Keep Effects Focused

Each effect should handle one concern. Split complex logic into multiple  
effects:  

```javascript
// ✅ Good: Separate concerns
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);

useEffect(() => {
  const handleResize = () => setWindowWidth(window.innerWidth);
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// ❌ Bad: Mixed concerns
useEffect(() => {
  document.title = `Count: ${count}`;
  const handleResize = () => setWindowWidth(window.innerWidth);
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, [count]);
```

### Use Functional Updates

When updating state based on previous state, use functional updates to  
avoid dependency issues:  

```javascript
// ✅ Good: Functional update
useEffect(() => {
  const id = setInterval(() => {
    setCount(c => c + 1);
  }, 1000);
  return () => clearInterval(id);
}, []);

// ❌ Bad: Requires count in dependencies
useEffect(() => {
  const id = setInterval(() => {
    setCount(count + 1);
  }, 1000);
  return () => clearInterval(id);
}, [count]);
```

### Extract Reusable Logic to Custom Hooks

For complex or repeated effect logic, create custom hooks:  

```javascript
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
  return <div>Window width: {width}px</div>;
}

export default ResponsiveComponent;
```

Custom hooks encapsulate effect logic for reuse across components, making  
your code more maintainable and testable.  

### Always Clean Up Resources

Resources like timers, subscriptions, and event listeners must be cleaned  
up to prevent memory leaks:  

```javascript
useEffect(() => {
  const timer = setTimeout(() => {}, 1000);
  const listener = () => {};
  window.addEventListener('scroll', listener);

  return () => {
    clearTimeout(timer);
    window.removeEventListener('scroll', listener);
  };
}, []);
```

### Avoid Infinite Loops

Be careful with dependencies to prevent infinite render loops:  

```javascript
// ❌ Bad: Infinite loop
useEffect(() => {
  setCount(count + 1);
}, [count]);

// ✅ Good: Controlled updates
useEffect(() => {
  if (count < 10) {
    setCount(count + 1);
  }
}, [count]);
```

---

## Summary

The `useEffect` Hook is essential for managing side effects in React  
functional components. Key takeaways:  

- Use `useEffect` for data fetching, subscriptions, and DOM manipulation  
- The dependency array controls when effects run  
- Always include all used values in the dependency array  
- Return cleanup functions to prevent memory leaks  
- Keep effects focused on single concerns  
- Use functional updates to avoid unnecessary dependencies  
- Extract complex logic to custom hooks  

Mastering `useEffect` and understanding the dependency array are crucial  
skills for building robust React applications that efficiently manage  
resources and side effects.
