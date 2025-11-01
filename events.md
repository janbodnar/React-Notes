# Event Handling

React provides a consistent and declarative approach to handling user  
interactions through its synthetic event system. Events in React are  
normalized across different browsers, ensuring consistent behavior and  
eliminating many cross-browser compatibility issues. Unlike traditional DOM  
events, React events are named using camelCase and pass functions as event  
handlers rather than strings.  

The synthetic event system wraps native browser events in a consistent  
interface that works identically across all browsers. This abstraction means  
you don't need to worry about browser-specific quirks or polyfills. React  
events are pooled for performance—the event object is reused for multiple  
events, so accessing event properties asynchronously requires calling  
`event.persist()` or extracting the needed values immediately.  

Event handling in React follows a declarative pattern where you specify what  
should happen when an event occurs, rather than imperatively attaching event  
listeners. This approach integrates seamlessly with React's component model,  
keeping event handlers close to the JSX they affect. Events can trigger  
state updates, call APIs, navigate to different views, or perform any other  
logic needed for your application.  

Key concepts when working with events in React include:  

- **Synthetic Events**: React's cross-browser event wrapper that provides  
  consistent properties and methods  
- **Event Handlers**: Functions that execute when specific events occur,  
  typically defined as arrow functions or methods  
- **Event Object**: Contains information about the event, such as target  
  element, event type, and relevant data  
- **Event Bubbling**: Events propagate up through the component tree,  
  allowing parent components to handle child events  
- **Preventing Defaults**: Use `event.preventDefault()` to stop default  
  browser behavior like form submission or link navigation  
- **Stopping Propagation**: Use `event.stopPropagation()` to prevent events  
  from bubbling up to parent elements  

This document covers common event types in React, from simple clicks to  
complex form interactions, keyboard navigation, mouse movements, and touch  
gestures. Each section includes practical examples demonstrating best  
practices for handling events in modern React applications.  

---

## Click Events

Click events are the most common form of user interaction in web  
applications. React handles them through the `onClick` attribute.  

```jsx
import { useState } from 'react';

function ClickExample() {
  const [count, setCount] = useState(0);
  const [message, setMessage] = useState("");

  const handleClick = () => {
    setCount(count + 1);
  };

  const handleDoubleClick = () => {
    setCount(count * 2);
    setMessage("Count doubled!");
  };

  const handleRightClick = (event) => {
    event.preventDefault();
    setMessage("Right click detected!");
  };

  return (
    <div>
      <p>Count: {count}</p>
      <p>{message}</p>
      
      <button onClick={handleClick}>
        Increment
      </button>
      
      <button onDoubleClick={handleDoubleClick}>
        Double Count
      </button>
      
      <button onContextMenu={handleRightClick}>
        Right Click Me
      </button>
    </div>
  );
}
```

Click events respond to user clicks on interactive elements. The `onClick`  
handler fires on single clicks, while `onDoubleClick` responds to double  
clicks. The `onContextMenu` event captures right-click actions, often used  
to prevent default context menus or show custom menus. Each handler receives  
a synthetic event object containing information about the click. Use  
`event.preventDefault()` to stop default browser behaviors like form  
submission or link navigation. For complex logic, define handlers as  
separate named functions rather than inline arrow functions. This keeps JSX  
clean and makes testing easier.  

---

## Click Event with Event Object

Access detailed information about click events through the event object.  

```jsx
function ClickEventDetails() {
  const handleClickWithDetails = (event) => {
    console.log('Button clicked:', event.currentTarget.textContent);
    console.log('Click coordinates:', event.clientX, event.clientY);
    console.log('Shift key held:', event.shiftKey);
    console.log('Ctrl key held:', event.ctrlKey);
    console.log('Alt key held:', event.altKey);
  };

  const handleLinkClick = (event) => {
    event.preventDefault();
    console.log('Link navigation prevented');
    // Custom navigation logic here
  };

  return (
    <div>
      <button onClick={handleClickWithDetails}>
        Click for Details
      </button>
      
      <a href="/nowhere" onClick={handleLinkClick}>
        Click Me (Default Prevented)
      </a>
    </div>
  );
}
```

