# Forms

React provides a powerful and flexible approach to handling forms through  
controlled and uncontrolled components. Form handling in React differs from  
traditional HTML forms because React components maintain their own state and  
update it based on user input. This declarative approach gives you complete  
control over form data, enabling real-time validation, dynamic fields, and  
complex user interactions.  

Forms are essential for collecting user input in web applications, from  
simple login screens to complex multi-step wizards. React's component-based  
architecture makes it easy to build reusable form elements, validate data,  
and manage form state efficiently. Unlike traditional forms that rely on DOM  
manipulation, React forms use state to drive the UI, creating a single  
source of truth for form data.  

Understanding form handling in React is crucial for building interactive  
applications. Whether you're creating a contact form, user registration, or  
a complex data entry system, mastering React form patterns will help you  
build robust, user-friendly interfaces.  

Key concepts when working with forms in React include:  

- **Controlled Components**: Form elements whose values are controlled by  
  React state, providing a single source of truth  
- **Uncontrolled Components**: Form elements that maintain their own internal  
  state, accessed via refs when needed  
- **Form Events**: Handle user interactions like onChange, onSubmit, onBlur,  
  and onFocus to update state and validate data  
- **Validation**: Check user input for correctness and provide feedback  
  before submission  
- **Form State Management**: Track values, errors, touched fields, and  
  submission status  
- **Synthetic Events**: React's cross-browser event wrapper that provides  
  consistent event handling  

This document covers everything from basic controlled inputs to advanced  
form patterns including validation, dynamic fields, file uploads, and  
multi-step forms. Each section includes practical examples demonstrating  
best practices for handling forms in modern React applications using  
TypeScript and React 19.2 features.  

## Passing Form Data Directly

React allows you to handle form submissions by passing a function directly to the `action`  
prop of a `<form>` element. This simplifies form handling for basic cases by removing the 
need for `useState` or `event.preventDefault()`.

```tsx
import type { JSX } from "react";

export function MyForm(): JSX.Element {
  function handleSubmit(formData: FormData) {
    // Handle form submission logic here
    const name = formData.get("name") as string;
    alert(`Form submitted with name: ${name}`);
    console.log("Form submitted with name:", name);
  }

  return (
    <>
      <style>{`form { color: darkgray; }`}</style>
      <form action={handleSubmit}>
        <label>
          Name:
          <input type="text" name="name" style={{ margin: "1em" }} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    </>
  );
}

export default MyForm;
```

In this example, the `handleSubmit` function receives `FormData` directly,  
which can be used to access the form fields by their `name` attribute. This  
approach is clean and aligns with modern React patterns for handling forms  
without manual state management.


## Handling Multiple Inputs

This example shows how to retrieve **multiple fields** from the `FormData` object,  
including a text input and a checkbox, demonstrating how to handle different data  
types and multiple field values simultaneously.

```tsx
import type { JSX } from "react";

export function MultiInputForm(): JSX.Element {
  function handleMultiSubmit(formData: FormData) {
    const email = formData.get("email") as string;
    // Checkboxes often only appear in FormData if checked
    const subscribe = formData.has("subscribe"); 
    
    console.log("Email:", email);
    console.log("Subscribed:", subscribe);
    
    alert(`Submission:\nEmail: ${email}\nSubscribed: ${subscribe ? 'Yes' : 'No'}`);
  }

  return (
    <>
      <style>{`form { color: darkgreen; border: 1px solid darkgreen; padding: 10px; margin-top: 10px; }`}</style>
      <h3>Multi-Input Form</h3>
      <form action={handleMultiSubmit}>
        <label style={{ display: "block", marginBottom: "10px" }}>
          Email:
          <input type="email" name="email" required style={{ margin: "0 1em" }} />
        </label>
        
        <label style={{ display: "block", marginBottom: "10px" }}>
          <input type="checkbox" name="subscribe" /> 
          Subscribe to newsletter
        </label>
        
        <input type="submit" value="Sign Up" />
      </form>
    </>
  );
}
```

In this second example, the `handleMultiSubmit` function efficiently gathers values for  
both the `email` text field and the `subscribe` checkbox. For the checkbox, using  
`formData.has('subscribe')` is an idiomatic way to check if it was checked, as unchecked  
checkboxes are usually omitted from the `FormData` object entirely.


## Submitting with `formData.entries()`

This example demonstrates using the `entries()` method of `FormData` to iterate  
over all fields. This is helpful when you want to convert the form data into  
a standard JavaScript object for easier processing or sending to an API.  

```tsx
import type { JSX } from "react";

export function DataIterationForm(): JSX.Element {
  function handleIterationSubmit(formData: FormData) {
    const data: { [key: string]: FormDataEntryValue } = {};
    
    // Iterate over all key-value pairs
    for (const [key, value] of formData.entries()) {
      data[key] = value;
    }

    console.log("Serialized Data:", data);
    alert(`Form Data Object: ${JSON.stringify(data, null, 2)}`);
  }

  return (
    <>
      <style>{`form { color: darkblue; border: 1px solid darkblue; padding: 10px; margin-top: 10px; }`}</style>
      <h3>Data Iteration Form</h3>
      <form action={handleIterationSubmit}>
        <label style={{ display: "block", marginBottom: "10px" }}>
          Item:
          <input type="text" name="item" required style={{ margin: "0 1em" }} />
        </label>
        
        <label style={{ display: "block", marginBottom: "10px" }}>
          Quantity:
          <input type="number" name="quantity" required style={{ margin: "0 1em" }} />
        </label>
        
        <input type="submit" value="Process Order" />
      </form>
    </>
  );
}
```

This example uses `formData.entries()` to loop through every field/value pair submitted  
by the form. This pattern is commonly used to serialize the `FormData` into a standard,  
iterable JavaScript object (`data`), which is convenient for debugging, complex data  
manipulation, or preparing the data payload for an external API request.


## Handling File Uploads

This example demonstrates how to correctly handle a file input using `FormData`.  
When a file is selected, the corresponding value in `FormData` is a `File` object,  
not a simple string.

```tsx
import type { JSX } from "react";

export function FileUploadForm(): JSX.Element {
  function handleFileUpload(formData: FormData) {
    const description = formData.get("description") as string;
    const file = formData.get("document");
    
    console.log("Description:", description);
    
    if (file instanceof File) {
      alert(`File uploaded:\nName: ${file.name}\nSize: ${file.size} bytes\nType: ${file.type}`);
    } else {
      alert("No file selected.");
    }
  }

  return (
    <>
      <style>{`form { color: darkred; border: 1px solid darkred; padding: 10px; margin-top: 10px; }`}</style>
      <h3>File Upload Form</h3>
      <form action={handleFileUpload}>
        <label style={{ display: "block", marginBottom: "10px" }}>
          Description:
          <input type="text" name="description" style={{ margin: "0 1em" }} />
        </label>
        
        <label style={{ display: "block", marginBottom: "10px" }}>
          Select Document:
          <input type="file" name="document" required style={{ margin: "0 1em" }} />
        </label>
        
        <input type="submit" value="Upload" />
      </form>
    </>
  );
}
```

