## Part 1: The Fundamentals (Videos 1 - 10)

### 1. Introduction to Formik
**Why use Formik?** Forms are a vital part of business applications (registration, login, placing orders, etc.). Handling forms in plain React requires manually managing form data, validation, error messages, and submission. Doing this manually for complex forms becomes tedious and results in highly repetitive boilerplate code. 

Formik is a small library that abstracts these annoying parts. It helps you deal with forms in a scalable, performant, and easier way.

**Core Problems Formik Solves:**
1. Managing the form state (tracking values in and out of the form).
2. Handling form submission seamlessly.
3. Form validation and displaying error messages.

### 2. The `useFormik` Hook
**What it is:** A React hook provided by the Formik library to manage the core functionalities of your form.

**How to initialize:**
```jsx
import { useFormik } from 'formik';

const formik = useFormik({
  // Configuration object properties go here
});
```
The hook accepts a configuration object and returns an object (which we store in a constant named `formik`). This object contains various useful properties and helper methods to wire up to our HTML form elements.

### 3. Managing Form State
**Why:** To track the values of the input fields as the user types, mimicking controlled components in React.

**How:**
* **`initialValues`**: Pass an `initialValues` object to the `useFormik` configuration. *Crucial rule: The keys here must exactly match the `name` attributes of your HTML inputs.*
* **`onChange`**: Bind the inputs to Formik by setting `onChange={formik.handleChange}`. This helper method automatically updates the state based on the input's `name` attribute.
* **`value`**: Pass the state back to the input by setting `value={formik.values.fieldName}`.

```jsx
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  }
});

// Inside your JSX form:
<input 
  type="text" 
  name="name" 
  onChange={formik.handleChange} 
  value={formik.values.name} 
/>
```
**Result:** `formik.values` will always reflect the current, real-time state of the form.

### 4. Handling Form Submission
**Why:** To capture the final state of the form when the user clicks submit and automatically prevent the default browser page refresh.

**How:**
* **`onSubmit` Configuration:** Pass an `onSubmit` function to the `useFormik` object. It automatically receives the latest form values object as its argument.
* **Form tag binding:** Add `onSubmit={formik.handleSubmit}` to your `<form>` tag. Formik will execute your custom `onSubmit` logic when the form triggers a submit event. Ensure your button has `type="submit"`.

```jsx
const formik = useFormik({
  initialValues: { name: '' },
  onSubmit: values => {
    // Make API calls here with the 'values' object
    console.log('Form data submitted', values);
  }
});

// Inside your JSX:
<form onSubmit={formik.handleSubmit}>
  {/* inputs here */}
  <button type="submit">Submit</button>
</form>
```

### 5. Form Validation (The `validate` Function)
**Why:** To ensure users input the correct data types (e.g., required fields, proper email formats) before allowing submission.

**How:**
Pass a `validate` function to `useFormik`.
*Rules for the validate function:*
* It automatically receives the `values` object as an argument.
* It must return an `errors` object.
* The keys of the `errors` object must perfectly match the `name` attributes of the inputs.
* The values assigned to the `errors` keys should be the string error message you want to display to the user.

```jsx
const validate = values => {
  let errors = {};
  
  // Required check
  if (!values.name) {
    errors.name = 'Required';
  }
  
  // Required + Format check using Regex
  if (!values.email) {
    errors.email = 'Required';
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
    errors.email = 'Invalid email format';
  }
  
  return errors;
};

const formik = useFormik({
  initialValues,
  onSubmit,
  validate // Attach validation function here
});
```
*Note: Formik automatically runs this validation function on every keystroke (`onChange`).*

### 6. Displaying Error Messages
**Why:** To give visual feedback to the user when validation fails.

**How:**
Formik populates the `formik.errors` object automatically based on what your `validate` function returns. You can conditionally render these messages in your JSX under the respective input fields:

```jsx
{formik.errors.name ? <div className="error">{formik.errors.name}</div> : null}
```

### 7. Improving UX with Visited Fields (`touched`)
**Why:** By default, Formik runs validation immediately on load and on change. If you strictly check `formik.errors` for rendering, a user will see error messages for fields they haven't even clicked on yet. This is a very poor user experience. We only want to show an error after a user has interacted with (visited) a field.

**How:**
* **`onBlur`**: Add `onBlur={formik.handleBlur}` to your input fields. This tells Formik to track when a user visits and subsequently clicks out of an input.
* **`formik.touched`**: Formik stores these visited fields in the `formik.touched` object (e.g., if a user visits the `name` field, `formik.touched.name` becomes true).
* **Conditional Rendering**: Update your JSX to only render the error message if the field has been touched AND has an active error.

