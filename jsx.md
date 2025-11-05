# JSX in React

JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to  
write HTML-like code within JavaScript. It was developed by Facebook as part  
of React to make UI development more intuitive and declarative. JSX combines  
the power of JavaScript with the familiarity of HTML markup, enabling  
developers to describe what the UI should look like in a more readable and  
maintainable way.  

JSX is not required for React development, but it has become the standard  
because of its numerous benefits. It provides a visual structure that closely  
resembles the final rendered output, making it easier to understand component  
hierarchies at a glance. JSX also enables type checking and better error  
messages during compilation, catching mistakes early in the development  
process.  

Under the hood, JSX is transformed into regular JavaScript function calls.  
Each JSX element becomes a `React.createElement()` call, which creates a  
React element object. This transformation happens during the build process,  
typically using tools like Babel or TypeScript. Understanding this  
transformation helps developers appreciate how JSX works and debug issues  
more effectively.  

Key concepts to understand when working with JSX include:  

- **JavaScript Expressions**: JSX allows embedding any JavaScript expression  
  within curly braces `{}`, enabling dynamic content rendering  
- **Attributes**: JSX uses camelCase naming for attributes (e.g.,  
  `className` instead of `class`, `onClick` instead of `onclick`)  
- **Children**: Elements can contain other elements or expressions as  
  children, creating component hierarchies  
- **Fragments**: React provides fragments to group multiple elements without  
  adding extra DOM nodes  
- **Self-Closing Tags**: Elements without children must use self-closing  
  syntax `<Element />`  

This document provides 30 progressive examples demonstrating JSX concepts  
from basic syntax to advanced patterns, helping you master this essential  
React feature.  

---

## Basic JSX Element

Creating a simple JSX element is the foundation of React development.  

```tsx
import type { JSX } from "react";

export default function BasicElement(): JSX.Element {
  return <div>Hello there!</div>;
}
```

This example demonstrates the most basic JSX syntax. The function returns a  
single `div` element containing text. JSX looks like HTML but is actually  
JavaScript. During build time, this gets transformed into  
`React.createElement('div', null, 'Hello there!')`. Notice how the element is  
returned directly without quotes, distinguishing JSX from strings.  

---

## Embedding JavaScript Expressions

JSX allows you to embed JavaScript expressions using curly braces.  

```tsx
import type { JSX } from "react";

export default function Expressions(): JSX.Element {
	const name: string = "React Developer";
	const age: number = 25;

	return (
		<div>
			<p>Name: {name}</p>
			<p>Age: {age}</p>
			<p>Next year: {age + 1}</p>
		</div>
	);
}
```

This example shows how to embed JavaScript variables and expressions within  
JSX using curly braces `{}`. Any valid JavaScript expression can be placed  
inside these braces, including variables, arithmetic operations, function  
calls, and more. The expressions are evaluated and their results are rendered  
in the UI. This makes JSX dynamic and powerful for creating interactive  
interfaces.  

---

## JSX with Attributes

JSX elements can have attributes similar to HTML, but with some differences.  

```tsx
import type { JSX } from "react";

export default function Attribute(): JSX.Element {
	const imageUrl: string = "https://picsum.photos/150";
	const altText: string = "Placeholder image";

	return (
		<img src={imageUrl} alt={altText} className="profile-image" width="150" />
	);
}
```

Attributes in JSX use camelCase naming convention. The `class` attribute  
becomes `className`, and `for` becomes `htmlFor` to avoid conflicts with  
JavaScript reserved keywords. String literals can be passed with quotes, while  
JavaScript expressions use curly braces. Self-closing tags must end with `/>`.  
This example demonstrates both static string attributes (`width`) and dynamic  
expression attributes (`src`, `alt`).  

---

## Multiple Elements with Fragments

When returning multiple elements, use React Fragments to avoid extra DOM nodes.  

```tsx
import type { JSX } from "react";

export default function FragmentExample(): JSX.Element {
  return (
    <>
      <h1>Main Title</h1>
      <p>First paragraph</p>
      <p>Second paragraph</p>
    </>
  );
}
```

React components must return a single root element. Instead of wrapping  
multiple elements in an unnecessary `div`, use the short fragment syntax  
`<>...</>` or the explicit `<React.Fragment>...</React.Fragment>`. Fragments  
let you group multiple elements without adding extra nodes to the DOM, keeping  
your HTML structure clean. This is particularly useful when creating lists or  
working with CSS Grid and Flexbox layouts.  

