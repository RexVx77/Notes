
This document covers comprehensive notes from the "[React Performance Optimization Patterns](https://www.youtube.com/watch?v=keTcXT145CI)" course, explaining the theory, anti-patterns, and code-based solutions for building high-performing React applications.

---

## 1. Re-rendering & Memoization

In React, a component re-renders when its state, props, or context changes. By default, when a parent component re-renders, all of its children re-render as well, regardless of whether their props have changed. We can optimize this using **Memoization**.

### `React.memo`
`React.memo` is a Higher-Order Component (HOC) that caches the rendered output of a component. The child component will only re-render if its props have strictly changed.

```jsx
import React, { useState, memo } from 'react';

// Wrap the child component in React.memo
const ProfileCard = memo(({ name }) => {
  console.log("ProfileCard Rendered");
  return <div>Profile: {name}</div>;
});

export default function ParentComponent() {
  const [value, setValue] = useState("");

  return (
    <div>
      {/* Typing in the input changes parent state, but ProfileCard will NOT re-render
          because its 'name' prop ("Tapas") remains completely static. */}
      <input type="text" onChange={(e) => setValue(e.target.value)} value={value} />
      <ProfileCard name="Tapas" />
    </div>
  );
}
```

### `useCallback`
Inline arrow functions get re-created (receive a new memory reference) on every re-render. If you pass an inline function as a prop to a memoized child component, the memoization breaks because the prop's reference changes. `useCallback` caches the function reference across re-renders.

```jsx
import React, { useState, useCallback, memo } from 'react';

const Child = memo(({ onClick }) => {
  console.log("Child Rendered");
  return <button onClick={onClick}>Child Button</button>;
});

export default function Parent() {
  const [count, setCount] = useState(0);

  // useCallback stabilizes the function reference. 
  // It only recreates the function if dependencies in the array change.
  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []); // Empty dependency array means the reference never changes

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      
      {/* Because handleClick's reference is stable, Child will not re-render 
          when 'count' state changes */}
      <Child onClick={handleClick} />
    </div>
  );
}
```

### `useMemo`
Used to cache the *result* of a heavy calculation or transformation (like sorting or filtering a huge array) so it doesn't re-run on every render unless its dependencies change.

```jsx
import React, { useState, useMemo } from 'react';

export default function ExpensiveComponent({ usersList }) {
  const [count, setCount] = useState(0);

  // useMemo caches the sorted list. 
  // Sorting only runs again if 'usersList' changes, NOT when 'count' changes.
  const sortedUsers = useMemo(() => {
    console.log("Sorting expensive list...");
    // Create a copy to avoid mutating the original array
    return [...usersList].sort((a, b) => a.name.localeCompare(b.name));
  }, [usersList]); 

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      <ul>
        {sortedUsers.map(user => <li key={user.id}>{user.name}</li>)}
      </ul>
    </div>
  );
}
```

---

## 2. The Derived State Anti-pattern

**Derived State** is any value that can be computed directly from existing state or props. Storing derived state in its own `useState` and updating it via `useEffect` is an anti-pattern. It causes unnecessary double re-renders and synchronization bugs.

### ❌ Bad Practice (Using useEffect)
```jsx
const Cart = ({ items }) => {
  const [total, setTotal] = useState(0);

  // Anti-pattern: Triggers a second render cycle
  useEffect(() => {
    const sum = items.reduce((acc, item) => acc + item.price, 0);
    setTotal(sum);
  }, [items]);

  return <div>Total: {total}</div>;
};
```

### ✅ Good Practice (Compute on the fly)
```jsx
const Cart = ({ items }) => {
  // Compute directly during the render phase. No extra state or hooks needed!
  const total = items.reduce((acc, item) => acc + item.price, 0);
  
  return <div>Total: {total}</div>;
};
```

---

## 3. Debouncing & Throttling

When responding to fast-firing events (like keystrokes or scrolling), API calls or DOM updates can crush performance. 

### Debouncing
Debouncing delays the execution of a function until a specified time has elapsed *since the last event*. Ideal for search inputs.

```jsx
import { useState, useEffect } from 'react';

// Custom Hook: useDebounce
function useDebounce(value, delay = 500) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    // Set a timeout to update the debounced value
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // Cleanup: If the value changes before the delay passes, clear the timeout
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

// Usage in Component
export default function SearchBox() {
  const [query, setQuery] = useState("");
  // debouncedQuery only updates 600ms after the user STOPS typing
  const debouncedQuery = useDebounce(query, 600);

  useEffect(() => {
    if (debouncedQuery) {
      console.log("Making API call for:", debouncedQuery);
    }
  }, [debouncedQuery]); // Runs ONLY when the debounced value updates

  return <input type="text" onChange={(e) => setQuery(e.target.value)} value={query} />;
}
```

### Throttling
Throttling guarantees that a function executes *at most once* within a specified time span. Ideal for scroll tracking or window resizing.