```jsx
<input 
  type="text" 
  name="name" 
  onChange={formik.handleChange} 
  onBlur={formik.handleBlur} // Tracks interaction
  value={formik.values.name} 
/>

{/* Better UX: Only show error if touched AND has error */}
{formik.touched.name && formik.errors.name ? (
  <div className="error">{formik.errors.name}</div>
) : null}
```

---

## Part 2: Yup & Context Components (Videos 11 - 20)

### 11. Schema Validation with Yup
**Why:** Writing custom validation logic (huge `if/else` statements) for every field is tedious, verbose, and difficult to maintain. Yup is a third-party object schema validation library that integrates seamlessly with Formik to handle complex validation gracefully.

**How:**
1. Install the library: `npm install yup` or `yarn add yup`.
2. Import it: `import * as Yup from 'yup';`
3. Define an object schema mapping to your form fields, applying validation rules via chaining methods (e.g., `.string()`, `.required()`, `.email()`).
4. Pass it to the `useFormik` configuration object using the `validationSchema` property instead of the standard `validate` function.

```jsx
import * as Yup from 'yup';

// 1. Define the validation schema
const validationSchema = Yup.object({
  name: Yup.string().required('Required!'),
  email: Yup.string().email('Invalid email format').required('Required!'),
  channel: Yup.string().required('Required!')
});

// 2. Pass it to the useFormik hook
const formik = useFormik({
  initialValues,
  onSubmit,
  validationSchema // replaces the custom 'validate' function
});
```

### 12. Reducing Boilerplate (`getFieldProps`)
**Why:** Binding `onChange`, `onBlur`, and `value` for every single input via `useFormik` properties is highly repetitive. Formik sees this common pattern and provides a helper method to cut down on code duplication.

**How:** Formik's `getFieldProps()` method automatically generates an object containing the `onChange`, `onBlur`, `name`, and `value` attributes needed for a specific field. You simply use the JavaScript spread operator `...` to apply it directly onto your input tag.

```jsx
{/* Before */}
<input 
  type="text" 
  name="name" 
  onChange={formik.handleChange} 
  onBlur={formik.handleBlur}
  value={formik.values.name} 
/>

{/* After: Greatly reduced boilerplate */}
<input type="text" id="name" {...formik.getFieldProps('name')} />
```

### 13. The `<Formik>` Component
**Why:** While `getFieldProps` reduces some boilerplate, Formik offers a fully declarative approach using React Context API components that completely abstract away the `useFormik` hook. The parent wrapper for this approach is the `<Formik>` component.

**How:**
* Import `Formik` from 'formik'.
* Wrap your entire form inside `<Formik>...</Formik>`.
* Pass `initialValues`, `validationSchema`, and `onSubmit` directly as props to this component.

```jsx
import { Formik } from 'formik';

function YouTubeForm() {
  return (
    <Formik 
      initialValues={initialValues} 
      validationSchema={validationSchema} 
      onSubmit={onSubmit}
    >
      {/* Form elements go here */}
    </Formik>
  );
}
```

### 14. The `<Form>` Component
**Why:** To automatically link your HTML form to Formik's submission handler natively, removing the need to manually attach `formik.handleSubmit`.

**How:**
* Import `Form` from 'formik'.
* Replace your standard HTML `<form>` tag with the `<Form>` component.
* Remove the `onSubmit` prop entirely. Formik silently intercepts the default HTML submit behavior via context.

```jsx
import { Formik, Form } from 'formik';

<Formik initialValues={...} onSubmit={...}>
  <Form>
    {/* Input fields */}
    <button type="submit">Submit</button>
  </Form>
</Formik>
```

### 15. The `<Field>` Component
**Why:** To seamlessly replace standard HTML `<input>` tags. The `<Field>` component automatically hooks up the `name` attribute to Formik's state tracking, completely removing the need to spread `getFieldProps`.

**How:** Import `Field` from 'formik' and swap out your `<input>`. By default, it renders an HTML `<input type="text">` and automatically maps state interactions natively to its `name` prop.

```jsx
import { Field } from 'formik';

{/* Instead of <input type="text" {...formik.getFieldProps('name')} /> */}
<Field type="text" id="name" name="name" />
```

### 16. The `<ErrorMessage>` Component
**Why:** To remove the repetitive conditional rendering logic required to check if a field was both touched AND has an error.

**How:** Import `ErrorMessage` and pass it the `name` of the associated field. It leverages context to automatically track the field's state and will only display the validation string if the conditions are met.