---

## Conditional Rendering with Ternary Operator

JSX supports conditional rendering using JavaScript's ternary operator.  

```tsx
import type { JSX } from "react";

export default function ConditionalExample(): JSX.Element {
  const isLoggedIn: boolean = true;
  
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome back!</h1>
      ) : (
        <h1>Please sign in</h1>
      )}
    </div>
  );
}
```

The ternary operator `condition ? trueValue : falseValue` is commonly used  
for conditional rendering in JSX. This example shows how to render different  
content based on a boolean condition. When `isLoggedIn` is `true`, it renders  
a welcome message; otherwise, it shows a sign-in prompt. Parentheses around  
JSX elements in the ternary expression improve readability when dealing with  
multi-line content.  

---

## Conditional Rendering with Logical AND

Use the logical AND operator for simpler conditional rendering.  

```tsx
import type { JSX } from "react";

function LogicalAndExample(): JSX.Element {
  const hasNotifications: boolean = true;
  const notificationCount: number = 5;
  
  return (
    <div>
      <h1>Dashboard</h1>
      {hasNotifications && (
        <p>You have {notificationCount} new notifications</p>
      )}
    </div>
  );
}
```

The logical AND operator `&&` provides a concise way to conditionally render  
elements. If the left side is truthy, React renders the right side; otherwise,  
nothing is rendered. This pattern is cleaner than ternary operators when you  
only need to show something conditionally without an else case. Be careful  
with falsy values like `0`, which will be rendered. Use explicit boolean  
conversions when necessary.  

---

## Rendering Arrays with map()

Transform arrays into lists of JSX elements using the map() method.  

```tsx
import type { JSX } from "react";

export default function ArrayMap(): JSX.Element {
	const fruits: string[] = ["Apple", "Banana", "Orange", "Mango"];

	return (
		<ul>
			{fruits.map((fruit, index) => (
				// biome-ignore lint/suspicious/noArrayIndexKey: <testing>
				<li key={index}>{fruit}</li>
			))}
		</ul>
	);
}
```

The `map()` method is the standard way to render lists in React. It  
transforms each array item into a JSX element. Every element in the array  
needs a unique `key` prop to help React identify which items have changed.  
While using array indices as keys works for static lists, prefer using unique  
identifiers for dynamic data. The key prop improves rendering performance and  
prevents subtle bugs when lists are modified.  

---

## Rendering Complex Objects

Render data from arrays of objects with multiple properties.  

```tsx
import type { JSX } from "react";

type User = {
  id: number;
  name: string;
  role: string;
};

function ComplexObjectExample(): JSX.Element {
  const users: User[] = [
    { id: 1, name: "Alice Johnson", role: "Developer" },
    { id: 2, name: "Bob Smith", role: "Designer" },
    { id: 3, name: "Carol White", role: "Manager" }
  ];
  
  return (
    <div>
      {users.map((user) => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>Role: {user.role}</p>
        </div>
      ))}
    </div>
  );
}
```

This example demonstrates rendering complex data structures. Each object in  
the array has multiple properties that are accessed and displayed. Using the  
unique `id` field as the `key` is a best practice for object arrays. The  
structure shows how to create reusable card-like components from data,  
combining multiple JSX elements within each mapped iteration.  

---

## Event Handling

JSX makes it easy to attach event handlers to elements.  

```tsx
import React, { ChangeEvent } from 'react';

function EventHandlingExample(): JSX.Element {
  const handleClick = (): void => {
    console.log("Button clicked!");
  };
  
  const handleInputChange = (event: ChangeEvent<HTMLInputElement>): void => {
    console.log("Input value:", event.target.value);
  };
  
  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
      <input type="text" onChange={handleInputChange} />
    </div>
  );
}
```

Event handlers in JSX use camelCase naming (`onClick`, `onChange`) and accept  
function references, not function calls. Pass the function without  
parentheses: `onClick={handleClick}` not `onClick={handleClick()}`. Event  
handlers receive a synthetic event object that wraps the native browser event,  
providing cross-browser consistency. Access DOM values through  
`event.target.value` or other event properties.  

