# Rendering Lists and Using Keys in React

Rendering lists is a fundamental operation in React applications, enabling you  
to dynamically generate UI elements from data arrays. React uses the  
JavaScript `.map()` method to transform array items into React elements,  
creating repeating patterns efficiently without manual duplication.  

When rendering lists, React needs a way to track each element across renders  
to optimize updates and maintain component state. This is where keys come  
into play. Keys are special attributes that help React identify which items  
have changed, been added, or been removed, enabling efficient reconciliation  
of the virtual DOM with the actual DOM.  

Understanding list rendering and keys is essential for building performant  
React applications. Without proper keys, React may re-render entire lists  
unnecessarily, causing performance issues and subtle bugs where component  
state gets mixed up between list items.  

Key concepts when working with lists in React include:  

- **The `.map()` Method**: JavaScript's array method used to transform data  
  into JSX elements, the standard approach for list rendering  
- **Keys**: Unique identifiers assigned to each element in a list to help  
  React track items efficiently  
- **Stable Keys**: Keys that remain consistent across renders, crucial for  
  maintaining component state and preventing UI glitches  
- **Unique Identifiers**: Using properties like `id` from your data instead  
  of array indices for dynamic lists  
- **Reconciliation**: React's process of comparing the virtual DOM tree to  
  determine minimal DOM updates needed  


## Rendering Array of Strings

The simplest form of list rendering transforms an array of strings into  
elements.  

```tsx
import type { JSX } from "react";

function FruitList(): JSX.Element {
	const fruits: string[] = ["Apple", "Banana", "Orange", "Mango", "Pineapple"];

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

This example demonstrates the basic pattern for rendering lists. The `.map()`  
method iterates over the `fruits` array, transforming each string into an  
`<li>` element. The `key` prop is required for list items to help React  
identify elements. While using the array index works for static lists that  
won't change, it's not ideal for dynamic lists where items may be added,  
removed, or reordered.  

---

## Rendering Array of Numbers

Numerical arrays can be rendered just like strings, useful for displaying  
data like scores or measurements.  

```tsx
import React from 'react';

function ScoreList(): JSX.Element {
  const scores: number[] = [95, 87, 92, 88, 91, 85, 90];
  
  return (
    <div>
      <h2>Test Scores</h2>
      <ul>
        {scores.map((score, index) => (
          <li key={index}>Student {index + 1}: {score}%</li>
        ))}
      </ul>
    </div>
  );
}

export default ScoreList;
```

Numbers are rendered directly in JSX, just like strings. This example shows  
how to combine the array index with the data value to create meaningful  
labels. The index is incremented by 1 to create human-friendly numbering  
starting from 1 instead of 0. For static score lists like this, using the  
index as a key is acceptable since the list order won't change.  

---

## Rendering Array of Objects

Real applications typically work with arrays of objects containing multiple  
properties that need to be displayed.  

```tsx
import React from 'react';

type User = {
  id: number;
  name: string;
  age: number;
  role: string;
};

function UserList(): JSX.Element {
  const users: User[] = [
    { id: 1, name: "Alice Johnson", age: 28, role: "Developer" },
    { id: 2, name: "Bob Smith", age: 34, role: "Designer" },
    { id: 3, name: "Carol Williams", age: 31, role: "Manager" },
    { id: 4, name: "David Brown", age: 29, role: "Developer" }
  ];
  
  return (
    <div>
      <h2>Team Members</h2>
      {users.map((user) => (
        <div key={user.id} className="user-card">
          <h3>{user.name}</h3>
          <p>Age: {user.age}</p>
          <p>Role: {user.role}</p>
        </div>
      ))}
    </div>
  );
}

export default UserList;
```

When rendering objects, use the unique identifier (like `id`) as the key.  
This is a best practice because IDs remain stable even when the array is  
sorted or filtered. Each object's properties are accessed using dot notation  
within the JSX. This pattern creates a card-like layout for each team member,  
demonstrating how to structure repeated UI elements from data.  

---

## Using Unique IDs for Keys

Always prefer unique identifiers over array indices for keys in dynamic  
lists.  

```tsx
import React from 'react';

type Task = {
  id: string;
  title: string;
  completed: boolean;
};

function TaskList(): JSX.Element {
  const tasks: Task[] = [
    { id: "task-1", title: "Review pull requests", completed: false },
    { id: "task-2", title: "Update documentation", completed: true },
    { id: "task-3", title: "Fix bug in login", completed: false },
    { id: "task-4", title: "Write unit tests", completed: true }
  ];
  
  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id} style={{ 
          textDecoration: task.completed ? 'line-through' : 'none' 
        }}>
          {task.title}
        </li>
      ))}
    </ul>
  );
}