The event object provides extensive information about click interactions.  
The `currentTarget` property references the element with the event handler,  
while `target` references the element that triggered the event. Coordinate  
properties like `clientX` and `clientY` give the click position relative to  
the viewport. Modifier key booleans (`shiftKey`, `ctrlKey`, `altKey`)  
indicate which keys were held during the click, useful for implementing  
keyboard shortcuts or alternative behaviors. Always call  
`event.preventDefault()` when you need to override default browser actions  
like following links or submitting forms.  

---

## Form Submission Events

Form events manage data collection and submission in React applications.  

```jsx
import { useState } from 'react';

function FormSubmitExample() {
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    message: ""
  });
  const [submitted, setSubmitted] = useState(false);

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Form submitted:', formData);
    setSubmitted(true);
    
    // Simulate API call
    setTimeout(() => {
      setSubmitted(false);
      setFormData({ username: "", email: "", message: "" });
    }, 2000);
  };

  const handleInputChange = (event) => {
    const { name, value } = event.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="username"
        value={formData.username}
        onChange={handleInputChange}
        placeholder="Username"
        required
      />
      
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleInputChange}
        placeholder="Email"
        required
      />
      
      <textarea
        name="message"
        value={formData.message}
        onChange={handleInputChange}
        placeholder="Your message"
        rows={4}
      />
      
      <button type="submit">Submit</button>
      
      {submitted && <p>Form submitted successfully!</p>}
    </form>
  );
}
```

Form submission events handle data collection and processing. Always call  
`event.preventDefault()` in the `onSubmit` handler to prevent the default  
browser form submission that would reload the page. Use controlled  
components where each input's value is tied to state and updated via  
`onChange` handlers. The controlled component pattern gives React complete  
control over form data, enabling real-time validation, formatting, and  
conditional logic. Extract input properties using destructuring  
(`const { name, value } = event.target`) for cleaner code. Manage multiple  
inputs efficiently by using the input's `name` attribute to update the  
corresponding state property.  

---

## Input Change Events

Track input changes in real-time with onChange and onInput events.  

```jsx
import { useState } from 'react';

function InputChangeExample() {
  const [searchTerm, setSearchTerm] = useState("");
  const [charCount, setCharCount] = useState(0);
  const [password, setPassword] = useState("");

  const handleSearchChange = (event) => {
    const value = event.target.value;
    setSearchTerm(value);
    setCharCount(value.length);
  };

  const handlePasswordChange = (event) => {
    const value = event.target.value;
    setPassword(value);
    
    // Real-time validation
    if (value.length < 8) {
      console.log('Password too short');
    }
  };

  const handleInput = (event) => {
    // onInput fires on every input change
    console.log('Input event:', event.target.value);
  };

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={handleSearchChange}
        placeholder="Search..."
      />
      <p>Characters: {charCount}</p>
      
      <input
        type="password"
        value={password}
        onChange={handlePasswordChange}
        onInput={handleInput}
        placeholder="Password (min 8 chars)"
      />
      <p>Password strength: {password.length >= 8 ? 'Good' : 'Weak'}</p>
    </div>
  );
}
```

The `onChange` event fires whenever an input's value changes, perfect for  
controlled components and real-time updates. Access the new value via  
`event.target.value`. The `onInput` event is similar but fires before the  
input is processed, useful for immediate feedback. Use `onChange` for most  
cases as it's the standard React pattern for controlled inputs. Combine  
onChange with state updates to create dynamic, responsive forms. Real-time  
character counting, validation, and search filtering all rely on onChange  
handlers. For performance-sensitive scenarios with rapid updates, consider  
debouncing the handler to reduce state updates.  

---

## Keyboard Events

Keyboard events enable keyboard navigation and shortcuts in applications.  