```jsx
import { ErrorMessage } from 'formik';

{/* Instead of {formik.touched.name && formik.errors.name ? <div>...</div> : null} */}
<ErrorMessage name="name" />
```

### 17. Journey So Far (Summary)
**Recap:** We successfully evolved a standard verbose React form into a streamlined, highly readable Formik architecture:
* Replaced repetitive state bindings with context-provided `<Formik>` wrapping.
* Abstracted form actions with `<Form>`.
* Stripped boilerplate bindings using `<Field>`.
* Automated complex validation rendering logic with `<ErrorMessage>`.

### 18. The `<Field>` Component Revisited
**Why:** Real-world forms require more than standard text inputs (like Textareas or Select dropdowns). In complex apps, you might also need total manual control over how a custom UI field component binds to Formik.

**How:**
* **Pass-through props:** Any standard HTML attribute added to `<Field>` (like `placeholder`) automatically passes right through to the underlying HTML element.
* **The `as` prop:** Use `as="textarea"` or `as="select"` to instruct Formik to render a different HTML element instead of the default input.
* **Render Props Pattern:** For ultimate custom control, pass an arrow function as the child of `<Field>`. This function exposes `field` (name, value, onChange, onBlur), `meta` (errors, touched state), and `form` objects, allowing you to explicitly wire Formik to custom React elements.

```jsx
{/* 1. Rendering a Textarea */}
<Field as="textarea" id="comments" name="comments" placeholder="Add comment..." />

{/* 2. Render Props Pattern */}
<Field name="address">
  {({ field, meta }) => {
    return (
      <div>
        <input type="text" {...field} />
        {meta.touched && meta.error ? <div>{meta.error}</div> : null}
      </div>
    )
  }}
</Field>
```

### 19. The `<ErrorMessage>` Component Revisited
**Why:** By default, the `<ErrorMessage>` component simply renders a raw text node into the DOM. You usually need it wrapped in an HTML container element (like a `<div>`) or styled through a custom React functional component.

**How:**
* **The `component` prop:** Add `component="div"` to wrap the raw text in an HTML element. You can alternatively pass a completely custom React functional component (e.g., `component={TextError}`).
* **Render Props Pattern:** Just like `<Field>`, you can pass an arrow function as a child to gain direct access to the error string itself, allowing you to build the JSX exactly how you'd like on the fly.

```jsx
{/* Wrapping in an HTML element */}
<ErrorMessage name="email" component="div" />

{/* Render Prop Pattern */}
<ErrorMessage name="email">
  {errorMsg => <div className="error">{errorMsg}</div>}
</ErrorMessage>
```

### 20. Nested Objects in Formik
**Why:** Often, backend databases or APIs expect incoming JSON payloads to be grouped or nested (for example, having a social object containing nested facebook and twitter strings instead of dumping them all into the root flat structure).

**How:**
* Organize the nested structure perfectly within your `initialValues` configuration object.
* Use dot-notation in the `name` attribute of your `<Field>` elements to drill down and accurately map to the nested structure.

```jsx
const initialValues = {
  name: '',
  social: {
    facebook: '',
    twitter: ''
  }
};

// In your JSX, instruct the Field to look at the dot-notated path:
<Field type="text" id="facebook" name="social.facebook" />
<Field type="text" id="twitter" name="social.twitter" />
```

---

## Part 3: Advanced Features & Control (Videos 21 - 30)

### 21. Arrays
**Why:** Sometimes you need to collect multiple values of the exact same type, such as two phone numbers (primary and secondary). Instead of creating separate state variables (`primaryPhone`, `secondaryPhone`), it is cleaner to store them in an array.

**How:** 1. Define the array in your `initialValues`.
2. Map to specific array indexes using bracket notation in the `name` attribute of your `<Field>`.

```jsx
const initialValues = {
  phNumbers: ['', '']
};

// In JSX:
<Field type="text" name="phNumbers[0]" />
<Field type="text" name="phNumbers[1]" />
```

### 22. The `<FieldArray>` Component
**Why:** Standard arrays are static (you hardcode `[0]` and `[1]`). Real-world applications often require dynamic forms where users can continuously add or remove fields on the fly (e.g., adding an arbitrary number of skills to a resume).

**How:** Import `<FieldArray>` from Formik. Use the Render Props pattern. The render prop function receives an object containing a `push` and `remove` method, along with the form state. You map over the array values and attach the methods to "+" and "-" buttons.