---

## Inline Event Handlers

Define event handlers directly within JSX for simple operations.  

```tsx
import React, { MouseEvent } from 'react';

function InlineEventExample(): JSX.Element {
  return (
    <div>
      <button onClick={() => console.log("Inline handler executed")}>
        Quick Action
      </button>
      <button onClick={(e: MouseEvent<HTMLButtonElement>) => {
        e.preventDefault();
        console.log("Event prevented");
      }}>
        Prevent Default
      </button>
    </div>
  );
}
```

Arrow functions can be defined inline for simple event handlers. This approach  
is convenient for one-liners or when you need to pass arguments to a function.  
However, be aware that inline arrow functions create a new function on each  
render, which can impact performance in large lists. For complex logic or  
frequently rendered components, define handlers outside the JSX as separate  
functions.  

---

## Inline Styles

Apply styles directly to JSX elements using JavaScript objects.  

```tsx
import React, { CSSProperties } from 'react';

function InlineStyleExample(): JSX.Element {
  const containerStyle: CSSProperties = {
    backgroundColor: "#f0f0f0",
    padding: "20px",
    borderRadius: "8px"
  };
  
  return (
    <div style={containerStyle}>
      <p style={{ color: "blue", fontSize: "18px" }}>Styled text</p>
    </div>
  );
}
```

Inline styles in JSX use JavaScript objects with camelCase property names  
(`backgroundColor` instead of `background-color`). Values are typically  
strings, but numeric values for pixels don't require units. Double curly  
braces `{{ }}` represent an object inside JSX expression braces. While inline  
styles are useful for dynamic styling, CSS classes are generally preferred for  
static styles and better performance.  

---

## className for CSS Classes

Use className to apply CSS classes to JSX elements.  

```tsx
import type { JSX } from "react";

function ClassNameExample(): JSX.Element {
  const isActive: boolean = true;
  const isPrimary: boolean = false;
  
  const buttonClass: string = `btn ${isActive ? 'active' : ''} ${isPrimary ? 'primary' : 'secondary'}`;
  
  return (
    <div className="container">
      <button className={buttonClass}>Styled Button</button>
    </div>
  );
}
```

The `className` attribute applies CSS classes to JSX elements. Use template  
literals to conditionally combine multiple classes based on component state.  
This example shows dynamic class assignment using ternary operators within  
template strings. For complex class logic, consider using libraries like  
`classnames` or `clsx` that provide cleaner syntax for conditional class  
management.  

---

## Nested Components

Compose components by nesting them within each other.  

```tsx
import type { JSX } from "react";

type AvatarProps = {
  name: string;
};

function Avatar({ name }: AvatarProps): JSX.Element {
  return <div className="avatar">{name.charAt(0)}</div>;
}

type UserCardProps = {
  name: string;
  email: string;
};

function UserCard({ name, email }: UserCardProps): JSX.Element {
  return (
    <div className="card">
      <Avatar name={name} />
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}

function NestedComponentExample(): JSX.Element {
  return (
    <div>
      <UserCard name="Jane Doe" email="jane@example.com" />
      <UserCard name="John Smith" email="john@example.com" />
    </div>
  );
}
```

Component composition is a fundamental React pattern. Components can render  
other components by including them in their JSX. Props flow down from parent  
to child components, creating a data flow hierarchy. This example shows a  
`UserCard` component that includes an `Avatar` component, demonstrating how  
complex UIs are built from smaller, reusable pieces.  

---

## Children Prop

Components can render content passed between opening and closing tags.  

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

function ChildrenExample(): JSX.Element {
  return (
    <Card title="User Profile">
      <p>Name: Alice Johnson</p>
      <p>Location: San Francisco</p>
      <button>Edit Profile</button>
    </Card>
  );
}
```

The special `children` prop contains any content placed between a component's  
opening and closing tags. This enables flexible component composition where  
parent components can wrap arbitrary content. The `Card` component here acts  
as a reusable container, applying consistent styling while allowing varied  
content. Children can be text, elements, components, or any valid JSX.  

---

## Conditional List Rendering

Combine conditional logic with list rendering for dynamic UIs.  

```tsx
import type { JSX } from "react";

type Task = {
  id: number;
  text: string;
  completed: boolean;
};