export default TaskList;
```

This example uses string IDs (`"task-1"`, `"task-2"`, etc.) as keys, which is  
ideal for lists where items might be reordered or filtered. The unique IDs  
ensure React can correctly track each task even when the array order changes.  
Conditional styling shows completed tasks with strikethrough text,  
demonstrating how to combine list rendering with dynamic styles.  

---

## Why Keys Matter - Virtual DOM Reconciliation

Keys help React's reconciliation algorithm efficiently update the DOM by  
identifying which items changed.  

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  text: string;
};

function DynamicList(): JSX.Element {
  const [items, setItems] = useState<Item[]>([
    { id: 1, text: "First item" },
    { id: 2, text: "Second item" },
    { id: 3, text: "Third item" }
  ]);
  
  const shuffleItems = (): void => {
    const shuffled = [...items].sort(() => Math.random() - 0.5);
    setItems(shuffled);
  };
  
  return (
    <div>
      <button onClick={shuffleItems}>Shuffle Items</button>
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.text}</li>
        ))}
      </ul>
    </div>
  );
}

export default DynamicList;
```

This interactive example demonstrates why keys are important. When you click  
the shuffle button, the array order changes randomly. With proper keys using  
`item.id`, React knows that the same items are being reordered and can  
efficiently update the DOM positions. Without keys, or with index-based keys,  
React would unnecessarily recreate elements, losing any internal state and  
causing performance issues.  

---

## Common Mistake - Using Index as Key in Dynamic Lists

Using array indices as keys can cause bugs when the list changes.  

```tsx
import React, { useState } from 'react';

function ProblematicIndexKeys(): JSX.Element {
  const [items, setItems] = useState<string[]>([
    "Item A",
    "Item B", 
    "Item C"
  ]);
  
  const removeFirst = (): void => {
    setItems(items.slice(1));
  };
  
  return (
    <div>
      <button onClick={removeFirst}>Remove First Item</button>
      <ul>
        {/* BAD: Using index as key in dynamic list */}
        {items.map((item, index) => (
          <li key={index}>
            {item} - Key: {index}
          </li>
        ))}
      </ul>
      <p>Notice how keys shift when items are removed</p>
    </div>
  );
}

export default ProblematicIndexKeys;
```

This example illustrates the problem with index-based keys. When you remove  
the first item, all subsequent items shift to new indices. React sees the  
same keys (0, 1, 2) but with different content, causing it to update existing  
elements instead of removing the correct one. This can lead to bugs with  
component state, animations, and form inputs. Always use stable, unique  
identifiers for keys in lists that can change.  

---

## Extracting List Items into Components

For complex list items, extract them into separate components for better  
organization and reusability.  

```tsx
import React from 'react';

type Product = {
  id: number;
  name: string;
  price: number;
  description: string;
};

type ProductCardProps = {
  product: Product;
};

function ProductCard({ product }: ProductCardProps): JSX.Element {
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <p className="price">${product.price.toFixed(2)}</p>
      <p className="description">{product.description}</p>
      <button>Add to Cart</button>
    </div>
  );
}

function ProductList(): JSX.Element {
  const products: Product[] = [
    { id: 101, name: "Laptop", price: 999.99, description: "High-performance laptop" },
    { id: 102, name: "Mouse", price: 29.99, description: "Wireless mouse" },
    { id: 103, name: "Keyboard", price: 79.99, description: "Mechanical keyboard" }
  ];
  
  return (
    <div className="product-grid">
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

export default ProductList;
```

Extracting list items into separate components improves code organization and  
makes the markup easier to understand. The `key` prop is placed on the  
`ProductCard` component in the `.map()`, not inside the component itself.  
This separation of concerns makes components more reusable and testable. The  
`ProductCard` receives the entire product object as a prop and handles its  
own rendering logic.  

---

## Rendering Nested Lists

Lists can contain other lists, creating hierarchical structures like menus  
or category trees.  