```jsx
import { useState } from 'react';

function KeyboardEventExample() {
  const [inputValue, setInputValue] = useState("");
  const [pressedKey, setPressedKey] = useState("");
  const [items, setItems] = useState([]);

  const handleKeyDown = (event) => {
    setPressedKey(event.key);
    
    // Submit on Enter
    if (event.key === 'Enter') {
      event.preventDefault();
      if (inputValue.trim()) {
        setItems([...items, inputValue]);
        setInputValue("");
      }
    }
    
    // Clear on Escape
    if (event.key === 'Escape') {
      setInputValue("");
    }
    
    // Check for keyboard shortcuts
    if (event.ctrlKey && event.key === 's') {
      event.preventDefault();
      console.log('Save shortcut triggered');
    }
  };

  const handleKeyUp = (event) => {
    console.log('Key released:', event.key);
  };

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        onKeyDown={handleKeyDown}
        onKeyUp={handleKeyUp}
        placeholder="Type and press Enter"
      />
      
      <p>Last key pressed: {pressedKey}</p>
      
      <ul>
        {items.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

Keyboard events track key presses for navigation and shortcuts. The  
`onKeyDown` event fires when a key is pressed, `onKeyUp` when released.  
Access the pressed key via `event.key` (returns key name like 'Enter',  
'Escape') or `event.code` (returns physical key code like 'KeyA'). Check  
modifier keys using `event.ctrlKey`, `event.shiftKey`, `event.altKey`, or  
`event.metaKey` to implement keyboard shortcuts. Always call  
`event.preventDefault()` for shortcuts to prevent browser defaults like  
Ctrl+S saving the page. The `onKeyPress` event is deprecated; use  
`onKeyDown` instead for better browser support and consistent behavior.  

---

## Advanced Keyboard Navigation

Implement arrow key navigation and complex keyboard interactions.  

```jsx
import { useState, useRef } from 'react';