function ConditionalListExample(): JSX.Element {
  const tasks: Task[] = [
    { id: 1, text: "Review code", completed: true },
    { id: 2, text: "Write tests", completed: false },
    { id: 3, text: "Update docs", completed: false }
  ];
  
  const incompleteTasks = tasks.filter(task => !task.completed);
  
  return (
    <div>
      <h2>Pending Tasks</h2>
      {incompleteTasks.length > 0 ? (
        <ul>
          {incompleteTasks.map(task => (
            <li key={task.id}>{task.text}</li>
          ))}
        </ul>
      ) : (
        <p>No pending tasks!</p>
      )}
    </div>
  );
}
```

This example combines array filtering, conditional rendering, and mapping to  
create dynamic lists. First, the array is filtered to get incomplete tasks,  
then a ternary operator checks if any tasks exist. If tasks are present, they  
are mapped to list items; otherwise, a message is shown. This pattern is  
common in real applications for displaying filtered or search results.  

---

## Comments in JSX

Add comments within JSX using JavaScript comment syntax.  

```tsx
import type { JSX } from "react";

function CommentsExample(): JSX.Element {
  return (
    <div>
      {/* This is a JSX comment */}
      <h1>Main Title</h1>
      
      {/* 
        Multi-line comment
        in JSX
      */}
      <p>Content here</p>
      
      {/* Comments can explain complex JSX */}
      <div className="complex-section">
        {/* Nested comment */}
        <span>Nested content</span>
      </div>
    </div>
  );
}
```

Comments in JSX must be wrapped in curly braces and use JavaScript comment  
syntax `{/* comment */}`. Regular HTML comments `<!-- -->` won't work in JSX.  
Multi-line comments follow the same pattern with `/* */` inside braces. Place  
comments on their own lines for better readability. They're useful for  
documenting complex JSX structures or temporarily disabling code during  
development.  

---

## Spreading Props

Use the spread operator to pass multiple props efficiently.  

```tsx
import type { JSX } from "react";

type UserInfoProps = {
  name: string;
  email: string;
  age: number;
  location: string;
};

function UserInfo({ name, email, age, location }: UserInfoProps): JSX.Element {
  return (
    <div>
      <p>Name: {name}</p>
      <p>Email: {email}</p>
      <p>Age: {age}</p>
      <p>Location: {location}</p>
    </div>
  );
}

function SpreadPropsExample(): JSX.Element {
  const userDetails: UserInfoProps = {
    name: "Sarah Connor",
    email: "sarah@example.com",
    age: 30,
    location: "Los Angeles"
  };
  
  return <UserInfo {...userDetails} />;
}
```

The spread operator `{...object}` passes all object properties as individual  
props. This is cleaner than manually specifying each prop when dealing with  
many properties. The spread syntax unpacks the object, so `{...userDetails}`  
becomes `name="Sarah Connor" email="sarah@example.com"` etc. This pattern is  
especially useful when passing data from APIs or state management.  

---

## Combining Spread with Additional Props

Override or add props when using the spread operator.  

```tsx
import React, { ReactNode, ButtonHTMLAttributes } from 'react';

type ButtonProps = {
  variant?: 'default' | 'primary' | 'secondary';
  size?: 'small' | 'medium' | 'large';
  children: ReactNode;
} & ButtonHTMLAttributes<HTMLButtonElement>;

function Button({ variant = "default", size = "medium", children, ...rest }: ButtonProps): JSX.Element {
  const className = `btn btn-${variant} btn-${size}`;
  
  return (
    <button className={className} {...rest}>
      {children}
    </button>
  );
}

function CombinedPropsExample(): JSX.Element {
  const baseProps = {
    type: "button",
    "aria-label": "Submit form"
  };
  
  return (
    <Button {...baseProps} variant="primary" onClick={() => console.log("Clicked")}>
      Submit
    </Button>
  );
}
```

Props can be combined using spread operators with explicitly defined props.  
Props listed after the spread operator override spread values, while props  
before are overridden by explicit ones. The rest parameter `...rest` collects  
remaining props, useful for passing through HTML attributes. This enables  
flexible component APIs that accept standard HTML attributes while adding  
custom functionality.  

---

## Boolean Attributes

Handle boolean attributes with shorthand syntax.  

```tsx
import type { JSX } from "react";