```jsx
import { FieldArray } from 'formik';

<FieldArray name="phNumbers">
  {fieldArrayProps => {
    const { push, remove, form } = fieldArrayProps;
    const { values } = form;
    
    return (
      <div>
        {values.phNumbers.map((phNumber, index) => (
          <div key={index}>
            <Field name={`phNumbers[${index}]`} />
            {/* Remove button (hide if it's the only field left) */}
            {index > 0 && <button onClick={() => remove(index)}>-</button>}
            {/* Add button */}
            <button onClick={() => push('')}>+</button>
          </div>
        ))}
      </div>
    );
  }}
</FieldArray>
```

### 23. The `<FastField>` Component
**Why:** Performance optimization. By default, typing in any `<Field>` causes every other `<Field>` in the entire form to re-render because the top-level Formik state changes. In a form with 30+ inputs, this causes noticeable input lag.

**How:** Import `<FastField>` and use it exactly like you would use `<Field>`. It implements an optimized `shouldComponentUpdate` lifecycle method, ensuring the component only re-renders if its specific value, error, or touched state changes.

```jsx
import { FastField } from 'formik';

{/* Replaces <Field> */}
<FastField name="address" />
```
*Note: Only use `<FastField>` for independent fields. If a field's validation depends on the value of another field, stick to `<Field>`.*

### 24. When Does Formik Run Validation?
**Why:** Understanding Formik's default validation lifecycle helps you debug "flickering" errors or unnecessary validation calls.

**How:** By default, Formik runs validation during three specific events:
* **`onChange`**: Every single keystroke.
* **`onBlur`**: Every time a field loses focus.
* **`onSubmit`**: Every time submission is attempted.

You can explicitly disable these defaults by passing props to the `<Formik>` wrapper if they cause performance issues:

```jsx
<Formik 
  initialValues={...} 
  validateOnChange={false} 
  validateOnBlur={false}
>
```

### 25. Field Level Validation
**Why:** Global validation schemas (like Yup) are great, but sometimes a specific validation rule is tightly coupled to a single component, or you want to reuse a custom field across multiple projects without dragging along a massive Yup schema.

**How:** You can pass a custom `validate` function directly to the `validate` prop on an individual `<Field>`.

```jsx
const validateComments = value => {
  let error;
  if (!value) {
    error = 'Required';
  }
  return error;
};

// In JSX:
<Field as="textarea" name="comments" validate={validateComments} />
```

### 26. Manually Triggering Validation
**Why:** You may have a requirement to validate the form via a custom button (e.g., a "Check for Errors" button) instead of waiting for `onSubmit`, or you may need to programmatically mark a field as touched.

**How:** Use the Render Props pattern on the main `<Formik>` wrapper. This exposes helper methods like `validateForm`, `validateField`, `setFieldTouched`, and `setTouched`.

```jsx
<Formik initialValues={...}>
  {formikProps => (
    <Form>
      {/* ...fields... */}
      <button onClick={() => formikProps.validateField('comments')}>
        Validate Comments
      </button>
      <button onClick={() => formikProps.validateForm()}>
        Validate All
      </button>
    </Form>
  )}
</Formik>
```

### 27. Disabling the Submit Button
**Why:** To prevent duplicate API calls (users double-clicking the submit button) and to prevent submission if the form has known errors.

**How:** Use `isValid` (checks if the errors object is empty), `dirty` (checks if the user has changed at least one value from `initialValues`; this should be used only when it is assumed that initial form state is invalid and user will be updating form before submitting), and `isSubmitting` (a boolean Formik toggles during submission).

1. In your `onSubmit` function, use the `onSubmitProps` to toggle the submitting state.
2. Disable the button conditionally.

```jsx
const onSubmit = (values, onSubmitProps) => {
  // Mock API call
  setTimeout(() => {
    onSubmitProps.setSubmitting(false); // Re-enables the button
  }, 2000);
};

// In JSX (inside Formik render props):
<button 
  type="submit" 
  disabled={!(formik.dirty && formikProps.isValid) || formikProps.isSubmitting}
>
  Submit
</button>
```

### 28. Load Saved Data (`enableReinitialize`)
**Why:** Forms are rarely just for creation; they are also used for editing. If your form relies on data fetched from an API, the component will mount before the data arrives. By default, Formik sets `initialValues` once on mount and ignores subsequent state updates.

**How:** Add the `enableReinitialize` prop to the `<Formik>` wrapper. This forces Formik to reset the form state whenever the `initialValues` object changes.

```jsx
const [savedData, setSavedData] = useState(null);

// Fetch data on mount...

<Formik 
  initialValues={savedData || initialValues} 
  enableReinitialize={true} 
>
```

### 29. Reset Form Data
**Why:** After a successful form submission, or if a user clicks a "Clear Form" button, you need to wipe the input fields back to their original `initialValues`.