In the final example, when handling a file input, accessing the field via `formData.get("document")`  
retrieves a `File` object if a file was selected. The logic uses `instanceof File` to confirm the  
value's type before attempting to access file properties like `name` or `size`. When sending this  
`FormData` to a server, the server-side framework will automatically recognize and process the binary  
file data included within the payload.


## Basic Controlled Input

Controlled components are the recommended pattern for form inputs in React.  
The input value is controlled by state and updated via onChange handlers.  

```tsx
import { type ChangeEvent, type FormEvent, type JSX, useState } from "react";

function BasicControlledInput(): JSX.Element {
	const [name, setName] = useState<string>("");
	const [submittedName, setSubmittedName] = useState<string>("");

	const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
		setName(event.target.value);
	};

	const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
		event.preventDefault();
		setSubmittedName(name);
	};

	return (
		<div>
			<form onSubmit={handleSubmit}>
				<input
					type="text"
					value={name}
					onChange={handleChange}
					placeholder="Enter your name"
				/>
				<button type="submit">Submit</button>
			</form>
			{submittedName && <p>Hello there, {submittedName}!</p>}
		</div>
	);
}

export default BasicControlledInput;
```

This example demonstrates the fundamental controlled component pattern. The  
input's value is tied to the `name` state variable, and the `onChange`  
handler updates that state whenever the user types. The `handleSubmit`  
function prevents the default form submission behavior and processes the  
data in JavaScript. Controlled components give React complete control over  
the input value, making it easy to validate, format, or transform data in  
real-time. Always call `event.preventDefault()` in form submission handlers  
to prevent page reloads.  

---

## Multiple Controlled Inputs

Handle multiple inputs efficiently by using a single state object and the  
input's name attribute to identify which field changed.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type FormData = {
  username: string;
  email: string;
  age: string;
};

function MultipleInputs(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    username: "",
    email: "",
    age: ""
  });

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = event.target;
    setFormData(prevData => ({
      ...prevData,
      [name]: value
    }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Form submitted:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="username"
        value={formData.username}
        onChange={handleChange}
        placeholder="Username"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input
        type="number"
        name="age"
        value={formData.age}
        onChange={handleChange}
        placeholder="Age"
      />
      <button type="submit">Submit</button>
    </form>
  );
}

export default MultipleInputs;
```

This pattern efficiently manages multiple inputs with a single change  
handler. Using the computed property name `[name]: value` syntax, you can  
update the correct field in the state object based on the input's name  
attribute. The spread operator `...prevData` preserves existing values while  
updating only the changed field. This approach scales well for forms with  
many fields and reduces code duplication. Make sure each input has a unique  
`name` attribute that matches the corresponding property in your state  
object.  

---

## Textarea and Select Elements

Textareas and select elements work similarly to text inputs in React, using  
the value prop and onChange handler.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type FeedbackForm = {
  message: string;
  category: string;
};

function TextareaSelectExample(): JSX.Element {
  const [formData, setFormData] = useState<FeedbackForm>({
    message: "",
    category: "general"
  });

  const handleChange = (
    event: ChangeEvent<HTMLTextAreaElement | HTMLSelectElement>
  ): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Feedback:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <select
        name="category"
        value={formData.category}
        onChange={handleChange}
      >
        <option value="general">General</option>
        <option value="bug">Bug Report</option>
        <option value="feature">Feature Request</option>
        <option value="question">Question</option>
      </select>

      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Your message"
        rows={5}
      />

      <button type="submit">Submit Feedback</button>
    </form>
  );
}

export default TextareaSelectExample;
```

React normalizes textarea and select elements to work like regular inputs.  
Unlike HTML where textareas use children for content, React textareas use  
the `value` prop. Select elements also use `value` instead of the `selected`  
attribute on options. This consistency makes form handling simpler and more  
predictable. The union type `HTMLTextAreaElement | HTMLSelectElement` in the  
event handler allows the same function to handle both element types.  

---

## Checkbox Inputs

Checkboxes use the checked prop instead of value and handle boolean state.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type Preferences = {
  newsletter: boolean;
  notifications: boolean;
  marketing: boolean;
};