```tsx
import React from 'react';

type Item = {
  id: number;
  name: string;
};

type Category = {
  id: number;
  name: string;
  items: Item[];
};

function CategoryList(): JSX.Element {
  const categories: Category[] = [
    {
      id: 1,
      name: "Electronics",
      items: [
        { id: 101, name: "Laptops" },
        { id: 102, name: "Phones" }
      ]
    },
    {
      id: 2,
      name: "Books",
      items: [
        { id: 201, name: "Fiction" },
        { id: 202, name: "Non-fiction" }
      ]
    }
  ];
  
  return (
    <div>
      {categories.map((category) => (
        <div key={category.id}>
          <h2>{category.name}</h2>
          <ul>
            {category.items.map((item) => (
              <li key={item.id}>{item.name}</li>
            ))}
          </ul>
        </div>
      ))}
    </div>
  );
}

export default CategoryList;
```

Nested lists require keys at each level of the hierarchy. The outer `.map()`  
uses `category.id` for keys, while the inner `.map()` uses `item.id`. Each  
list maintains its own set of keys, which only need to be unique among  
siblings, not globally. This pattern is common for navigation menus,  
organizational charts, and hierarchical data structures.  

---

## Filtering Lists Before Rendering

Combine array filtering with mapping to show only relevant items.  

```tsx
import React from 'react';

type Task = {
  id: number;
  title: string;
  status: 'active' | 'completed';
};

function FilteredTaskList(): JSX.Element {
  const tasks: Task[] = [
    { id: 1, title: "Complete project", status: "active" },
    { id: 2, title: "Review code", status: "completed" },
    { id: 3, title: "Write tests", status: "active" },
    { id: 4, title: "Update docs", status: "completed" }
  ];
  
  const activeTasks = tasks.filter(task => task.status === "active");
  
  return (
    <div>
      <h2>Active Tasks</h2>
      {activeTasks.length > 0 ? (
        <ul>
          {activeTasks.map((task) => (
            <li key={task.id}>{task.title}</li>
          ))}
        </ul>
      ) : (
        <p>No active tasks!</p>
      )}
    </div>
  );
}

export default FilteredTaskList;
```

Filtering arrays before mapping is a common pattern for displaying subsets of  
data. The `.filter()` method creates a new array containing only items that  
match the condition. A conditional check ensures appropriate messaging when  
the filtered list is empty. This approach keeps the JSX clean by separating  
data transformation logic from rendering logic.  

---

## Sorting Lists Before Rendering

Sort arrays to control the display order of list items.  

```tsx
import React from 'react';

type Student = {
  id: number;
  name: string;
  grade: number;
};

function SortedStudentList(): JSX.Element {
  const students: Student[] = [
    { id: 1, name: "Zoe Taylor", grade: 88 },
    { id: 2, name: "Alice Johnson", grade: 95 },
    { id: 3, name: "Mike Brown", grade: 91 },
    { id: 4, name: "Emma Wilson", grade: 87 }
  ];
  
  const sortedByGrade = [...students].sort((a, b) => b.grade - a.grade);
  
  return (
    <div>
      <h2>Students by Grade (Highest First)</h2>
      <ol>
        {sortedByGrade.map((student) => (
          <li key={student.id}>
            {student.name} - {student.grade}%
          </li>
        ))}
      </ol>
    </div>
  );
}

export default SortedStudentList;
```

Sorting arrays changes the display order without affecting the original data.  
The spread operator `[...students]` creates a copy before sorting to avoid  
mutating the original array. The sort function compares grades in descending  
order (`b.grade - a.grade`). Notice that we still use `student.id` as the  
key, not the array index, because keys should identify the item itself, not  
its position.  

---

## Rendering Lists with State

Interactive lists where items can be added or removed using React state.  

```tsx
import React, { useState, ChangeEvent } from 'react';

type Todo = {
  id: number;
  text: string;
};

function TodoList(): JSX.Element {
  const [todos, setTodos] = useState<Todo[]>([
    { id: 1, text: "Learn React" },
    { id: 2, text: "Build a project" }
  ]);
  const [nextId, setNextId] = useState<number>(3);
  const [inputValue, setInputValue] = useState<string>("");
  
  const addTodo = (): void => {
    if (inputValue.trim()) {
      setTodos([...todos, { id: nextId, text: inputValue }]);
      setNextId(nextId + 1);
      setInputValue("");
    }
  };
  
  const removeTodo = (id: number): void => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      <input
        value={inputValue}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setInputValue(e.target.value)}
        placeholder="Enter a todo"
      />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            {todo.text}
            <button onClick={() => removeTodo(todo.id)}>Remove</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

This example demonstrates a fully interactive list with add and remove  
functionality. Each todo has a unique ID generated from incrementing state.  
The `addTodo` function creates a new array with the spread operator, adding  
the new item while preserving existing ones. The `removeTodo` function  
filters out the item by ID. Using unique IDs as keys ensures React correctly  
tracks items as they're added and removed.  

---

## Conditional Rendering Within Lists

Apply different rendering logic to individual list items based on their  
properties.  

```tsx
import React from 'react';