**How:** 1. **Via standard button:** Formik automatically binds to any button with `type="reset"`.
2. **Programmatically:** Call `resetForm()` from the `onSubmitProps` after a successful API request.

```jsx
const onSubmit = (values, onSubmitProps) => {
  // After successful API call:
  onSubmitProps.resetForm();
};

// HTML implementation:
<button type="reset">Clear Form</button>
```

### 30. Formik Component Reusability (Introduction)
**Why:** By this point, you've learned to build forms fast, but writing `<Field>`, `<ErrorMessage>`, and wrapping `<div>` elements for every input across a large application violates the DRY (Don't Repeat Yourself) principle and leads to inconsistent styling.

**How:** The next phase is architectural. We will build a central switchboard component called `FormikControl`. Instead of manually wiring elements, we will simply call `<FormikControl control="input" name="email" />` or `<FormikControl control="select" name="course" />`, allowing a single reusable component to dynamically render the correct, fully-wired Formik field.

---

## Part 4: Reusable Component Architecture (Videos 31 - 40)

### 31. Reusable Formik Controls (Architecture Overview)
**Why:** In a real-world application, you don't want to rewrite the wrapper `<div>`, `<label>`, `<Field>`, and `<ErrorMessage>` every time you need an input. This leads to massive code duplication and inconsistent UI.

**How:** We create a single "switchboard" component called `FormikControl`. It accepts a `control` prop (e.g., 'input', 'select', 'radio') and delegates the rendering to specific, highly reusable UI components.

```jsx
// FormikControl.jsx
import React from 'react';
import Input from './Input';
import Textarea from './Textarea';
import Select from './Select';
import RadioButtons from './RadioButtons';
import CheckboxGroup from './CheckboxGroup';
import DatePicker from './DatePicker';

function FormikControl({ control, ...rest }) {
  switch (control) {
    case 'input': return <Input {...rest} />;
    case 'textarea': return <Textarea {...rest} />;
    case 'select': return <Select {...rest} />;
    case 'radio': return <RadioButtons {...rest} />;
    case 'checkbox': return <CheckboxGroup {...rest} />;
    case 'date': return <DatePicker {...rest} />;
    default: return null;
  }
}
export default FormikControl;
```

### 32. Reusable Input Component
**Why:** To standardize text inputs across the entire application.
**How:** Extract the common boilerplate into an `Input.jsx` component. It receives the label, name, and any other HTML attributes via the `...rest` spread operator.

```jsx
import React from 'react';
import { Field, ErrorMessage } from 'formik';
import TextError from './TextError'; // Custom error wrapper

function Input(props) {
  const { label, name, ...rest } = props;
  return (
    <div className="form-control">
      <label htmlFor={name}>{label}</label>
      <Field id={name} name={name} {...rest} />
      <ErrorMessage name={name} component={TextError} />
    </div>
  );
}
export default Input;
```

### 33. Reusable Textarea Component
**Why:** To standardize multi-line text inputs.
**How:** Almost identical to the Input component, but we pass `as="textarea"` to the `<Field>` component so Formik knows to render the correct HTML element.

```jsx
function Textarea(props) {
  const { label, name, ...rest } = props;
  return (
    <div className="form-control">
      <label htmlFor={name}>{label}</label>
      <Field as="textarea" id={name} name={name} {...rest} />
      <ErrorMessage name={name} component={TextError} />
    </div>
  );
}
```

### 34. Reusable Select (Dropdown) Component
**Why:** Dropdowns require a list of `<option>` tags. Hardcoding them is bad practice. We need a reusable component that maps over an array of option objects dynamically.
**How:** We pass an `options` array as a prop. Each object in the array should have a `key` (display text) and `value` (the actual data saved to Formik state).

```jsx
function Select(props) {
  const { label, name, options, ...rest } = props;
  return (
    <div className="form-control">
      <label htmlFor={name}>{label}</label>
      <Field as="select" id={name} name={name} {...rest}>
        {options.map(option => {
          return (
            <option key={option.value} value={option.value}>
              {option.key}
            </option>
          );
        })}
      </Field>
      <ErrorMessage name={name} component={TextError} />
    </div>
  );
}
```

### 35. Reusable Radio Buttons
**Why:** Native radio buttons share the same `name` attribute but have distinct `value` attributes. Formik needs exact control over this to know which option is currently selected.
**How:** We use the `<Field>` Render Props pattern to gain access to the field object. We map over an options array and manually bind the checked state to Formik's current value.