```jsx
import { useState, useRef, useEffect } from 'react';

// Custom Hook: useThrottle
function useThrottle(value, delay = 1000) {
  const [throttledValue, setThrottledValue] = useState(value);
  const lastExecuted = useRef(Date.now());

  useEffect(() => {
    const now = Date.now();
    // If enough time has passed, update the value and the ref
    if (now - lastExecuted.current >= delay) {
      setThrottledValue(value);
      lastExecuted.current = now;
    }
  }, [value, delay]);

  return throttledValue;
}
```

---

## 4. React 19 Compiler

React 19 introduces the **React Compiler**, which automatically memoizes components, objects, and function references behind the scenes. 
- You no longer need to write `React.memo`, `useMemo`, or `useCallback`.
- It drastically reduces boilerplate code.
- Configured via `babel-plugin-react-compiler`.

If you ever need to opt a specific component out of the compiler's auto-memoization, you can add `"use no memo"` at the top of the function:
```jsx
function ProblematicComponent() {
  "use no memo";
  // component logic...
}
```

---

## 5. Code Splitting & Lazy Loading

Large applications generate huge JavaScript bundles. Code Splitting / Lazy Loading ensures components are only downloaded when needed (e.g. hidden modals or routes).

```jsx
import React, { useState, Suspense, lazy } from 'react';

// Dynamically import the heavy component. It won't download initially!
const HeavyComponent = lazy(() => import('./HeavyComponent'));
import LightComponent from './LightComponent'; // Static import

export default function App() {
  const [showHeavy, setShowHeavy] = useState(false);

  return (
    <div>
      <LightComponent />
      <button onClick={() => setShowHeavy(!showHeavy)}>Toggle Heavy Component</button>

      {/* Suspense shows a fallback UI while the heavy chunk downloads */}
      {showHeavy && (
        <Suspense fallback={<div>Loading heavy component...</div>}>
          <HeavyComponent />
        </Suspense>
      )}
    </div>
  );
}
```

---

## 6. Context Optimization

Using one massive, global context (e.g. `AppProvider` storing User, Theme, and Cart) forces *all* consuming components to re-render whenever *any* piece of that context changes.

**Optimization Pattern:** Split contexts by logical domains.

```jsx
// ❌ Bad: Monolithic Context
<GlobalProvider> 
  <App />
</GlobalProvider>

// ✅ Good: Granular Contexts
<ThemeProvider>
  <UserProvider>
    <CartProvider>
      <App />
    </CartProvider>
  </UserProvider>
</ThemeProvider>
```
If a component only consumes `ThemeContext`, it won't care (or re-render) if `UserContext` updates.

---

## 7. Virtualization

When rendering thousands of items in a list, rendering every DOM node crashes the browser. Virtualization libraries (like `react-window`) fix this by only rendering the elements currently visible in the scroll viewport.

```jsx
import { FixedSizeList as List } from 'react-window';

const Row = ({ index, style }) => (
  <div style={style}>Row {index}</div>
);

export default function VirtualizedList() {
  return (
    // Instead of mapping over 10,000 items and making 10,000 <div>s, 
    // this renders only the visible rows and recycles them on scroll.
    <List
      height={400} // Viewport height
      itemCount={10000} // Total number of items
      itemSize={35} // Height of each row
      width={300}
    >
      {Row}
    </List>
  );
}
```

---

## 8. Concurrent Rendering 

When you have a high-priority update (like an input keystroke) competing with a low-priority heavy update (like filtering a massive list), React 18+ provides concurrent hooks to prevent the UI from freezing.

### `useTransition`
Controls the state update *function*.

```jsx
import { useState, useTransition } from 'react';

export default function FilterList() {
  const [text, setText] = useState("");
  const [query, setQuery] = useState("");
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    const val = e.target.value;
    
    // Urgent update: The input box updates immediately
    setText(val); 

    // Non-urgent update: The list filtering can wait if the user is typing fast
    startTransition(() => {
      setQuery(val);
    });
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} />
      {isPending ? <p>Loading...</p> : <HeavyList query={query} />}
    </div>
  );
}
```

### `useDeferredValue`
Controls the computed *value* directly, acting somewhat like an automatic concurrent debounce.

```jsx
import { useState, useDeferredValue } from 'react';

export default function App() {
  const [text, setText] = useState("");
  // React will automatically defer this value during heavy updates
  const deferredText = useDeferredValue(text); 

  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <HeavyList query={deferredText} />
    </div>
  );
}
```

---

## 9. Stable Keys

When mapping arrays, never use the index as a `key`.

```jsx
// ❌ Bad: React loses track if items are inserted/removed/sorted, 
// breaking memoization and destroying/recreating DOM nodes.
{items.map((item, index) => <li key={index}>{item.name}</li>)}

// ✅ Good: Use a stable unique identifier from your data.
{items.map(item => <li key={item.id}>{item.name}</li>)}
```