type Status = 'online' | 'offline' | 'maintenance';

type Item = {
  id: number;
  name: string;
  status: Status;
  uptime: number;
};

function StatusList(): JSX.Element {
  const items: Item[] = [
    { id: 1, name: "Server A", status: "online", uptime: 99.9 },
    { id: 2, name: "Server B", status: "offline", uptime: 0 },
    { id: 3, name: "Server C", status: "maintenance", uptime: 85.5 },
    { id: 4, name: "Server D", status: "online", uptime: 100 }
  ];
  
  const getStatusColor = (status: Status): string => {
    switch(status) {
      case "online": return "green";
      case "offline": return "red";
      case "maintenance": return "orange";
      default: return "gray";
    }
  };
  
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>
          <span style={{ color: getStatusColor(item.status), fontWeight: "bold" }}>
            {item.status.toUpperCase()}
          </span>
          {" - "}
          {item.name}
          {item.status === "online" && ` (Uptime: ${item.uptime}%)`}
        </li>
      ))}
    </ul>
  );
}

export default StatusList;
```

Individual list items can have conditional styling and content based on their  
data. This example uses a helper function to determine colors based on  
status. The logical AND operator shows uptime only for online servers. This  
pattern is useful for dashboards, status pages, or any UI where items need  
different visual treatments based on their state.  

---

## Rendering Empty State for Empty Lists

Provide helpful feedback when lists are empty instead of showing nothing.  

```tsx
import React from 'react';

type Message = {
  id: number;
  sender: string;
  text: string;
};

type MessageListProps = {
  messages: Message[];
};