function BooleanAttributeExample(): JSX.Element {
  const isRequired: boolean = true;
  const isDisabled: boolean = false;
  
  return (
    <form>
      <input 
        type="text" 
        required={isRequired}
        disabled={isDisabled}
      />
      
      {/* Shorthand for true values */}
      <input type="checkbox" checked readOnly />
      
      {/* Conditional boolean attribute */}
      <button disabled={!isRequired}>
        Submit
      </button>
    </form>
  );
}
```

Boolean attributes in JSX can be set to `true` by including just the attribute  
name without a value. For dynamic values, use curly braces with boolean  
expressions. When the value is `false`, the attribute is omitted from the  
rendered HTML. This example shows various ways to handle boolean attributes,  
including static true values, expressions, and conditional logic.  

---

## Fragments with Keys

Use explicit Fragment syntax when you need to add keys to fragments.  

```tsx
import type { JSX } from "react";

type GlossaryItem = {
  id: number;
  term: string;
  definition: string;
};

function FragmentWithKeysExample(): JSX.Element {
  const glossary: GlossaryItem[] = [
    { id: 1, term: "JSX", definition: "JavaScript XML syntax extension" },
    { id: 2, term: "Props", definition: "Component properties" },
    { id: 3, term: "State", definition: "Component data that changes" }
  ];
  
  return (
    <dl>
      {glossary.map((item) => (
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.definition}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

When mapping over arrays and returning multiple elements per iteration, use  
`<React.Fragment key={...}>` instead of the short syntax `<>`. Fragments need  
keys just like other elements in lists to help React track changes. This  
pattern is useful for definition lists, tables, or any structure requiring  
multiple sibling elements per array item without wrapper divs.  

---

## Null and Undefined Rendering

Understand how JSX handles null, undefined, and other falsy values.  

```tsx
import type { JSX } from "react";

function NullRenderingExample(): JSX.Element {
  const nullValue = null;
  const undefinedValue = undefined;
  const falseValue = false;
  const zeroValue = 0;
  const emptyString = "";
  
  return (
    <div>
      <p>Null: {nullValue}</p>
      <p>Undefined: {undefinedValue}</p>
      <p>False: {falseValue}</p>
      <p>Zero: {zeroValue}</p>
      <p>Empty: {emptyString}</p>
    </div>
  );
}
```

JSX renders `null`, `undefined`, `false`, and `true` as nothing (empty). The  
number `0` and empty strings are rendered as-is. This behavior is important  
for conditional rendering—using `&&` with numbers might unexpectedly display  
`0`. To avoid this, use explicit boolean conversions: `{count > 0 && <p>Count:  
{count}</p>}` instead of `{count && <p>Count: {count}</p>}`.  

---

## Dynamic Tag Names

Render different HTML elements based on variables.  

```tsx
import React, { ReactNode } from 'react';

type DynamicTagProps = {
  level?: 1 | 2 | 3 | 4 | 5 | 6;
  children: ReactNode;
};

function DynamicTagExample({ level = 1, children }: DynamicTagProps): JSX.Element {
  const Tag = `h${level}` as keyof JSX.IntrinsicElements;
  
  return <Tag>{children}</Tag>;
}

function DynamicTagsDemo(): JSX.Element {
  return (
    <div>
      <DynamicTagExample level={1}>Main Heading</DynamicTagExample>
      <DynamicTagExample level={2}>Subheading</DynamicTagExample>
      <DynamicTagExample level={3}>Minor Heading</DynamicTagExample>
    </div>
  );
}
```

JSX element types can be dynamic by storing tag names in variables. The  
variable must start with a capital letter to distinguish it from HTML tags.  
This pattern is useful for creating flexible components that adapt their HTML  
structure based on props. Common use cases include heading levels, buttons vs  
links, or different container types based on context.  

---

## Input Handling with State

Manage form inputs using React state and controlled components.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

function InputHandlingExample(): JSX.Element {
  const [name, setName] = useState<string>("");
  const [email, setEmail] = useState<string>("");
  
  const handleSubmit = (e: FormEvent<HTMLFormElement>): void => {
    e.preventDefault();
    console.log({ name, email });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setName(e.target.value)}
        placeholder="Name"
      />
      <input
        type="email"
        value={email}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

Controlled components sync input values with React state. The input's `value`  
prop is set to state, and `onChange` updates the state, creating a controlled  
loop. This gives React complete control over input values, enabling  
validation, formatting, and conditional logic. The form's `onSubmit` handler  
prevents default submission and processes the data in JavaScript.  

---

## Checkbox and Radio Inputs

Handle checkbox and radio button inputs with proper JSX syntax.  

```tsx
import React, { useState, ChangeEvent } from 'react';

function CheckboxRadioExample(): JSX.Element {
  const [agreed, setAgreed] = useState<boolean>(false);
  const [color, setColor] = useState<string>("blue");
  
  return (
    <div>
      <label>
        <input
          type="checkbox"
          checked={agreed}
          onChange={(e: ChangeEvent<HTMLInputElement>) => setAgreed(e.target.checked)}
        />
        I agree to terms
      </label>
      
      <div>
        {["red", "blue", "green"].map((c) => (
          <label key={c}>
            <input
              type="radio"
              value={c}
              checked={color === c}
              onChange={(e: ChangeEvent<HTMLInputElement>) => setColor(e.target.value)}
            />
            {c}
          </label>
        ))}
      </div>
    </div>
  );
}
```

Checkboxes use the `checked` attribute and `e.target.checked` to get boolean  
values. Radio buttons also use `checked` but compare the current state to each  
button's value. Labels can wrap inputs for better accessibility, creating  
clickable areas larger than the input itself. The `value` attribute on radio  
inputs identifies which option is selected.  

---

## Select Dropdown

Create dropdown selects with JSX and state management.  

```tsx
import React, { useState, ChangeEvent } from 'react';

type Country = {
  code: string;
  name: string;
};

function SelectExample(): JSX.Element {
  const [country, setCountry] = useState<string>("usa");
  
  const countries: Country[] = [
    { code: "usa", name: "United States" },
    { code: "uk", name: "United Kingdom" },
    { code: "canada", name: "Canada" }
  ];
  
  return (
    <div>
      <select value={country} onChange={(e: ChangeEvent<HTMLSelectElement>) => setCountry(e.target.value)}>
        {countries.map((c) => (
          <option key={c.code} value={c.code}>
            {c.name}
          </option>
        ))}
      </select>
      <p>Selected: {country}</p>
    </div>
  );
}
```

Select elements in React use the `value` prop on the `<select>` tag rather  
than `selected` on `<option>` tags. This makes controlled selects consistent  
with other inputs. Map over an array to generate options dynamically. The  
`onChange` handler receives the selected value through `e.target.value`. Each  
option needs a unique `key` prop when generated from an array.  

---

## Textarea Element

Handle multi-line text input using the textarea element.  

```tsx
import React, { useState, ChangeEvent } from 'react';

function TextareaExample(): JSX.Element {
  const [message, setMessage] = useState<string>("");
  const maxLength = 200;
  
  return (
    <div>
      <textarea
        value={message}
        onChange={(e: ChangeEvent<HTMLTextAreaElement>) => setMessage(e.target.value)}
        placeholder="Enter your message"
        rows={5}
        maxLength={maxLength}
      />
      <p>Characters: {message.length}/{maxLength}</p>
    </div>
  );
}
```

Unlike HTML, React's `<textarea>` uses a `value` prop instead of child  
content. This makes it consistent with other form inputs and easier to manage  
as a controlled component. The `rows` attribute sets visible height, and  
`maxLength` limits character count. Tracking `message.length` provides  
real-time character count feedback, a common UX pattern for text inputs.  

---

## Dangerously Set Inner HTML

Render raw HTML content with proper sanitization awareness.  

```tsx
import type { JSX } from "react";

function DangerouslySetInnerHTMLExample(): JSX.Element {
  const htmlContent = {
    __html: "<strong>Bold text</strong> and <em>italic text</em>"
  };
  
  const userContent: string = "<script>alert('XSS')</script>";
  
  return (
    <div>
      {/* Safe: controlled HTML */}
      <div dangerouslySetInnerHTML={htmlContent} />
      
      {/* UNSAFE: never use with user input */}
      {/* <div dangerouslySetInnerHTML={{ __html: userContent }} /> */}
    </div>
  );
}
```

The `dangerouslySetInnerHTML` prop renders raw HTML strings. It requires an  
object with an `__html` key, a deliberate API design to remind developers of  
XSS risks. Only use this with trusted, sanitized content—never with user  
input. The name and API are intentionally verbose to prevent accidental  
misuse. For user-generated content, use libraries like DOMPurify to sanitize  
HTML before rendering.  

---

## Portals for Modal Rendering

Use portals to render components outside the normal DOM hierarchy.  

```tsx
import React, { useState, ReactNode, MouseEvent } from 'react';
import { createPortal } from 'react-dom';

type ModalProps = {
  children: ReactNode;
  onClose: () => void;
};

function Modal({ children, onClose }: ModalProps): React.ReactPortal {
  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={(e: MouseEvent) => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.body
  );
}

function PortalExample(): JSX.Element {
  const [showModal, setShowModal] = useState<boolean>(false);
  
  return (
    <div>
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      {showModal && (
        <Modal onClose={() => setShowModal(false)}>
          <h2>Modal Content</h2>
          <p>This renders outside the parent DOM hierarchy</p>
        </Modal>
      )}
    </div>
  );
}
```

Portals render children into a different DOM node while maintaining the React  
component tree. This is essential for modals, tooltips, and dropdowns that  
need to break out of parent containers with overflow or z-index constraints.  
The `createPortal` function takes JSX and a DOM node. Events still bubble  
through the React tree, not the DOM tree, preserving expected behavior.  

---

## Error Boundaries with JSX

Implement error boundaries to gracefully handle component errors.  

```tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

interface Props {
  children: ReactNode;
}

interface State {
  hasError: boolean;
}

class ErrorBoundary extends Component<Props, State> {
  public state: State = {
    hasError: false
  };

  public static getDerivedStateFromError(_: Error): State {
    return { hasError: true };
  }

  public componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error("Uncaught error:", error, errorInfo);
  }

  public render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}


function BuggyComponent(): JSX.Element {
  throw new Error("Component crashed!");
}

function ErrorBoundaryExample(): JSX.Element {
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}
```

Error boundaries catch JavaScript errors in child components, log them, and  
display fallback UI instead of crashing the entire app. They must be class  
components implementing `getDerivedStateFromError` or `componentDidCatch`.  
Error boundaries don't catch errors in event handlers, async code, or the  
boundary itself. Wrap components that might fail with error boundaries to  
improve app resilience.  

---

## Lazy Loading Components

Use React.lazy and Suspense for code splitting with JSX.  

```tsx
import React, { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function LazyLoadingExample(): JSX.Element {
  return (
    <div>
      <h1>Main Content</h1>
      
      <Suspense fallback={<div>Loading component...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}
```

`React.lazy` enables code splitting by loading components dynamically. Wrap  
lazy components in `<Suspense>` with a `fallback` prop to show loading state.  
This reduces initial bundle size by splitting code into smaller chunks loaded  
on demand. The fallback can be any JSX, typically a spinner or skeleton  
screen. Lazy loading improves performance for large apps by loading code only  
when needed.  

---

## Ref Attribute for DOM Access

Access DOM nodes directly using refs in JSX.  

```tsx
import React, { useRef, useEffect } from 'react';

function RefExample(): JSX.Element {
  const inputRef = useRef<HTMLInputElement>(null);
  const divRef = useRef<HTMLDivElement>(null);
  
  useEffect(() => {
    inputRef.current?.focus();
    console.log('Div height:', divRef.current?.offsetHeight);
  }, []);
  
  const scrollToTop = (): void => {
    if (divRef.current) {
      divRef.current.scrollTop = 0;
    }
  };
  
  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Auto-focused" />
      
      <div ref={divRef} style={{ height: "200px", overflow: "auto" }}>
        <p>Scrollable content...</p>
        <p>More content...</p>
        <p>Even more content...</p>
      </div>
      
      <button onClick={scrollToTop}>Scroll to Top</button>
    </div>
  );
}
```

Refs provide direct access to DOM nodes. Create refs with `useRef` and attach  
them to JSX elements via the `ref` attribute. Access the DOM node through  
`ref.current`. Refs are useful for focus management, measurements, integrating  
third-party libraries, or animations. Unlike state, updating refs doesn't  
trigger re-renders. Use refs sparingly—prefer declarative React patterns when  
possible.  

---