function KeyboardNavigationExample() {
  const [selectedIndex, setSelectedIndex] = useState(0);
  const items = ['First Item', 'Second Item', 'Third Item', 'Fourth Item'];
  const listRef = useRef(null);

  const handleKeyDown = (event) => {
    switch (event.key) {
      case 'ArrowDown':
        event.preventDefault();
        setSelectedIndex(prev => 
          prev < items.length - 1 ? prev + 1 : prev
        );
        break;
      
      case 'ArrowUp':
        event.preventDefault();
        setSelectedIndex(prev => prev > 0 ? prev - 1 : prev);
        break;
      
      case 'Home':
        event.preventDefault();
        setSelectedIndex(0);
        break;
      
      case 'End':
        event.preventDefault();
        setSelectedIndex(items.length - 1);
        break;
      
      case 'Enter':
        event.preventDefault();
        console.log('Selected:', items[selectedIndex]);
        break;
    }
  };

  return (
    <div 
      ref={listRef}
      onKeyDown={handleKeyDown}
      tabIndex={0}
      style={{ outline: 'none', padding: '10px' }}
    >
      <p>Use arrow keys to navigate, Enter to select</p>
      
      <ul>
        {items.map((item, index) => (
          <li 
            key={index}
            style={{
              backgroundColor: index === selectedIndex ? '#e0e0e0' : 'white',
              padding: '8px',
              cursor: 'pointer'
            }}
          >
            {item}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

Advanced keyboard navigation enables accessible, keyboard-friendly  
interfaces. Attach keyboard handlers to container elements and set  
`tabIndex={0}` to make them focusable. Use arrow keys (ArrowUp, ArrowDown,  
ArrowLeft, ArrowRight) for navigation, Home and End for jumping to  
boundaries, and Enter or Space for selection. Always prevent default  
behavior for navigation keys to stop page scrolling. Maintain selected state  
and provide visual feedback showing the current selection. This pattern is  
essential for accessible dropdowns, menus, lists, and custom form controls.  
Consider using `useRef` to manage focus programmatically when needed.  

---

## Focus and Blur Events

Focus events manage input focus state and validation timing.  

```jsx
import { useState } from 'react';

function FocusBlurExample() {
  const [email, setEmail] = useState("");
  const [emailError, setEmailError] = useState("");
  const [isFocused, setIsFocused] = useState(false);

  const handleFocus = (event) => {
    setIsFocused(true);
    setEmailError("");
    console.log('Input focused');
  };

  const handleBlur = (event) => {
    setIsFocused(false);
    
    // Validate on blur
    const value = event.target.value;
    if (value && !value.includes('@')) {
      setEmailError('Invalid email format');
    }
  };

  const handleInputChange = (event) => {
    setEmail(event.target.value);
    // Clear error while typing
    if (emailError) {
      setEmailError("");
    }
  };

  return (
    <div>
      <input
        type="email"
        value={email}
        onChange={handleInputChange}
        onFocus={handleFocus}
        onBlur={handleBlur}
        placeholder="Enter email"
        style={{
          borderColor: isFocused ? 'blue' : emailError ? 'red' : 'gray',
          outline: 'none'
        }}
      />
      
      {emailError && <p style={{ color: 'red' }}>{emailError}</p>}
      
      <p>Status: {isFocused ? 'Focused' : 'Not focused'}</p>
    </div>
  );
}
```

Focus events track when elements receive or lose focus. The `onFocus` event  
fires when an element becomes focused, typically by clicking or tabbing to  
it. The `onBlur` event fires when focus leaves the element. Use onBlur for  
validation, as it triggers after users finish editing a field. Clear error  
messages on focus to improve user experience. Provide visual feedback for  
focused states using CSS or inline styles. The onFocus and onBlur events are  
crucial for creating accessible forms that guide users through input  
requirements and validate data at appropriate times.  

---

## Focus Management with useRef

Control focus programmatically for better user experience.  

```jsx
import { useRef, useState } from 'react';

function FocusManagementExample() {
  const usernameRef = useRef(null);
  const emailRef = useRef(null);
  const passwordRef = useRef(null);
  const [step, setStep] = useState(1);

  const handleUsernameKeyDown = (event) => {
    if (event.key === 'Enter') {
      event.preventDefault();
      emailRef.current.focus();
      setStep(2);
    }
  };

  const handleEmailKeyDown = (event) => {
    if (event.key === 'Enter') {
      event.preventDefault();
      passwordRef.current.focus();
      setStep(3);
    }
  };

  const resetForm = () => {
    setStep(1);
    usernameRef.current.focus();
  };

  return (
    <div>
      <h3>Step {step} of 3</h3>
      
      <input
        ref={usernameRef}
        type="text"
        placeholder="Username"
        onKeyDown={handleUsernameKeyDown}
        autoFocus
      />
      
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
        onKeyDown={handleEmailKeyDown}
      />
      
      <input
        ref={passwordRef}
        type="password"
        placeholder="Password"
      />
      
      <button onClick={resetForm}>Reset</button>
    </div>
  );
}
```

Use `useRef` to manage focus programmatically. Create refs for input  
elements and attach them via the `ref` attribute. Call  
`ref.current.focus()` to focus an element imperatively. This pattern is  
useful for multi-step forms, error handling (focusing invalid fields), and  
improving keyboard navigation. The `autoFocus` attribute automatically  
focuses an element on mount, but use it sparingly as it can confuse users.  
Programmatic focus management creates smooth user flows where pressing Enter  
advances to the next field or form step.  

---

## Mouse Events

Mouse events track pointer movements and interactions beyond simple clicks.  

```jsx
import { useState } from 'react';

function MouseEventExample() {
  const [isHovering, setIsHovering] = useState(false);
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [enteredCount, setEnteredCount] = useState(0);

  const handleMouseEnter = () => {
    setIsHovering(true);
    setEnteredCount(prev => prev + 1);
  };

  const handleMouseLeave = () => {
    setIsHovering(false);
  };

  const handleMouseMove = (event) => {
    const rect = event.currentTarget.getBoundingClientRect();
    setPosition({
      x: event.clientX - rect.left,
      y: event.clientY - rect.top
    });
  };

  return (
    <div>
      <div
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        onMouseMove={handleMouseMove}
        style={{
          width: '300px',
          height: '200px',
          backgroundColor: isHovering ? '#e3f2fd' : '#f5f5f5',
          border: '2px solid #333',
          position: 'relative'
        }}
      >
        <p>Hover over this area</p>
        <p>Status: {isHovering ? 'Hovering' : 'Not hovering'}</p>
        <p>Mouse position: ({Math.round(position.x)}, {Math.round(position.y)})</p>
        <p>Entered {enteredCount} times</p>
      </div>
    </div>
  );
}
```

Mouse events provide fine-grained control over pointer interactions. The  
`onMouseEnter` event fires when the mouse enters an element, while  
`onMouseLeave` fires when it exits. Unlike `onMouseOver` and `onMouseOut`,  
these events don't bubble, making them ideal for hover states.  
`onMouseMove` tracks movement within an element, useful for custom cursors,  
drawing apps, or interactive visualizations. Access coordinates via  
`event.clientX` and `event.clientY`. For element-relative positions,  
subtract the element's bounding rect values. Be cautious with onMouseMove as  
it fires frequently—consider throttling or debouncing for performance.  

---

## Drag and Drop Events

Implement drag and drop functionality with mouse events.  

```jsx
import { useState } from 'react';

function DragDropExample() {
  const [draggedItem, setDraggedItem] = useState(null);
  const [items, setItems] = useState(['Item 1', 'Item 2', 'Item 3']);

  const handleDragStart = (event, index) => {
    setDraggedItem(index);
    event.dataTransfer.effectAllowed = 'move';
  };

  const handleDragOver = (event) => {
    event.preventDefault();
    event.dataTransfer.dropEffect = 'move';
  };

  const handleDrop = (event, dropIndex) => {
    event.preventDefault();
    
    if (draggedItem === null) return;
    
    const newItems = [...items];
    const [removed] = newItems.splice(draggedItem, 1);
    newItems.splice(dropIndex, 0, removed);
    
    setItems(newItems);
    setDraggedItem(null);
  };

  return (
    <div>
      <h3>Drag items to reorder</h3>
      {items.map((item, index) => (
        <div
          key={index}
          draggable
          onDragStart={(e) => handleDragStart(e, index)}
          onDragOver={handleDragOver}
          onDrop={(e) => handleDrop(e, index)}
          style={{
            padding: '10px',
            margin: '5px',
            backgroundColor: draggedItem === index ? '#bbdefb' : '#e0e0e0',
            cursor: 'move',
            userSelect: 'none'
          }}
        >
          {item}
        </div>
      ))}
    </div>
  );
}
```

Drag and drop events enable intuitive reordering and organization. Set  
`draggable` attribute to true on elements you want to drag. The  
`onDragStart` event fires when dragging begins—use it to store the dragged  
item's data. Call `event.preventDefault()` in `onDragOver` to allow  
dropping. The `onDrop` event handles the drop action, where you update state  
to reflect the new order. Use `event.dataTransfer` to pass data between drag  
and drop handlers. The drag and drop API is powerful but complex—consider  
using libraries like react-beautiful-dnd or dnd-kit for production apps with  
accessibility and mobile support.  

---

## Touch Events

Handle touch gestures for mobile-friendly interfaces.  

```jsx
import { useState } from 'react';

function TouchEventExample() {
  const [touchStart, setTouchStart] = useState(null);
  const [touchEnd, setTouchEnd] = useState(null);
  const [swipeDirection, setSwipeDirection] = useState("");
  const [tapCount, setTapCount] = useState(0);

  const minSwipeDistance = 50;

  const handleTouchStart = (event) => {
    setTouchEnd(null);
    setTouchStart(event.touches[0].clientX);
  };

  const handleTouchMove = (event) => {
    setTouchEnd(event.touches[0].clientX);
  };

  const handleTouchEnd = () => {
    if (!touchStart || !touchEnd) return;
    
    const distance = touchStart - touchEnd;
    const isLeftSwipe = distance > minSwipeDistance;
    const isRightSwipe = distance < -minSwipeDistance;
    
    if (isLeftSwipe) {
      setSwipeDirection('Left swipe detected');
    } else if (isRightSwipe) {
      setSwipeDirection('Right swipe detected');
    }
    
    setTouchStart(null);
    setTouchEnd(null);
  };

  const handleTap = () => {
    setTapCount(prev => prev + 1);
  };

  return (
    <div>
      <div
        onTouchStart={handleTouchStart}
        onTouchMove={handleTouchMove}
        onTouchEnd={handleTouchEnd}
        onClick={handleTap}
        style={{
          width: '100%',
          height: '200px',
          backgroundColor: '#e8f5e9',
          border: '2px solid #4caf50',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          flexDirection: 'column',
          userSelect: 'none',
          WebkitUserSelect: 'none'
        }}
      >
        <p>Swipe left or right</p>
        <p>{swipeDirection}</p>
        <p>Taps: {tapCount}</p>
      </div>
    </div>
  );
}
```

Touch events enable mobile gesture support. The `onTouchStart` event fires  
when a finger touches the screen, providing touch coordinates via  
`event.touches[0]`. Use `onTouchMove` to track movement as the finger drags  
across the screen. The `onTouchEnd` event fires when the finger lifts,  
perfect for completing gesture recognition. Implement swipe detection by  
comparing start and end positions—calculate distance and direction to  
determine the gesture. Touch events support multi-touch via the `touches`  
array. Remember to set `userSelect: 'none'` to prevent text selection during  
gestures. Touch events don't prevent click events, so mobile users trigger  
both. Handle this by using touch events exclusively on mobile or checking  
`event.type` to differentiate.  

---

## Multi-Touch Gestures

Implement pinch-to-zoom and multi-finger gestures.  

```jsx
import { useState } from 'react';

function MultiTouchExample() {
  const [scale, setScale] = useState(1);
  const [touches, setTouches] = useState(0);
  const [initialDistance, setInitialDistance] = useState(null);

  const getDistance = (touch1, touch2) => {
    const dx = touch1.clientX - touch2.clientX;
    const dy = touch1.clientY - touch2.clientY;
    return Math.sqrt(dx * dx + dy * dy);
  };

  const handleTouchStart = (event) => {
    const touchCount = event.touches.length;
    setTouches(touchCount);
    
    if (touchCount === 2) {
      const distance = getDistance(event.touches[0], event.touches[1]);
      setInitialDistance(distance);
    }
  };

  const handleTouchMove = (event) => {
    event.preventDefault();
    
    if (event.touches.length === 2 && initialDistance) {
      const currentDistance = getDistance(event.touches[0], event.touches[1]);
      const newScale = currentDistance / initialDistance;
      setScale(prevScale => Math.max(0.5, Math.min(prevScale * newScale, 3)));
      setInitialDistance(currentDistance);
    }
  };

  const handleTouchEnd = () => {
    setInitialDistance(null);
    setTouches(0);
  };

  return (
    <div
      onTouchStart={handleTouchStart}
      onTouchMove={handleTouchMove}
      onTouchEnd={handleTouchEnd}
      style={{
        width: '100%',
        height: '300px',
        backgroundColor: '#fff3e0',
        border: '2px solid #ff9800',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        overflow: 'hidden',
        touchAction: 'none'
      }}
    >
      <div
        style={{
          transform: `scale(${scale})`,
          transition: touches > 0 ? 'none' : 'transform 0.2s',
          userSelect: 'none'
        }}
      >
        <h2>Pinch to zoom</h2>
        <p>Current scale: {scale.toFixed(2)}x</p>
        <p>Touches: {touches}</p>
      </div>
    </div>
  );
}
```

Multi-touch gestures enable sophisticated mobile interactions. Detect pinch  
gestures by measuring distance between two touch points. Calculate distance  
using the Pythagorean theorem from touch coordinates. Track initial distance  
on touch start, then compare with current distance during movement to  
determine zoom level. Limit scale values using `Math.min` and `Math.max` to  
prevent excessive zooming. Set `touchAction: 'none'` to prevent default  
browser touch behaviors like scrolling or zooming. Multi-touch support  
requires careful state management—track both touch count and gesture data.  
For production apps with complex gestures, consider libraries like  
Hammer.js or react-use-gesture.  

---

## Event Delegation and Bubbling

Leverage event bubbling for efficient event handling.  

```jsx
import { useState } from 'react';

function EventDelegationExample() {
  const [clickedItem, setClickedItem] = useState("");
  const [events, setEvents] = useState([]);

  const logEvent = (message) => {
    setEvents(prev => [...prev.slice(-4), message]);
  };

  const handleContainerClick = (event) => {
    logEvent(`Container clicked: ${event.target.tagName}`);
    
    if (event.target.tagName === 'BUTTON') {
      const action = event.target.dataset.action;
      setClickedItem(action);
      logEvent(`Button action: ${action}`);
    }
  };

  const handleInnerClick = (event) => {
    logEvent('Inner div clicked');
    // Event will bubble to container
  };

  const handleStopPropagation = (event) => {
    event.stopPropagation();
    logEvent('Propagation stopped!');
  };

  return (
    <div onClick={handleContainerClick} style={{ padding: '20px', backgroundColor: '#f5f5f5' }}>
      <h3>Event Delegation Demo</h3>
      
      <button data-action="save">Save</button>
      <button data-action="delete">Delete</button>
      <button data-action="cancel">Cancel</button>
      
      <div onClick={handleInnerClick} style={{ margin: '10px', padding: '10px', backgroundColor: '#e0e0e0' }}>
        Inner div (bubbles up)
      </div>
      
      <button onClick={handleStopPropagation}>
        Click me (stops bubbling)
      </button>
      
      <div>
        <p>Last clicked: {clickedItem}</p>
        <p>Event log:</p>
        <ul>
          {events.map((event, index) => (
            <li key={index}>{event}</li>
          ))}
        </ul>
      </div>
    </div>
  );
}
```

Event bubbling propagates events from child elements up to parent elements.  
This enables event delegation where a single handler on a parent manages  
events from multiple children. Check `event.target` to identify which child  
triggered the event. Use data attributes (`data-action`) to store metadata  
on elements for easy access in handlers. Call `event.stopPropagation()` to  
prevent events from bubbling further up the tree. Event delegation reduces  
memory usage and improves performance when handling many similar elements  
like list items or buttons. It also automatically handles dynamically added  
elements without attaching new listeners.  

---

## Preventing Default Behavior

Control browser default actions with event.preventDefault().  

```jsx
import { useState } from 'react';

function PreventDefaultExample() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault(); // Prevent page reload
    
    if (input.trim()) {
      setTodos([...todos, input]);
      setInput("");
    }
  };

  const handleLinkClick = (event) => {
    event.preventDefault();
    console.log('Navigation prevented - custom behavior here');
  };

  const handleContextMenu = (event) => {
    event.preventDefault();
    console.log('Custom context menu would appear here');
  };

  const handleKeyDown = (event) => {
    // Prevent certain characters
    if (event.key === 'Enter' && event.ctrlKey) {
      event.preventDefault();
      console.log('Ctrl+Enter prevented');
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={handleKeyDown}
          placeholder="Add todo"
        />
        <button type="submit">Add</button>
      </form>
      
      <a href="/somewhere" onClick={handleLinkClick}>
        Link (default prevented)
      </a>
      
      <div onContextMenu={handleContextMenu} style={{ padding: '20px', backgroundColor: '#f0f0f0', marginTop: '10px' }}>
        Right-click me
      </div>
      
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
    </div>
  );
}
```

Preventing default behavior lets you override browser actions. Call  
`event.preventDefault()` in form submit handlers to prevent page reloads and  
handle submission in JavaScript. Use it in link click handlers to implement  
client-side routing or confirmation dialogs. Context menu events can be  
prevented to show custom menus. Keyboard events often need prevention for  
shortcuts that conflict with browser defaults. Always call preventDefault()  
early in the handler before conditional logic to ensure it executes. This  
method doesn't stop event propagation—use `event.stopPropagation()` for  
that. Combining both provides full control over event flow.  

---

## Async Event Handlers

Handle asynchronous operations in event handlers safely.  

```jsx
import { useState } from 'react';

function AsyncEventHandlerExample() {
  const [isLoading, setIsLoading] = useState(false);
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  const handleAsyncClick = async (event) => {
    event.preventDefault();
    setIsLoading(true);
    setError(null);
    
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      
      const mockData = {
        id: Date.now(),
        message: "Data loaded successfully!"
      };
      
      setData(mockData);
    } catch (err) {
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  };

  const handleFormSubmit = async (event) => {
    event.preventDefault();
    
    // Extract form data
    const formData = new FormData(event.target);
    const values = Object.fromEntries(formData.entries());
    
    setIsLoading(true);
    
    try {
      // Simulate API submission
      await new Promise(resolve => setTimeout(resolve, 1500));
      console.log('Submitted:', values);
      event.target.reset();
    } catch (err) {
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div>
      <button onClick={handleAsyncClick} disabled={isLoading}>
        {isLoading ? 'Loading...' : 'Fetch Data'}
      </button>
      
      {data && <p>{data.message}</p>}
      {error && <p style={{ color: 'red' }}>{error}</p>}
      
      <form onSubmit={handleFormSubmit}>
        <input type="text" name="username" placeholder="Username" required />
        <input type="email" name="email" placeholder="Email" required />
        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Submitting...' : 'Submit'}
        </button>
      </form>
    </div>
  );
}
```

Async event handlers manage asynchronous operations like API calls. Declare  
handlers as `async` functions to use `await`. Call `event.preventDefault()`  
before any async operations to ensure it executes synchronously. Track  
loading state to disable buttons and prevent duplicate submissions. Use  
try-catch-finally blocks to handle errors and cleanup consistently. The  
event object properties are nullified after the handler completes, so  
extract needed values before async operations. For form submissions, use  
FormData API to easily gather input values. Always provide user feedback  
during async operations through loading indicators and error messages.  

---