function MessageList({ messages }: MessageListProps): JSX.Element {
  if (messages.length === 0) {
    return (
      <div className="empty-state">
        <p>No messages yet</p>
        <p>Start a conversation to see messages here</p>
      </div>
    );
  }
  
  return (
    <div>
      <h2>Messages ({messages.length})</h2>
      <ul>
        {messages.map((message) => (
          <li key={message.id}>
            <strong>{message.sender}:</strong> {message.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

function MessageApp(): JSX.Element {
  const messages: Message[] = [];
  
  return <MessageList messages={messages} />;
}

export default MessageApp;
```

Empty states improve user experience by explaining why a list is empty and  
what users can do. This example checks the array length and returns different  
JSX based on whether messages exist. The empty state provides helpful context  
instead of leaving users confused by a blank screen. This pattern is  
essential for good UX in applications with dynamic data.  

---

## Lists with Multiple Actions

List items often need multiple interactive elements like edit and delete  
buttons.  

```tsx
import React, { useState } from 'react';

type Contact = {
  id: number;
  name: string;
  email: string;
};

function ContactList(): JSX.Element {
  const [contacts, setContacts] = useState<Contact[]>([
    { id: 1, name: "Alice Johnson", email: "alice@example.com" },
    { id: 2, name: "Bob Smith", email: "bob@example.com" },
    { id: 3, name: "Carol White", email: "carol@example.com" }
  ]);
  
  const deleteContact = (id: number): void => {
    setContacts(contacts.filter(contact => contact.id !== id));
  };
  
  const sendEmail = (email: string): void => {
    console.log(`Sending email to ${email}`);
  };
  
  return (
    <ul>
      {contacts.map((contact) => (
        <li key={contact.id}>
          <span>{contact.name} - {contact.email}</span>
          <button onClick={() => sendEmail(contact.email)}>Email</button>
          <button onClick={() => deleteContact(contact.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}

export default ContactList;
```

List items commonly need multiple action buttons. Each button's onClick  
handler receives a different parameter specific to that contact. Arrow  
functions in the onClick props capture the current item's data. This pattern  
is widely used in CRUD applications where each list item has view, edit, and  
delete operations.  

---

## Rendering Lists from JSON Data

Simulating real-world scenarios where data comes from APIs or external  
sources.  

```tsx
import React from 'react';

type Book = {
  id: string;
  title: string;
  author: string;
  year: number;
  rating: number;
};

function BookList(): JSX.Element {
  const booksData: Book[] = [
    {
      id: "isbn-001",
      title: "React Fundamentals",
      author: "John Developer",
      year: 2023,
      rating: 4.5
    },
    {
      id: "isbn-002",
      title: "Advanced JavaScript",
      author: "Jane Coder",
      year: 2022,
      rating: 4.8
    },
    {
      id: "isbn-003",
      title: "Web Development Guide",
      author: "Mike Engineer",
      year: 2024,
      rating: 4.6
    }
  ];
  
  return (
    <div>
      <h2>Available Books</h2>
      <div className="book-grid">
        {booksData.map((book) => (
          <article key={book.id} className="book-card">
            <h3>{book.title}</h3>
            <p>By {book.author}</p>
            <p>Published: {book.year}</p>
            <p>Rating: {"⭐".repeat(Math.round(book.rating))}</p>
          </article>
        ))}
      </div>
    </div>
  );
}

export default BookList;
```

This example simulates working with data from an API. The data structure  
includes various properties typical of real applications. Using string IDs  
like ISBN numbers is common with external data. The rating display uses the  
`repeat()` method to show star symbols, demonstrating how to process data for  
visual representation. In real applications, this data would come from a  
fetch call or state management system.  

---

## Lists with Checkboxes and Selection State

Track which items are selected using state for multi-select functionality.  

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  name: string;
};

function SelectableList(): JSX.Element {
  const items: Item[] = [
    { id: 1, name: "Item One" },
    { id: 2, name: "Item Two" },
    { id: 3, name: "Item Three" },
    { id: 4, name: "Item Four" }
  ];
  
  const [selectedIds, setSelectedIds] = useState<number[]>([]);
  
  const toggleSelection = (id: number): void => {
    if (selectedIds.includes(id)) {
      setSelectedIds(selectedIds.filter(selectedId => selectedId !== id));
    } else {
      setSelectedIds([...selectedIds, id]);
    }
  };
  
  return (
    <div>
      <p>Selected: {selectedIds.length} items</p>
      <ul>
        {items.map((item) => (
          <li key={item.id}>
            <label>
              <input
                type="checkbox"
                checked={selectedIds.includes(item.id)}
                onChange={() => toggleSelection(item.id)}
              />
              {item.name}
            </label>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default SelectableList;
```

Multi-select lists maintain an array of selected item IDs in state. The  
`toggleSelection` function adds or removes IDs from the array. Each checkbox  
checks if its ID is in the `selectedIds` array. This pattern is common in  
file managers, email clients, and any interface requiring bulk actions on  
multiple items.  

---

## Paginated Lists

Display large lists in pages to improve performance and user experience.  

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  title: string;
  description: string;
};

function PaginatedList(): JSX.Element {
  const allItems: Item[] = Array.from({ length: 50 }, (_, i) => ({
    id: i + 1,
    title: `Item ${i + 1}`,
    description: `Description for item ${i + 1}`
  }));
  
  const [currentPage, setCurrentPage] = useState<number>(1);
  const itemsPerPage = 10;
  
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const currentItems = allItems.slice(startIndex, endIndex);
  const totalPages = Math.ceil(allItems.length / itemsPerPage);
  
  return (
    <div>
      <h2>All Items (Page {currentPage} of {totalPages})</h2>
      <ul>
        {currentItems.map((item) => (
          <li key={item.id}>
            <strong>{item.title}</strong> - {item.description}
          </li>
        ))}
      </ul>
      <div>
        <button 
          onClick={() => setCurrentPage(currentPage - 1)}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        <span> Page {currentPage} of {totalPages} </span>
        <button 
          onClick={() => setCurrentPage(currentPage + 1)}
          disabled={currentPage === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}

export default PaginatedList;
```

Pagination splits large datasets into manageable chunks. The `slice()` method  
extracts the current page's items based on the page number and items per  
page. Navigation buttons update the current page state, with disabled states  
preventing invalid navigation. This approach reduces initial render time and  
improves scrolling performance for large lists.  

---

## Search and Filter Lists

Implement real-time search filtering for lists based on user input.  

```tsx
import React, { useState, ChangeEvent } from 'react';

type Person = {
  id: number;
  name: string;
  department: string;
};

function SearchableList(): JSX.Element {
  const [searchTerm, setSearchTerm] = useState<string>("");
  
  const people: Person[] = [
    { id: 1, name: "Alice Johnson", department: "Engineering" },
    { id: 2, name: "Bob Smith", department: "Marketing" },
    { id: 3, name: "Carol Williams", department: "Engineering" },
    { id: 4, name: "David Brown", department: "Sales" },
    { id: 5, name: "Emma Davis", department: "Marketing" }
  ];
  
  const filteredPeople = people.filter((person) =>
    person.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
    person.department.toLowerCase().includes(searchTerm.toLowerCase())
  );
  
  return (
    <div>
      <input
        type="text"
        placeholder="Search by name or department..."
        value={searchTerm}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setSearchTerm(e.target.value)}
      />
      <p>Found {filteredPeople.length} results</p>
      <ul>
        {filteredPeople.map((person) => (
          <li key={person.id}>
            {person.name} - {person.department}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default SearchableList;
```

Search functionality filters lists in real-time as users type. The filter  
checks if the search term appears in either the name or department, using  
`toLowerCase()` for case-insensitive matching. The filtered array updates  
with every keystroke, immediately showing matching results. This pattern  
provides instant feedback and is common in directory, product, and document  
search interfaces.  

---

## Lists with Expand/Collapse State

Individual list items can maintain their own expanded/collapsed state for  
detailed views.  

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  title: string;
  details: string;
  category: string;
};

type ExpandableItemProps = {
  item: Item;
};

function ExpandableItem({ item }: ExpandableItemProps): JSX.Element {
  const [isExpanded, setIsExpanded] = useState<boolean>(false);
  
  return (
    <li>
      <div onClick={() => setIsExpanded(!isExpanded)} style={{ cursor: "pointer" }}>
        <strong>{isExpanded ? "▼" : "▶"} {item.title}</strong>
      </div>
      {isExpanded && (
        <div style={{ marginLeft: "20px", marginTop: "10px" }}>
          <p>{item.details}</p>
          <p>Category: {item.category}</p>
        </div>
      )}
    </li>
  );
}

function ExpandableList(): JSX.Element {
  const items: Item[] = [
    { id: 1, title: "React Basics", details: "Learn core React concepts", category: "Tutorial" },
    { id: 2, title: "State Management", details: "Managing app state", category: "Advanced" },
    { id: 3, title: "Hooks Guide", details: "Using React Hooks", category: "Tutorial" }
  ];
  
  return (
    <ul>
      {items.map((item) => (
        <ExpandableItem key={item.id} item={item} />
      ))}
    </ul>
  );
}

export default ExpandableList;
```

Extracting list items into components allows each to maintain independent  
state. Each `ExpandableItem` tracks its own `isExpanded` state, enabling  
individual expand/collapse functionality. The arrow symbol changes based on  
state, and additional details appear conditionally. This pattern is useful  
for FAQs, file explorers, and any interface with collapsible sections.  

---

## Lists with Drag and Drop Ordering

Advanced list manipulation with visual reordering (conceptual example showing  
data structure).  

```tsx
import React, { useState } from 'react';

type Item = {
  id: number;
  text: string;
  order: number;
};

function ReorderableList(): JSX.Element {
  const [items, setItems] = useState<Item[]>([
    { id: 1, text: "First priority", order: 1 },
    { id: 2, text: "Second priority", order: 2 },
    { id: 3, text: "Third priority", order: 3 },
    { id: 4, text: "Fourth priority", order: 4 }
  ]);
  
  const moveUp = (id: number): void => {
    const index = items.findIndex(item => item.id === id);
    if (index > 0) {
      const newItems = [...items];
      [newItems[index - 1], newItems[index]] = [newItems[index], newItems[index - 1]];
      setItems(newItems);
    }
  };
  
  const moveDown = (id: number): void => {
    const index = items.findIndex(item => item.id === id);
    if (index < items.length - 1) {
      const newItems = [...items];
      [newItems[index], newItems[index + 1]] = [newItems[index + 1], newItems[index]];
      setItems(newItems);
    }
  };
  
  return (
    <ul>
      {items.map((item, index) => (
        <li key={item.id}>
          {item.text}
          <button onClick={() => moveUp(item.id)} disabled={index === 0}>
            ↑
          </button>
          <button onClick={() => moveDown(item.id)} disabled={index === items.length - 1}>
            ↓
          </button>
        </li>
      ))}
    </ul>
  );
}

export default ReorderableList;
```

This example demonstrates reorderable lists using move up/down buttons. The  
`moveUp` and `moveDown` functions swap adjacent items using array  
destructuring. Buttons are disabled at the boundaries to prevent invalid  
moves. Notice that keys remain tied to item IDs, not positions—this is  
crucial because as items move, their IDs stay constant while their positions  
change. For production drag-and-drop, consider libraries like  
`react-beautiful-dnd` or `dnd-kit`.