function CheckboxExample(): JSX.Element {
  const [preferences, setPreferences] = useState<Preferences>({
    newsletter: false,
    notifications: true,
    marketing: false
  });

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const { name, checked } = event.target;
    setPreferences(prev => ({
      ...prev,
      [name]: checked
    }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Preferences saved:", preferences);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        <input
          type="checkbox"
          name="newsletter"
          checked={preferences.newsletter}
          onChange={handleChange}
        />
        Subscribe to newsletter
      </label>

      <label>
        <input
          type="checkbox"
          name="notifications"
          checked={preferences.notifications}
          onChange={handleChange}
        />
        Enable notifications
      </label>

      <label>
        <input
          type="checkbox"
          name="marketing"
          checked={preferences.marketing}
          onChange={handleChange}
        />
        Receive marketing emails
      </label>

      <button type="submit">Save Preferences</button>
    </form>
  );
}

export default CheckboxExample;
```

Checkboxes use the `checked` prop to control their state and  
`event.target.checked` to access the new value. Unlike text inputs that use  
`value`, checkboxes represent boolean states. The pattern is similar to  
other controlled components, but with `checked` instead of `value`. Always  
wrap checkboxes with labels for better accessibility and usability—clicking  
the label text will toggle the checkbox.  

---

## Radio Button Groups

Radio buttons let users select one option from a group. All radios in a  
group share the same name but have different values.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type PaymentMethod = "credit-card" | "paypal" | "bank-transfer";

function RadioButtonExample(): JSX.Element {
  const [paymentMethod, setPaymentMethod] = useState<PaymentMethod>("credit-card");

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    setPaymentMethod(event.target.value as PaymentMethod);
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Selected payment method:", paymentMethod);
  };

  return (
    <form onSubmit={handleSubmit}>
      <fieldset>
        <legend>Select Payment Method</legend>
        
        <label>
          <input
            type="radio"
            name="payment"
            value="credit-card"
            checked={paymentMethod === "credit-card"}
            onChange={handleChange}
          />
          Credit Card
        </label>

        <label>
          <input
            type="radio"
            name="payment"
            value="paypal"
            checked={paymentMethod === "paypal"}
            onChange={handleChange}
          />
          PayPal
        </label>

        <label>
          <input
            type="radio"
            name="payment"
            value="bank-transfer"
            checked={paymentMethod === "bank-transfer"}
            onChange={handleChange}
          />
          Bank Transfer
        </label>
      </fieldset>

      <button type="submit">Continue</button>
    </form>
  );
}

export default RadioButtonExample;
```

Radio buttons in the same group must share the same `name` attribute. Each  
radio has a unique `value`, and the `checked` prop is set based on comparing  
the radio's value to the current state. When a radio is selected, the change  
handler updates the state to that radio's value. Use `fieldset` and `legend`  
elements to group related radio buttons for better accessibility and  
semantic HTML structure.  

---

## Basic Form Validation

Implement client-side validation to check user input before submission and  
provide helpful error messages.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type FormData = {
  email: string;
  password: string;
};

type Errors = {
  email?: string;
  password?: string;
};

function BasicValidation(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    email: "",
    password: ""
  });
  const [errors, setErrors] = useState<Errors>({});

  const validateForm = (): boolean => {
    const newErrors: Errors = {};

    if (!formData.email) {
      newErrors.email = "Email is required";
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = "Email is invalid";
    }

    if (!formData.password) {
      newErrors.password = "Password is required";
    } else if (formData.password.length < 8) {
      newErrors.password = "Password must be at least 8 characters";
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    if (errors[name as keyof Errors]) {
      setErrors(prev => ({ ...prev, [name]: undefined }));
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (validateForm()) {
      console.log("Form is valid:", formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          placeholder="Email"
        />
        {errors.email && <span style={{ color: 'red' }}>{errors.email}</span>}
      </div>

      <div>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && (
          <span style={{ color: 'red' }}>{errors.password}</span>
        )}
      </div>

      <button type="submit">Sign In</button>
    </form>
  );
}

export default BasicValidation;
```

Form validation checks data before submission and provides user feedback.  
This example validates on submit and clears errors as users type. The  
`validateForm` function checks each field and returns whether the form is  
valid. Error messages are stored in state and displayed below each input.  
Clearing errors on change improves UX by removing error messages as soon as  
users start correcting their input. Use regex patterns for email validation  
and length checks for passwords.  

---

## Real-time Validation with Blur Events

Validate fields when users leave them (onBlur) for better user experience  
without being too aggressive.  

```tsx
import React, { useState, ChangeEvent, FocusEvent, FormEvent } from 'react';

type FormData = {
  username: string;
  email: string;
};

type Errors = {
  username?: string;
  email?: string;
};

type Touched = {
  username: boolean;
  email: boolean;
};

function BlurValidation(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    username: "",
    email: ""
  });
  const [errors, setErrors] = useState<Errors>({});
  const [touched, setTouched] = useState<Touched>({
    username: false,
    email: false
  });

  const validateField = (name: keyof FormData, value: string): string | undefined => {
    if (name === "username") {
      if (!value) return "Username is required";
      if (value.length < 3) return "Username must be at least 3 characters";
    }
    if (name === "email") {
      if (!value) return "Email is required";
      if (!/\S+@\S+\.\S+/.test(value)) return "Email format is invalid";
    }
  };

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleBlur = (event: FocusEvent<HTMLInputElement>): void => {
    const { name, value } = event.target;
    const fieldName = name as keyof FormData;
    
    setTouched(prev => ({ ...prev, [fieldName]: true }));
    
    const error = validateField(fieldName, value);
    setErrors(prev => ({ ...prev, [fieldName]: error }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    
    const newErrors: Errors = {};
    (Object.keys(formData) as Array<keyof FormData>).forEach(key => {
      const error = validateField(key, formData[key]);
      if (error) newErrors[key] = error;
    });

    setErrors(newErrors);
    setTouched({ username: true, email: true });

    if (Object.keys(newErrors).length === 0) {
      console.log("Form submitted:", formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          name="username"
          value={formData.username}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Username"
        />
        {touched.username && errors.username && (
          <span style={{ color: 'red' }}>{errors.username}</span>
        )}
      </div>

      <div>
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Email"
        />
        {touched.email && errors.email && (
          <span style={{ color: 'red' }}>{errors.email}</span>
        )}
      </div>

      <button type="submit">Register</button>
    </form>
  );
}

export default BlurValidation;
```

Blur validation provides a better user experience by validating fields when  
users finish editing them, not on every keystroke. The `touched` state  
tracks which fields the user has interacted with, preventing errors from  
showing on pristine fields. Only display errors for fields that have been  
touched. This approach balances immediate feedback with not being overly  
aggressive in showing errors before users have a chance to enter valid data.  

---

## Uncontrolled Component with useRef

Uncontrolled components maintain their own internal state. Access their  
values using refs when needed, typically on form submission.  

```tsx
import React, { useRef, FormEvent } from 'react';

function UncontrolledForm(): JSX.Element {
  const nameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);
  const messageRef = useRef<HTMLTextAreaElement>(null);

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    
    const formData = {
      name: nameRef.current?.value || "",
      email: emailRef.current?.value || "",
      message: messageRef.current?.value || ""
    };

    console.log("Form submitted:", formData);

    event.currentTarget.reset();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        ref={nameRef}
        placeholder="Name"
        defaultValue=""
      />
      
      <input
        type="email"
        ref={emailRef}
        placeholder="Email"
        defaultValue=""
      />
      
      <textarea
        ref={messageRef}
        placeholder="Message"
        defaultValue=""
        rows={4}
      />

      <button type="submit">Send</button>
    </form>
  );
}

export default UncontrolledForm;
```

Uncontrolled components are useful when you don't need to track every change  
or validate in real-time. Use `useRef` to create references to DOM elements,  
and access their values via `ref.current.value`. Note the use of  
`defaultValue` instead of `value`—this sets the initial value but doesn't  
control it. Uncontrolled components require less code but give you less  
control over the data. They're appropriate for simple forms or when  
integrating with non-React libraries. The form's `reset()` method clears all  
fields after submission.  

---

## File Upload Handling

Handle file uploads by accessing files from the input's files property.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type FileData = {
  file: File | null;
  preview: string | null;
};

function FileUploadExample(): JSX.Element {
  const [fileData, setFileData] = useState<FileData>({
    file: null,
    preview: null
  });
  const [uploadStatus, setUploadStatus] = useState<string>("");

  const handleFileChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const file = event.target.files?.[0];
    
    if (file) {
      if (file.size > 5 * 1024 * 1024) {
        setUploadStatus("File size must be less than 5MB");
        return;
      }

      if (!file.type.startsWith('image/')) {
        setUploadStatus("Only image files are allowed");
        return;
      }

      const reader = new FileReader();
      reader.onloadend = () => {
        setFileData({
          file,
          preview: reader.result as string
        });
        setUploadStatus("");
      };
      reader.readAsDataURL(file);
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    
    if (!fileData.file) {
      setUploadStatus("Please select a file");
      return;
    }

    const formData = new FormData();
    formData.append('image', fileData.file);
    
    console.log("Uploading file:", fileData.file.name);
    setUploadStatus("File uploaded successfully!");
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="file"
        accept="image/*"
        onChange={handleFileChange}
      />

      {fileData.preview && (
        <div>
          <img
            src={fileData.preview}
            alt="Preview"
            style={{ maxWidth: '300px', marginTop: '10px' }}
          />
          <p>File: {fileData.file?.name}</p>
          <p>Size: {((fileData.file?.size || 0) / 1024).toFixed(2)} KB</p>
        </div>
      )}

      {uploadStatus && <p>{uploadStatus}</p>}

      <button type="submit" disabled={!fileData.file}>
        Upload
      </button>
    </form>
  );
}

export default FileUploadExample;
```

File uploads require special handling since file inputs can't be controlled  
components. Access the selected file via `event.target.files[0]`. Use  
`FileReader` to create preview images by reading the file as a data URL.  
Validate file size and type before processing. The `FormData` API is used to  
prepare files for upload to a server. The `accept` attribute limits which  
file types users can select, improving UX.  

---

## Dynamic Form Fields

Add and remove form fields dynamically based on user interaction.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type PhoneNumber = {
  id: number;
  value: string;
};

function DynamicFields(): JSX.Element {
  const [phoneNumbers, setPhoneNumbers] = useState<PhoneNumber[]>([
    { id: 1, value: "" }
  ]);

  const handleChange = (id: number, value: string): void => {
    setPhoneNumbers(prev =>
      prev.map(phone => phone.id === id ? { ...phone, value } : phone)
    );
  };

  const addPhoneNumber = (): void => {
    const newId = Math.max(...phoneNumbers.map(p => p.id), 0) + 1;
    setPhoneNumbers(prev => [...prev, { id: newId, value: "" }]);
  };

  const removePhoneNumber = (id: number): void => {
    if (phoneNumbers.length > 1) {
      setPhoneNumbers(prev => prev.filter(phone => phone.id !== id));
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    const validNumbers = phoneNumbers.filter(p => p.value.trim() !== "");
    console.log("Phone numbers:", validNumbers);
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Phone Numbers</h3>
      {phoneNumbers.map((phone, index) => (
        <div key={phone.id} style={{ marginBottom: '10px' }}>
          <input
            type="tel"
            value={phone.value}
            onChange={(e: ChangeEvent<HTMLInputElement>) =>
              handleChange(phone.id, e.target.value)
            }
            placeholder={`Phone ${index + 1}`}
          />
          <button
            type="button"
            onClick={() => removePhoneNumber(phone.id)}
            disabled={phoneNumbers.length === 1}
          >
            Remove
          </button>
        </div>
      ))}
      
      <button type="button" onClick={addPhoneNumber}>
        Add Phone Number
      </button>
      <button type="submit">Submit</button>
    </form>
  );
}

export default DynamicFields;
```

Dynamic fields allow users to add or remove inputs as needed. Store an array  
of objects with unique IDs to track each field. Use `map` to render inputs  
for each item, and update specific fields by finding and replacing the  
matching object. The unique `id` ensures React can track each field  
correctly during re-renders. Filter out empty fields before submission to  
avoid processing blank entries. Disable the remove button when only one  
field remains to ensure at least one input is always present.  

---

## Multi-Step Form

Break complex forms into multiple steps for better user experience.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type PersonalInfo = {
  firstName: string;
  lastName: string;
};

type ContactInfo = {
  email: string;
  phone: string;
};

type AllFormData = PersonalInfo & ContactInfo;

function MultiStepForm(): JSX.Element {
  const [step, setStep] = useState<number>(1);
  const [formData, setFormData] = useState<AllFormData>({
    firstName: "",
    lastName: "",
    email: "",
    phone: ""
  });

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const nextStep = (): void => setStep(prev => prev + 1);
  const prevStep = (): void => setStep(prev => prev - 1);

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Final submission:", formData);
  };

  const renderStep = (): JSX.Element => {
    switch (step) {
      case 1:
        return (
          <div>
            <h3>Step 1: Personal Information</h3>
            <input
              type="text"
              name="firstName"
              value={formData.firstName}
              onChange={handleChange}
              placeholder="First Name"
              required
            />
            <input
              type="text"
              name="lastName"
              value={formData.lastName}
              onChange={handleChange}
              placeholder="Last Name"
              required
            />
            <button type="button" onClick={nextStep}>
              Next
            </button>
          </div>
        );

      case 2:
        return (
          <div>
            <h3>Step 2: Contact Information</h3>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleChange}
              placeholder="Email"
              required
            />
            <input
              type="tel"
              name="phone"
              value={formData.phone}
              onChange={handleChange}
              placeholder="Phone"
              required
            />
            <button type="button" onClick={prevStep}>
              Back
            </button>
            <button type="button" onClick={nextStep}>
              Next
            </button>
          </div>
        );

      case 3:
        return (
          <div>
            <h3>Step 3: Review & Submit</h3>
            <p>Name: {formData.firstName} {formData.lastName}</p>
            <p>Email: {formData.email}</p>
            <p>Phone: {formData.phone}</p>
            <button type="button" onClick={prevStep}>
              Back
            </button>
            <button type="submit">Submit</button>
          </div>
        );

      default:
        return <div>Unknown step</div>;
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>Step {step} of 3</div>
      {renderStep()}
    </form>
  );
}

export default MultiStepForm;
```

Multi-step forms break complex data collection into manageable chunks. Track  
the current step with state and conditionally render different field sets.  
Store all form data in a single state object so it persists across steps.  
Provide navigation buttons to move between steps, and show a review screen  
before final submission. This pattern improves user experience for long  
forms by reducing cognitive load and making progress clear. Consider adding  
a progress indicator and validating each step before allowing users to  
proceed.  

---

## Async Form Validation

Validate fields asynchronously, such as checking username availability with  
an API.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type ValidationStatus = "idle" | "checking" | "valid" | "invalid";

function AsyncValidation(): JSX.Element {
  const [username, setUsername] = useState<string>("");
  const [validationStatus, setValidationStatus] = useState<ValidationStatus>("idle");
  const [errorMessage, setErrorMessage] = useState<string>("");

  const checkUsernameAvailability = async (value: string): Promise<void> => {
    if (value.length < 3) {
      setValidationStatus("invalid");
      setErrorMessage("Username must be at least 3 characters");
      return;
    }

    setValidationStatus("checking");

    try {
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const unavailableUsernames = ["admin", "user", "test"];
      const isAvailable = !unavailableUsernames.includes(value.toLowerCase());

      if (isAvailable) {
        setValidationStatus("valid");
        setErrorMessage("");
      } else {
        setValidationStatus("invalid");
        setErrorMessage("Username is already taken");
      }
    } catch (error) {
      setValidationStatus("invalid");
      setErrorMessage("Error checking username");
    }
  };

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const value = event.target.value;
    setUsername(value);
    setValidationStatus("idle");
    setErrorMessage("");
  };

  const handleBlur = (): void => {
    if (username) {
      checkUsernameAvailability(username);
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (validationStatus === "valid") {
      console.log("Username registered:", username);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          value={username}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Choose a username"
        />
        
        {validationStatus === "checking" && <span>Checking...</span>}
        {validationStatus === "valid" && (
          <span style={{ color: 'green' }}>✓ Available</span>
        )}
        {validationStatus === "invalid" && (
          <span style={{ color: 'red' }}>{errorMessage}</span>
        )}
      </div>

      <button type="submit" disabled={validationStatus !== "valid"}>
        Register
      </button>
    </form>
  );
}

export default AsyncValidation;
```

Async validation checks data against external sources like APIs. Validate on  
blur rather than every keystroke to avoid excessive API calls. Show a  
loading indicator while checking and provide clear feedback for valid and  
invalid states. Disable the submit button until validation completes  
successfully. For production applications, consider debouncing the  
validation to further reduce API calls. Handle errors gracefully and provide  
meaningful error messages to guide users.  

---

## Custom Hook for Form Management

Extract form logic into a reusable custom hook for cleaner components.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type FormValues = Record<string, string>;

function useForm<T extends FormValues>(initialValues: T) {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});

  const handleChange = (
    event: ChangeEvent<HTMLInputElement | HTMLTextAreaElement | HTMLSelectElement>
  ): void => {
    const { name, value } = event.target;
    setValues(prev => ({ ...prev, [name]: value }));
    if (errors[name as keyof T]) {
      setErrors(prev => ({ ...prev, [name]: undefined }));
    }
  };

  const resetForm = (): void => {
    setValues(initialValues);
    setErrors({});
  };

  return {
    values,
    errors,
    setErrors,
    handleChange,
    resetForm
  };
}

type ContactFormData = {
  name: string;
  email: string;
  message: string;
};

function ContactFormWithHook(): JSX.Element {
  const { values, errors, setErrors, handleChange, resetForm } = 
    useForm<ContactFormData>({
      name: "",
      email: "",
      message: ""
    });

  const validate = (): boolean => {
    const newErrors: Partial<Record<keyof ContactFormData, string>> = {};

    if (!values.name) newErrors.name = "Name is required";
    if (!values.email) newErrors.email = "Email is required";
    if (!values.message) newErrors.message = "Message is required";

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (validate()) {
      console.log("Form submitted:", values);
      resetForm();
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          name="name"
          value={values.name}
          onChange={handleChange}
          placeholder="Your name"
        />
        {errors.name && <span style={{ color: 'red' }}>{errors.name}</span>}
      </div>

      <div>
        <input
          type="email"
          name="email"
          value={values.email}
          onChange={handleChange}
          placeholder="Your email"
        />
        {errors.email && <span style={{ color: 'red' }}>{errors.email}</span>}
      </div>

      <div>
        <textarea
          name="message"
          value={values.message}
          onChange={handleChange}
          placeholder="Your message"
          rows={4}
        />
        {errors.message && <span style={{ color: 'red' }}>{errors.message}</span>}
      </div>

      <button type="submit">Send Message</button>
    </form>
  );
}

export default ContactFormWithHook;
```

Custom hooks encapsulate form logic for reuse across multiple components.  
The `useForm` hook manages values, errors, and common operations like  
handling changes and resetting the form. This pattern reduces code  
duplication and makes components cleaner by separating concerns. Generic  
types allow the hook to work with any form structure. Custom hooks are  
powerful tools for abstracting complex logic while maintaining React's  
composition model.  

---

## Form with Loading State

Handle async submissions with proper loading states and user feedback.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type SubmissionState = "idle" | "submitting" | "success" | "error";

function FormWithLoading(): JSX.Element {
  const [email, setEmail] = useState<string>("");
  const [submissionState, setSubmissionState] = useState<SubmissionState>("idle");
  const [message, setMessage] = useState<string>("");

  const handleSubmit = async (event: FormEvent<HTMLFormElement>): Promise<void> => {
    event.preventDefault();
    setSubmissionState("submitting");
    setMessage("");

    try {
      await new Promise((resolve, reject) => {
        setTimeout(() => {
          if (email.includes("error")) {
            reject(new Error("Submission failed"));
          } else {
            resolve(true);
          }
        }, 2000);
      });

      setSubmissionState("success");
      setMessage("Subscription successful! Check your email.");
      setEmail("");
    } catch (error) {
      setSubmissionState("error");
      setMessage("Something went wrong. Please try again.");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Subscribe to Newsletter</h3>
      
      <input
        type="email"
        value={email}
        onChange={(e: ChangeEvent<HTMLInputElement>) => setEmail(e.target.value)}
        placeholder="Enter your email"
        disabled={submissionState === "submitting"}
        required
      />

      <button
        type="submit"
        disabled={submissionState === "submitting" || !email}
      >
        {submissionState === "submitting" ? "Subscribing..." : "Subscribe"}
      </button>

      {message && (
        <p style={{
          color: submissionState === "success" ? "green" : "red"
        }}>
          {message}
        </p>
      )}
    </form>
  );
}

export default FormWithLoading;
```

Track submission state to provide appropriate feedback during async  
operations. Disable inputs and the submit button while submitting to prevent  
duplicate submissions. Show loading indicators and update button text to  
inform users that processing is happening. Display success or error messages  
based on the outcome. Clear the form on successful submission. This pattern  
creates a polished user experience for forms that interact with APIs or  
perform time-consuming operations.  

---

## Form with Password Strength Indicator

Provide real-time feedback on password strength to guide users.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type PasswordStrength = "weak" | "medium" | "strong";

function PasswordStrengthForm(): JSX.Element {
  const [password, setPassword] = useState<string>("");
  const [strength, setStrength] = useState<PasswordStrength | null>(null);

  const calculateStrength = (pwd: string): PasswordStrength => {
    let score = 0;
    
    if (pwd.length >= 8) score++;
    if (pwd.length >= 12) score++;
    if (/[a-z]/.test(pwd) && /[A-Z]/.test(pwd)) score++;
    if (/\d/.test(pwd)) score++;
    if (/[^a-zA-Z\d]/.test(pwd)) score++;

    if (score <= 2) return "weak";
    if (score <= 4) return "medium";
    return "strong";
  };

  const handlePasswordChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const value = event.target.value;
    setPassword(value);
    
    if (value) {
      setStrength(calculateStrength(value));
    } else {
      setStrength(null);
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (strength === "strong") {
      console.log("Password accepted:", password);
    }
  };

  const getStrengthColor = (): string => {
    switch (strength) {
      case "weak": return "red";
      case "medium": return "orange";
      case "strong": return "green";
      default: return "gray";
    }
  };

  const getStrengthWidth = (): string => {
    switch (strength) {
      case "weak": return "33%";
      case "medium": return "66%";
      case "strong": return "100%";
      default: return "0%";
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Create Password</h3>
      
      <input
        type="password"
        value={password}
        onChange={handlePasswordChange}
        placeholder="Enter password"
      />

      {password && (
        <div>
          <div style={{
            height: '5px',
            width: '100%',
            backgroundColor: '#e0e0e0',
            marginTop: '5px',
            borderRadius: '3px',
            overflow: 'hidden'
          }}>
            <div style={{
              height: '100%',
              width: getStrengthWidth(),
              backgroundColor: getStrengthColor(),
              transition: 'all 0.3s ease'
            }} />
          </div>
          <p style={{ color: getStrengthColor(), marginTop: '5px' }}>
            Strength: {strength?.toUpperCase()}
          </p>
        </div>
      )}

      <div style={{ fontSize: '12px', color: '#666', marginTop: '10px' }}>
        <p>Password should contain:</p>
        <ul>
          <li>At least 8 characters (12+ recommended)</li>
          <li>Uppercase and lowercase letters</li>
          <li>Numbers</li>
          <li>Special characters</li>
        </ul>
      </div>

      <button type="submit" disabled={strength !== "strong"}>
        Create Account
      </button>
    </form>
  );
}

export default PasswordStrengthForm;
```

Password strength indicators help users create secure passwords by providing  
real-time feedback. Calculate strength based on length, character diversity,  
and complexity requirements. Use visual indicators like colored progress  
bars to make strength immediately apparent. Provide clear guidelines about  
password requirements. Disable submission until the password meets minimum  
strength requirements. This pattern improves security while maintaining good  
user experience.  

---

## Autocomplete Input

Implement autocomplete suggestions based on user input.  

```tsx
import React, { useState, ChangeEvent, KeyboardEvent, FormEvent } from 'react';

const countries = [
  "United States", "United Kingdom", "Canada", "Australia",
  "Germany", "France", "Spain", "Italy", "Japan", "China"
];

function AutocompleteForm(): JSX.Element {
  const [inputValue, setInputValue] = useState<string>("");
  const [suggestions, setSuggestions] = useState<string[]>([]);
  const [selectedIndex, setSelectedIndex] = useState<number>(-1);
  const [showSuggestions, setShowSuggestions] = useState<boolean>(false);

  const handleInputChange = (event: ChangeEvent<HTMLInputElement>): void => {
    const value = event.target.value;
    setInputValue(value);

    if (value.length > 0) {
      const filtered = countries.filter(country =>
        country.toLowerCase().includes(value.toLowerCase())
      );
      setSuggestions(filtered);
      setShowSuggestions(true);
      setSelectedIndex(-1);
    } else {
      setSuggestions([]);
      setShowSuggestions(false);
    }
  };

  const handleSuggestionClick = (suggestion: string): void => {
    setInputValue(suggestion);
    setShowSuggestions(false);
    setSuggestions([]);
  };

  const handleKeyDown = (event: KeyboardEvent<HTMLInputElement>): void => {
    if (!showSuggestions || suggestions.length === 0) return;

    if (event.key === "ArrowDown") {
      event.preventDefault();
      setSelectedIndex(prev => 
        prev < suggestions.length - 1 ? prev + 1 : prev
      );
    } else if (event.key === "ArrowUp") {
      event.preventDefault();
      setSelectedIndex(prev => prev > 0 ? prev - 1 : -1);
    } else if (event.key === "Enter" && selectedIndex >= 0) {
      event.preventDefault();
      handleSuggestionClick(suggestions[selectedIndex]);
    } else if (event.key === "Escape") {
      setShowSuggestions(false);
    }
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Selected country:", inputValue);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div style={{ position: 'relative' }}>
        <input
          type="text"
          value={inputValue}
          onChange={handleInputChange}
          onKeyDown={handleKeyDown}
          placeholder="Search country"
          autoComplete="off"
        />

        {showSuggestions && suggestions.length > 0 && (
          <ul style={{
            position: 'absolute',
            top: '100%',
            left: 0,
            right: 0,
            border: '1px solid #ccc',
            backgroundColor: 'white',
            listStyle: 'none',
            margin: 0,
            padding: 0,
            maxHeight: '200px',
            overflowY: 'auto',
            zIndex: 1000
          }}>
            {suggestions.map((suggestion, index) => (
              <li
                key={suggestion}
                onClick={() => handleSuggestionClick(suggestion)}
                style={{
                  padding: '8px 12px',
                  cursor: 'pointer',
                  backgroundColor: index === selectedIndex ? '#e0e0e0' : 'white'
                }}
              >
                {suggestion}
              </li>
            ))}
          </ul>
        )}
      </div>

      <button type="submit">Submit</button>
    </form>
  );
}

export default AutocompleteForm;
```

Autocomplete enhances user experience by suggesting options as users type.  
Filter suggestions based on input value and display them in a dropdown. Add  
keyboard navigation with arrow keys for accessibility. Highlight the  
selected suggestion and allow selection with Enter. Hide suggestions when  
Escape is pressed or a selection is made. Position the suggestions list  
absolutely to avoid layout shifts. For large datasets, consider debouncing  
the filter operation or fetching suggestions from an API.  

---

## Conditional Fields

Show or hide form fields based on other field values.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type AccountType = "personal" | "business";

type FormData = {
  accountType: AccountType;
  firstName: string;
  lastName: string;
  companyName: string;
  taxId: string;
};

function ConditionalFields(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    accountType: "personal",
    firstName: "",
    lastName: "",
    companyName: "",
    taxId: ""
  });

  const handleChange = (
    event: ChangeEvent<HTMLInputElement | HTMLSelectElement>
  ): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    
    if (formData.accountType === "personal") {
      const { firstName, lastName } = formData;
      console.log("Personal account:", { firstName, lastName });
    } else {
      const { companyName, taxId } = formData;
      console.log("Business account:", { companyName, taxId });
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <h3>Create Account</h3>
      
      <select
        name="accountType"
        value={formData.accountType}
        onChange={handleChange}
      >
        <option value="personal">Personal</option>
        <option value="business">Business</option>
      </select>

      {formData.accountType === "personal" ? (
        <div>
          <input
            type="text"
            name="firstName"
            value={formData.firstName}
            onChange={handleChange}
            placeholder="First Name"
            required
          />
          <input
            type="text"
            name="lastName"
            value={formData.lastName}
            onChange={handleChange}
            placeholder="Last Name"
            required
          />
        </div>
      ) : (
        <div>
          <input
            type="text"
            name="companyName"
            value={formData.companyName}
            onChange={handleChange}
            placeholder="Company Name"
            required
          />
          <input
            type="text"
            name="taxId"
            value={formData.taxId}
            onChange={handleChange}
            placeholder="Tax ID"
            required
          />
        </div>
      )}

      <button type="submit">Create Account</button>
    </form>
  );
}

export default ConditionalFields;
```

Conditional fields adapt the form based on user selections, showing only  
relevant inputs. Use conditional rendering to display different field sets  
based on state values. This pattern reduces clutter and guides users through  
complex forms. Make sure to handle validation appropriately—only validate  
and submit fields that are currently visible. Conditional logic can be based  
on any state value, enabling sophisticated form flows.  

---

## Form with Debounced Search

Implement search with debouncing to reduce API calls while typing.  

```tsx
import React, { useState, useEffect, ChangeEvent } from 'react';

type SearchResult = {
  id: number;
  name: string;
};

function DebouncedSearchForm(): JSX.Element {
  const [searchTerm, setSearchTerm] = useState<string>("");
  const [debouncedTerm, setDebouncedTerm] = useState<string>("");
  const [results, setResults] = useState<SearchResult[]>([]);
  const [isSearching, setIsSearching] = useState<boolean>(false);

  useEffect(() => {
    const timerId = setTimeout(() => {
      setDebouncedTerm(searchTerm);
    }, 500);

    return () => clearTimeout(timerId);
  }, [searchTerm]);

  useEffect(() => {
    if (debouncedTerm) {
      performSearch(debouncedTerm);
    } else {
      setResults([]);
    }
  }, [debouncedTerm]);

  const performSearch = async (term: string): Promise<void> => {
    setIsSearching(true);
    
    try {
      await new Promise(resolve => setTimeout(resolve, 500));
      
      const mockResults: SearchResult[] = [
        { id: 1, name: `${term} - Result 1` },
        { id: 2, name: `${term} - Result 2` },
        { id: 3, name: `${term} - Result 3` }
      ];
      
      setResults(mockResults);
    } catch (error) {
      console.error("Search error:", error);
      setResults([]);
    } finally {
      setIsSearching(false);
    }
  };

  const handleChange = (event: ChangeEvent<HTMLInputElement>): void => {
    setSearchTerm(event.target.value);
  };

  return (
    <div>
      <h3>Search</h3>
      
      <input
        type="text"
        value={searchTerm}
        onChange={handleChange}
        placeholder="Type to search..."
      />

      {isSearching && <p>Searching...</p>}

      {!isSearching && results.length > 0 && (
        <ul>
          {results.map(result => (
            <li key={result.id}>{result.name}</li>
          ))}
        </ul>
      )}

      {!isSearching && searchTerm && results.length === 0 && (
        <p>No results found</p>
      )}
    </div>
  );
}

export default DebouncedSearchForm;
```

Debouncing delays executing a function until after a specified time has  
passed since the last invocation. This pattern is essential for search  
inputs to avoid making an API call on every keystroke. Use `useEffect` with  
a timeout to update a debounced version of the search term. Perform the  
actual search when the debounced term changes. Display loading states while  
searching and show results or "no results" messages appropriately. The  
cleanup function in `useEffect` cancels pending timeouts when the search  
term changes again.  

---

## Form with Field Dependencies

Update field options based on other field selections.  

```tsx
import React, { useState, ChangeEvent, FormEvent } from 'react';

type Country = "usa" | "canada" | "uk";

const statesByCountry: Record<Country, string[]> = {
  usa: ["California", "Texas", "New York", "Florida"],
  canada: ["Ontario", "Quebec", "British Columbia", "Alberta"],
  uk: ["England", "Scotland", "Wales", "Northern Ireland"]
};

type AddressForm = {
  country: Country;
  state: string;
  city: string;
};

function DependentFieldsForm(): JSX.Element {
  const [formData, setFormData] = useState<AddressForm>({
    country: "usa",
    state: "",
    city: ""
  });

  const handleCountryChange = (event: ChangeEvent<HTMLSelectElement>): void => {
    const country = event.target.value as Country;
    setFormData({
      country,
      state: "",
      city: ""
    });
  };

  const handleChange = (
    event: ChangeEvent<HTMLInputElement | HTMLSelectElement>
  ): void => {
    const { name, value } = event.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log("Address submitted:", formData);
  };

  const availableStates = statesByCountry[formData.country];

  return (
    <form onSubmit={handleSubmit}>
      <h3>Enter Address</h3>

      <select
        name="country"
        value={formData.country}
        onChange={handleCountryChange}
      >
        <option value="usa">United States</option>
        <option value="canada">Canada</option>
        <option value="uk">United Kingdom</option>
      </select>

      <select
        name="state"
        value={formData.state}
        onChange={handleChange}
        disabled={!availableStates.length}
      >
        <option value="">Select State/Province</option>
        {availableStates.map(state => (
          <option key={state} value={state}>
            {state}
          </option>
        ))}
      </select>

      <input
        type="text"
        name="city"
        value={formData.city}
        onChange={handleChange}
        placeholder="City"
        required
      />

      <button type="submit">Submit</button>
    </form>
  );
}

export default DependentFieldsForm;
```

Field dependencies create dynamic forms where one field's options depend on  
another field's value. When the parent field changes, reset dependent fields  
to avoid invalid combinations. Use a mapping object to store options for  
each parent value. This pattern is common in location selection (country →  
state → city), category filtering, and hierarchical data. Clear dependent  
fields when the parent changes to maintain data consistency.  

---

## Form with Complex Validation Schema

For complex forms, using a schema-based validation library like Zod or Yup
can simplify validation logic and make it more declarative.

```tsx
import React, { useState, FormEvent } from 'react';

// Mock schema validation - in a real app, you'd use a library like Zod
const schema = {
  profile: {
    name: (val: string) => val.length > 0,
    email: (val: string) => /\S+@\S+\.\S+/.test(val),
  },
  preferences: {
    newsletter: (val: boolean) => typeof val === 'boolean',
  },
};

type FormData = {
  profile: { name: string; email: string };
  preferences: { newsletter: boolean };
};

function SchemaValidationForm(): JSX.Element {
  const [formData, setFormData] = useState<FormData>({
    profile: { name: '', email: '' },
    preferences: { newsletter: false },
  });
  const [errors, setErrors] = useState<any>({});

  const validate = (): boolean => {
    const newErrors: any = { profile: {}, preferences: {} };
    if (!schema.profile.name(formData.profile.name)) {
      newErrors.profile.name = 'Name is required';
    }
    if (!schema.profile.email(formData.profile.email)) {
      newErrors.profile.email = 'Invalid email address';
    }
    if (!schema.preferences.newsletter(formData.preferences.newsletter)) {
      newErrors.preferences.newsletter = 'Invalid selection';
    }

    setErrors(newErrors);
    return !newErrors.profile.name && !newErrors.profile.email && !newErrors.preferences.newsletter;
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (validate()) {
      console.log('Form submitted:', formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="text"
          placeholder="Name"
          value={formData.profile.name}
          onChange={(e) =>
            setFormData({
              ...formData,
              profile: { ...formData.profile, name: e.target.value },
            })
          }
        />
        {errors.profile?.name && <p style={{ color: 'red' }}>{errors.profile.name}</p>}
      </div>
      <div>
        <input
          type="email"
          placeholder="Email"
          value={formData.profile.email}
          onChange={(e) =>
            setFormData({
              ...formData,
              profile: { ...formData.profile, email: e.target.value },
            })
          }
        />
        {errors.profile?.email && <p style={{ color: 'red' }}>{errors.profile.email}</p>}
      </div>
      <div>
        <label>
          <input
            type="checkbox"
            checked={formData.preferences.newsletter}
            onChange={(e) =>
              setFormData({
                ...formData,
                preferences: { newsletter: e.target.checked },
              })
            }
          />
          Subscribe to newsletter
        </label>
        {errors.preferences?.newsletter && <p style={{ color: 'red' }}>{errors.preferences.newsletter}</p>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

Schema-based validation centralizes validation logic, making it easier to
manage for complex, nested data structures.

---

## Form State with useReducer

For forms with complex state transitions, `useReducer` can be a more robust
alternative to `useState`.

```tsx
import React, { useReducer, FormEvent } from 'react';

type State = { count: number; text: string };
type Action = { type: 'increment' } | { type: 'decrement' } | { type: 'setText'; payload: string };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment': return { ...state, count: state.count + 1 };
    case 'decrement': return { ...state, count: state.count - 1 };
    case 'setText': return { ...state, text: action.payload };
    default: throw new Error();
  }
};

function ReducerForm(): JSX.Element {
  const [state, dispatch] = useReducer(reducer, { count: 0, text: '' });

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log('Form state:', state);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={state.text} onChange={e => dispatch({ type: 'setText', payload: e.target.value })} />
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <span>{state.count}</span>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button type="submit">Submit</button>
    </form>
  );
}
```

`useReducer` is ideal when the next state depends on the previous one, or
when multiple sub-values are involved in the state logic.

---

## Wizard Form with Local Storage Persistence

For long, multi-step forms, persisting the state to local storage can
prevent data loss and improve user experience.

```tsx
import React, { useState, useEffect, FormEvent } from 'react';

function PersistentWizardForm(): JSX.Element {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState(() => {
    const saved = localStorage.getItem('wizardFormData');
    return saved ? JSON.parse(saved) : { step1: '', step2: '' };
  });

  useEffect(() => {
    localStorage.setItem('wizardFormData', JSON.stringify(formData));
  }, [formData]);

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    console.log('Wizard complete:', formData);
    localStorage.removeItem('wizardFormData');
  };

  return (
    <form onSubmit={handleSubmit}>
      {step === 1 && (
        <div>
          <h3>Step 1</h3>
          <input
            type="text"
            placeholder="Step 1 Data"
            value={formData.step1}
            onChange={(e) => setFormData({ ...formData, step1: e.target.value })}
          />
          <button onClick={() => setStep(2)}>Next</button>
        </div>
      )}
      {step === 2 && (
        <div>
          <h3>Step 2</h3>
          <input
            type="text"
            placeholder="Step 2 Data"
            value={formData.step2}
            onChange={(e) => setFormData({ ...formData, step2: e.target.value })}
          />
          <button onClick={() => setStep(1)}>Back</button>
          <button type="submit">Submit</button>
        </div>
      )}
    </form>
  );
}
```

This pattern ensures that users can close the browser and return to the
form without losing their progress.

---

## Form with Drag-and-Drop File Upload

A more advanced file upload that supports drag-and-drop for a better user
experience.

```tsx
import React, { useState, DragEvent } from 'react';

function DragDropUpload(): JSX.Element {
  const [files, setFiles] = useState<File[]>([]);

  const handleDrop = (event: DragEvent<HTMLDivElement>): void => {
    event.preventDefault();
    const droppedFiles = Array.from(event.dataTransfer.files);
    setFiles(prev => [...prev, ...droppedFiles]);
  };

  const handleDragOver = (event: DragEvent<HTMLDivElement>): void => {
    event.preventDefault();
  };

  return (
    <div onDrop={handleDrop} onDragOver={handleDragOver} style={{ border: '2px dashed #ccc', padding: '20px' }}>
      <p>Drag and drop files here</p>
      <ul>{files.map(file => <li key={file.name}>{file.name}</li>)}</ul>
    </div>
  );
}
```

This component uses the HTML Drag and Drop API to create an interactive
file upload area.

---

## Dynamic List with Validation

A form that allows users to add a list of items, with validation for each
item in the list.

```tsx
import React, { useState, FormEvent } from 'react';

type Item = { id: number; value: string };
type Errors = { [key: number]: string };

function ValidatedDynamicList(): JSX.Element {
  const [items, setItems] = useState<Item[]>([{ id: 1, value: '' }]);
  const [errors, setErrors] = useState<Errors>({});

  const validate = (): boolean => {
    const newErrors: Errors = {};
    items.forEach(item => {
      if (item.value.length < 3) {
        newErrors[item.id] = 'Must be at least 3 characters';
      }
    });
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>): void => {
    event.preventDefault();
    if (validate()) {
      console.log('Submitted items:', items);
    }
  };

  const handleAddItem = () => {
    setItems([...items, { id: Date.now(), value: '' }]);
  };

  const handleItemChange = (id: number, value: string) => {
    setItems(items.map(item => (item.id === id ? { ...item, value } : item)));
  };

  return (
    <form onSubmit={handleSubmit}>
      {items.map(item => (
        <div key={item.id}>
          <input
            type="text"
            value={item.value}
            onChange={(e) => handleItemChange(item.id, e.target.value)}
          />
          {errors[item.id] && <p style={{ color: 'red' }}>{errors[item.id]}</p>}
        </div>
      ))}
      <button type="button" onClick={handleAddItem}>Add Item</button>
      <button type="submit">Submit</button>
    </form>
  );
}
```

This pattern is useful for forms where users need to input a variable number
of items, such as adding multiple phone numbers or addresses.

---

## Summary

React provides powerful patterns for handling forms through controlled and  
uncontrolled components. Key takeaways for effective form handling:  

- **Controlled Components** are the recommended pattern—use state to control  
  input values for full control and validation  
- **Handle Multiple Inputs** efficiently using a single change handler and  
  the input's name attribute  
- **Validate Early and Often** using onBlur for field-level validation and  
  onSubmit for final checks  
- **Provide User Feedback** with loading states, error messages, and success  
  confirmations  
- **Use TypeScript** for type safety and better developer experience  
- **Extract Reusable Logic** to custom hooks for cleaner, more maintainable  
  code  
- **Handle Async Operations** properly with loading states and error handling  
- **Debounce Expensive Operations** like API calls triggered by user input  
- **Implement Progressive Enhancement** with multi-step forms and conditional  
  fields  
- **Always Prevent Default** form submission to handle data in JavaScript  

Forms are a critical part of web applications. Mastering these patterns will  
help you build robust, user-friendly forms that handle validation, async  
operations, and complex user interactions effectively. Consider using form  
libraries like React Hook Form or Formik for production applications with  
complex requirements.