```jsx
function RadioButtons(props) {
  const { label, name, options, ...rest } = props;
  return (
    <div className="form-control">
      <label>{label}</label>
      <Field name={name} {...rest}>
        {({ field }) => {
          return options.map(option => {
            return (
              <React.Fragment key={option.value}>
                <input
                  type="radio"
                  id={option.value}
                  {...field} // passes name, onBlur, onChange
                  value={option.value}
                  checked={field.value === option.value}
                />
                <label htmlFor={option.value}>{option.key}</label>
              </React.Fragment>
            );
          });
        }}
      </Field>
      <ErrorMessage name={name} component={TextError} />
    </div>
  );
}
```

### 36. Reusable Checkbox Group
**Why:** Checkboxes are identical in structure to radio buttons, but instead of holding a single string, the Formik state holds an array of selected strings.
**How:** The implementation is identical to the Radio Buttons component above, just change `type="radio"` to `type="checkbox"`. Formik's `onChange` handler automatically detects the checkbox type and manages the array additions/removals for you behind the scenes!

```jsx
{/* Inside the mapping function: */}
<input
  type="checkbox"
  id={option.value}
  {...field}
  value={option.value}
  checked={field.value.includes(option.value)}
/>
```

### 37. Reusable Date Picker (Integrating Third-Party UI)
**Why:** Standard HTML5 date inputs look different on every browser. Developers usually use third-party libraries (like `react-datepicker`). However, third-party libraries don't natively know about Formik's state.
**How:** Use the Render Props pattern. We extract `form.setFieldValue` to manually force Formik to update its state when the third-party date picker triggers a change.

```jsx
import DateView from 'react-datepicker';
import 'react-datepicker/dist/react-datepicker.css';

function DatePicker(props) {
  const { label, name, ...rest } = props;
  return (
    <div className="form-control">
      <label htmlFor={name}>{label}</label>
      <Field name={name}>
        {({ form, field }) => {
          const { setFieldValue } = form;
          const { value } = field;
          return (
            <DateView
              id={name}
              {...field}
              {...rest}
              selected={value}
              onChange={val => setFieldValue(name, val)} // Manual binding!
            />
          );
        }}
      </Field>
      <ErrorMessage name={name} component={TextError} />
    </div>
  );
}
```

### 38. The Formik Container
**Why:** To prove the architecture works. We create a master component (`FormikContainer`) that manages the global configuration (`initialValues`, Yup `validationSchema`, `onSubmit`), and entirely builds the UI using only our abstract `FormikControl` component.

```jsx
function FormikContainer() {
  const dropdownOptions = [
    { key: 'Select an option', value: '' },
    { key: 'Option 1', value: 'option1' }
  ];

  return (
    <Formik initialValues={...} validationSchema={...} onSubmit={...}>
      {formik => (
        <Form>
          <FormikControl control="input" type="email" label="Email" name="email" />
          <FormikControl control="select" label="Select a topic" name="selectOption" options={dropdownOptions} />
          <FormikControl control="date" label="Pick a date" name="birthDate" />
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  );
}
```

