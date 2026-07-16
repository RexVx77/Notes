
## 1. Introduction & Setup
React Router is a fully-featured client and server-side routing library for React, enabling navigation between different URLs without full page reloads. 

**Installation:**
```bash
npm install react-router-dom@6
# or
yarn add react-router-dom@6
```

## 2. Configuring Basic Routes
To configure routing, connect your app to the browser URL using the `<BrowserRouter>` component. Use `<Routes>` to wrap your individual `<Route>` definitions.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './components/Home';
import About from './components/About';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## 3. Navigation Links
Do not use standard HTML `<a>` tags for internal links, as they cause full page reloads. Use React Router's built-in link components.

* **`<Link>`:** Used for standard navigation across different components.
* **`<NavLink>`:** Specifically meant for navigation elements like navbars, tabs, or breadcrumbs. It automatically receives an `active` class when the link matches the current route, allowing you to highlight the current section. You can also conditionally style it via the `isActive` flag.

```jsx
import { Link, NavLink } from 'react-router-dom';

// Standard Link
<Link to="/about">About Us</Link>

// NavLink with inline active styling
<NavLink 
  to="/" 
  style={({ isActive }) => ({ fontWeight: isActive ? 'bold' : 'normal' })}>
  Home
</NavLink>
```

## 4. Navigating Programmatically
Sometimes you need to trigger navigation after an event finishes (e.g., an order form submission). You can achieve this using the `useNavigate` hook.

```jsx
import { useNavigate } from 'react-router-dom';

function Order() {
  const navigate = useNavigate();

  return (
    <>
      {/* Navigate to a specific route */}
      <button onClick={() => navigate('/order-summary')}>Place Order</button>
      
      {/* Replace the history stack instead of pushing to it */}
      <button onClick={() => navigate('/order-summary', { replace: true })}>Replace History</button>
      
      {/* Go back to the previous page */}
      <button onClick={() => navigate(-1)}>Go Back</button>
    </>
  );
}
```

## 5. Catch-All (404 / No Match Route)
Handle invalid or missing URLs to improve user experience by rendering a custom "Page Not Found" component. Use the asterisk (`*`) path to match anything that hasn't been defined.

```jsx
<Route path="*" element={<PageNotFound />} />
```

## 6. Nested Routes & Outlet
Nested routes allow you to switch out only a portion of the UI on the same page. 

* **Parent Route:** Defines the main layout and uses an `<Outlet />` component to indicate where child components should render.
* **Child Routes:** Nested directly inside the parent `<Route>` block. 

```jsx
// 1. App.js - Route Configuration
import { Routes, Route } from 'react-router-dom';

<Route path="products" element={<Products />}>
  {/* Nested Routes */}
  <Route path="featured" element={<FeaturedProducts />} />
  <Route path="new" element={<NewProducts />} />
</Route>

// 2. Products.js - Parent Component
import { Link, Outlet } from 'react-router-dom';

function Products() {
  return (
    <div>
      <input type="search" placeholder="Search products" />
      <nav>
        <Link to="featured">Featured</Link>
        <Link to="new">New</Link>
      </nav>
      {/* Child nested routes will render here */}
      <Outlet /> 
    </div>
  );
}
```
*Note on Relative Links: The `to="featured"` inside the navbar above is a relative link. Because it doesn't start with a slash `/`, it automatically inherits the closest route path (`/products/featured`) without needing to spell out the absolute path.*

## 7. Index Routes
When using nested routes, you might want to display a default view at the parent level before any child links are clicked. You can do this by passing the `index` prop to a child route instead of `path`.

```jsx
<Route path="products" element={<Products />}>
  {/* This will render as the default view at the /products URL */}
  <Route index element={<FeaturedProducts />} /> 
  <Route path="featured" element={<FeaturedProducts />} />
</Route>
```

## 8. Dynamic Routes & URL Params
When dealing with list/detail patterns (e.g., specific user profiles), use dynamic route parameters denoted by a colon (`:`). Extract the parameters in your component using the `useParams` hook.

*Note: React Router is smart enough to match explicitly defined specific routes (like `/users/admin`) before trying to map them to dynamic generic routes (like `/users/:userId`).*

```jsx
// 1. App.js Config
<Route path="users/:userId" element={<UserDetails />} />

// 2. UserDetails.js
import { useParams } from 'react-router-dom';

function UserDetails() {
  const { userId } = useParams();
  return <h1>Details about user: {userId}</h1>;
}
```

## 9. Search (Query) Params
To handle search query strings in the URL (e.g., `?filter=active`), use the `useSearchParams` hook, which functions very similarly to React's `useState`.

```jsx
import { useSearchParams } from 'react-router-dom';

function Users() {
  const [searchParams, setSearchParams] = useSearchParams();
  const showActiveUsers = searchParams.get('filter') === 'active';

  return (
    <div>
      <button onClick={() => setSearchParams({ filter: 'active' })}>Active Users</button>
      <button onClick={() => setSearchParams({})}>Reset</button>

      {showActiveUsers ? <h2>Showing active users</h2> : <h2>Showing all users</h2>}
    </div>
  );
}
```

## 10. Lazy Loading Routes
To boost application performance and reduce the initial load time, you can incrementally download chunks of your application. Utilize `React.lazy()` along with `React.Suspense`.

```jsx
import React from 'react';
import { Routes, Route } from 'react-router-dom';

// 1. Use dynamic imports via React.lazy
const LazyAbout = React.lazy(() => import('./components/About'));

function App() {
  return (
    <Routes>
      {/* 2. Wrap the lazy component in Suspense with a fallback UI */}
      <Route path="about" element={
        <React.Suspense fallback={<div>Loading...</div>}>
          <LazyAbout />
        </React.Suspense>
      } />
    </Routes>
  );
}
```

## 11. Authentication & Protected Routes
To secure sensitive routes (like user dashboards), construct a reusable wrapper component that checks the user's authentication state. If they aren't logged in, redirect them.

```jsx
import { Navigate, useLocation } from 'react-router-dom';
import { useAuth } from './authContext'; // Your app's custom auth hook

// 1. The Wrapper Component
function RequireAuth({ children }) {
  const auth = useAuth();
  const location = useLocation(); // Track origin to securely redirect back after login

  if (!auth.user) {
    // Redirect to login if unauthenticated
    return <Navigate to="/login" state={{ path: location.pathname }} replace />;
  }
  
  return children;
}

// 2. App.js Configuration
<Route path="/profile" element={
  <RequireAuth>
    <Profile />
  </RequireAuth>
} />
```