### 39. Practical App 1: Registration Form
**Why:** Applying our reusable architecture to a real-world scenario.
**How:** A user registration form requires an Email, Password, Password Confirmation (using Yup's `oneOf([Yup.ref('password'), ''])` to ensure they match), and a Radio Button group for "Mode of Contact" (Email vs. Phone). Because of our `FormikControl` setup, building this entire complex form takes less than 30 lines of declarative JSX.

### 40. Practical App 2: Login Form
**Why:** A simple implementation to show how fast new forms can be spun up.
**How:** Requires only `initialValues` (email, password), a basic Yup schema, and two `<FormikControl control="input" />` elements. What previously took 50+ lines of vanilla React state management now takes just a few minutes to write with complete validation and error handling included.

---

## Part 5: UI Libraries & Wrap Up (Videos 41 - 45)

### 41. Practical App 3: Course Enrollment Form
**Why:** To solidify the reusable `FormikControl` architecture by building a comprehensive, real-world form that utilizes almost every custom control we created (Input, Textarea, Select, Checkbox, and DatePicker).
**How:** 1. Define the `initialValues` corresponding to the required fields.
2. Create the Yup validation schema for all fields. 
3. Stack the `<FormikControl />` components declaratively.

```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function EnrollmentForm() {
  const dropdownOptions = [
    { key: 'Select your course', value: '' },
    { key: 'React', value: 'react' },
    { key: 'Angular', value: 'angular' },
    { key: 'Vue', value: 'vue' }
  ];

  const checkboxOptions = [
    { key: 'HTML', value: 'html' },
    { key: 'CSS', value: 'css' },
    { key: 'JavaScript', value: 'js' }
  ];

  const initialValues = {
    email: '',
    bio: '',
    course: '',
    skills: [],
    courseDate: null
  };

  const validationSchema = Yup.object({
    email: Yup.string().email('Invalid email').required('Required'),
    bio: Yup.string().required('Required'),
    course: Yup.string().required('Required'),
    skills: Yup.array().min(1, 'Pick at least one skill'),
    courseDate: Yup.date().required('Required').nullable()
  });

  const onSubmit = values => {
    console.log('Form data', values);
  };

  return (
    <Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
      {formik => (
        <Form>
          <FormikControl control="input" type="email" label="Email" name="email" />
          <FormikControl control="textarea" label="Bio" name="bio" />
          <FormikControl control="select" label="Course" name="course" options={dropdownOptions} />
          <FormikControl control="checkbox" label="Your skillset" name="skills" options={checkboxOptions} />
          <FormikControl control="date" label="Course date" name="courseDate" />
          
          <button type="submit" disabled={!formik.isValid}>Submit</button>
        </Form>
      )}
    </Formik>
  );
}

export default EnrollmentForm;
```

### 42. UI Library Integration (Introduction to Chakra UI)
**Why:** While writing custom CSS classes is fine for learning, modern production applications heavily rely on UI component libraries (like Material UI, Ant Design, or Chakra UI). You need to know how to connect Formik's internal state to a third-party UI library's specific components.
**How:** Install a UI library and wrap your root `<App />` component in the library's Theme Provider so the styles inject correctly.

### 43. Creating a Chakra UI Input Component
**Why:** Chakra UI has its own specific components for forms: `<FormControl>`, `<FormLabel>`, `<Input>`, and `<FormErrorMessage>`. You cannot just use Formik's standard `<Field>` without losing Chakra's built-in styling, accessibility, and focus-state features.
**How:** We create a new component `ChakraInput.jsx`. We use Formik's `<Field>` **Render Props Pattern** to extract the `field` and `meta` objects, and map them directly to Chakra's native components.

### 44. Managing Chakra UI Validation State
**Why:** UI libraries handle error styling (like turning borders red) via specific boolean props. In Chakra UI, this prop is `isInvalid` on the `<FormControl>` wrapper. We must tell Chakra exactly when a field is invalid based on Formik's state.
**How:** 1. Pass `isInvalid={form.errors[name] && form.touched[name]}` to Chakra's `<FormControl>`.
2. Spread `{...field}` directly onto Chakra's `<Input>` to hook up the typing events.
3. Pass the `meta.error` string into Chakra's `<FormErrorMessage>` component.

```jsx
// ChakraInput.jsx
import React from 'react';
import { Field } from 'formik';
import { Input, FormControl, FormLabel, FormErrorMessage } from '@chakra-ui/core'; 

function ChakraInput(props) {
  const { label, name, ...rest } = props;
  
  return (
    <Field name={name}>
      {({ field, form }) => {
        return (
          <FormControl isInvalid={form.errors[name] && form.touched[name]}>
            <FormLabel htmlFor={name}>{label}</FormLabel>
            
            {/* Spread the field props to hook Formik to Chakra's input */}
            <Input id={name} {...rest} {...field} />
            
            {/* Chakra automatically displays this ONLY if isInvalid is true */}
            <FormErrorMessage>{form.errors[name]}</FormErrorMessage>
          </FormControl>
        );
      }}
    </Field>
  );
}

export default ChakraInput;
```
*Note: Add `case 'chakraInput': return <ChakraInput {...rest} />` to the `FormikControl` switchboard to use this.*

### 45. Course Wrap Up
**Summary of the Journey:**
1. **The Basics:** Started with plain React state and moved to `useFormik` hook.
2. **Abstracting Boilerplate:** Replaced hook with Context-based components: `<Formik>`, `<Form>`, `<Field>`, `<ErrorMessage>`.
3. **Yup Integration:** Replaced manual validation with clean object schemas.
4. **Advanced Formik:** Learned `<FieldArray>`, `<FastField>`, and manual validation triggers.
5. **Reusable Architecture:** Built a scalable `FormikControl` switchboard.
6. **Third-Party Integration:** Used render props to connect Formik with `react-datepicker` and Chakra UI.

---

## Part 6: React Formik Tutorial with Yup (Nikita Dev)

### 1. Introduction: Why use Formik?
**Why:** In plain React, managing forms requires `useState` for every input and writing custom logic to validate fields and handle submission. Formik abstracts this process, automatically managing:
1. **Form State** (values)
2. **Form Validation** (errors)
3. **Form Submitting State** (isSubmitting, touched, etc.)

### 2. Using the `useFormik` Hook
**Why:** The `useFormik` hook is the most basic way to tie Formik's engine to standard HTML form elements.
**How:** Import `useFormik`, configure `initialValues` and `onSubmit`, then bind `values` and `handleChange` directly to `<input>` tags.

```jsx
import { useFormik } from 'formik';

function BasicForm() {
  const { values, handleChange, handleSubmit } = useFormik({
    initialValues: { email: '', age: '' },
    onSubmit: (values, actions) => {
      console.log(values);
      actions.resetForm(); // Resets the input fields after submission
    }
  });

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="email" 
        name="email" 
        value={values.email} 
        onChange={handleChange} 
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### 3. Schema Validation with Yup
**Why:** Writing manual if/else logic is tedious. Yup allows you to cleanly define an "Object Schema" of rules.
**How:**
1. Create a Schema using `yup.object().shape()`.
2. Map fields to types (`.string()`, `.number()`) and rules (`.email()`, `.required()`, `.min()`, `.matches()`).
3. Use `yup.ref()` to compare fields (like "Confirm Password").
4. Pass it into `useFormik` under `validationSchema`.

```jsx
import * as yup from 'yup';

const passwordRules = /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{5,}$/; 

export const basicSchema = yup.object().shape({
  email: yup.string().email('Please enter a valid email').required('Required'),
  age: yup.number().positive().integer().required('Required'),
  password: yup.string().min(5).matches(passwordRules, 'Please create a stronger password').required('Required'),
  confirmPassword: yup.string().oneOf([yup.ref('password'), null], 'Passwords must match').required('Required')
});

// Linking to Formik:
const { values, errors, touched, handleBlur, handleChange, handleSubmit } = useFormik({
  initialValues: { email: '', age: '', password: '', confirmPassword: '' },
  validationSchema: basicSchema, 
  onSubmit: onSubmitHandler
});
```

### 4. Displaying Validation Errors and UX (`touched`)
**Why:** If you just check for errors, users see red text before typing. Use the `touched` state (via `onBlur`) to ensure errors only show after the user interacts and clicks away.
**How:** Add `onBlur={handleBlur}` to inputs. Render errors based on `errors.fieldName && touched.fieldName`.

```jsx
<input 
  name="email" 
  value={values.email} 
  onChange={handleChange} 
  onBlur={handleBlur} 
  className={errors.email && touched.email ? 'input-error' : ''} 
/>
{errors.email && touched.email && <p className="error">{errors.email}</p>}
```

### 5. Form Submitting State (`isSubmitting`)
**Why:** To prevent duplicate submissions while an API request is processing.
**How:** Extract `isSubmitting` from `useFormik` and apply it to the submit button's disabled state.

```jsx
const { isSubmitting } = useFormik({ ... });

<button disabled={isSubmitting} type="submit">Submit</button>
```

### 6. Formik Context Components (`<Formik>`, `<Form>`, `<Field>`)
**Why:** Spreading state across every input is repetitive. Formik provides declarative components to map this automatically.
**How:** Wrap the form area in `<Formik>`, replace forms with `<Form>`, and replace inputs with `<Field name="fieldName" />`.

```jsx
import { Formik, Form, Field } from 'formik';

<Formik initialValues={{ name: '' }} validationSchema={schema} onSubmit={submitHandler}>
  {() => (
    <Form>
      <Field type="text" name="name" placeholder="Enter Name" />
      <button type="submit">Submit</button>
    </Form>
  )}
</Formik>
```

### 7. Advanced: Building Custom Inputs (`useField`)
**Why:** `<Field>` renders a basic HTML input. For fully custom UI components, you need to hook directly into Formik's internal engine.
**How:** Use the `useField` hook inside your custom component. It returns `[field, meta]`.
* `field`: `{ name, value, onChange, onBlur }`
* `meta`: `{ touched, error }`

```jsx
import { useField } from 'formik';

const CustomCheckbox = ({ label, ...props }) => {
  const [field, meta] = useField(props);

  return (
    <div className="checkbox">
      <input 
        {...field} 
        {...props} 
        className={meta.touched && meta.error ? 'input-error' : ''}
      />
      <span>{label}</span>
      
      {meta.touched && meta.error && <div className="error">{meta.error}</div>}
    </div>
  );
};

export default CustomCheckbox;

// Usage:
<Form>
  <CustomCheckbox type="checkbox" name="acceptedTos" label="I accept the Terms of Service" />
</Form>
```