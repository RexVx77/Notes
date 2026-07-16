## Packages

### Core Framework

- **React 17** — UI library for building component-based interfaces
- **React DOM** — Renders React components to the browser DOM
- **TypeScript 4.4** — Adds static typing to JavaScript

### Routing

- **react-router-dom v5** — Client-side routing — `Route`, `Switch`, `useHistory`, `useParams`

### State Management & Data Fetching

- **@apollo/client** — GraphQL client — manages queries, mutations, caching, and local state
- **graphql** — Core GraphQL language parser/utilities (peer dep of Apollo)
- **@tanstack/react-query v4** — Server-state management for REST APIs — caching, refetching, background updates
- **restful-react** — Declarative REST API hooks (code-generated from OpenAPI specs)
- **immer** — Immutable state updates using mutable-style syntax

### UI Component Libraries

- **@harness/uicore** — Harness's internal component library — Button, Layout, Text, Container, etc.
- **@harness/design-system** — Design tokens — Color, FontVariation, Intent, Spacing constants
- **@harness/icons** — Icon set used across Harness products
- **@blueprintjs/core** — Base UI toolkit (Blueprint.js) — menus, dialogs, toasts, overlays
- **@blueprintjs/datetime** — Date and time picker components
- **@blueprintjs/select** — Advanced select/dropdown components
- **@harness/help-panel** — Contextual help panel sidebar
- **@harness/use-modal** — Hook-based modal dialog management
- **@harness/ng-tooltip** — Tooltip component with Harness styling
- **@harnessio/filters** — Reusable filter UI components
- **@popperjs/core + react-popper** — Tooltip/popover positioning engine

### Forms & Validation

- **formik** — Form state management — handles values, validation, submission, errors
- **yup** — Schema-based form validation (used with Formik)

### Tables

- **@tanstack/react-table v8** — Headless table library — sorting, filtering, pagination (newer)
- **react-table v7** — Older headless table library (legacy usage in codebase)

### Charts & Visualization

- **highcharts + highcharts-react-official** — Rich charting library — line, bar, pie, area charts
- **reactflow** — Node-based graph/diagram editor (used for Chaos Studio workflow canvas)
- **elkjs** — Graph layout algorithm engine (auto-positions nodes in ReactFlow diagrams)

### Code Editor

- **react-monaco-editor** — Monaco editor (the engine behind VS Code) embedded in the browser for YAML/code editing

### Styling

- **sass** — SCSS preprocessor — variables, nesting, mixins, functions
- **classnames** — Utility for conditionally joining CSS class names
- **normalize.css** — CSS reset for consistent cross-browser base styles

### Utilities

- **lodash-es** — Utility belt — `get`, `set`, `debounce`, `cloneDeep`, `isEmpty`, etc.
- **moment + moment-timezone** — Date/time parsing, formatting, and timezone handling
- **uuid** — Generates unique IDs (UUIDv4)
- **qs** — URL query string parsing and stringifying
- **yaml** — Parses and serializes YAML (used for experiment manifests)
- **marked** — Converts Markdown text to HTML
- **cron-parser** — Parses cron expressions into dates
- **cronstrue** — Converts cron expressions to human-readable strings
- **clipboard-copy** — Copies text to clipboard
- **linkifyjs** — Detects and converts URLs in text to links
- **anser** — Parses ANSI escape codes to HTML (for terminal/log output)
- **jsonschema** — JSON Schema validation
- **idb** — Promise-based wrapper around IndexedDB for local storage
- **masonry-layout** — Pinterest-style grid layout
- **react-virtuoso** — Virtualized list rendering for large datasets
- **react-draggable** — Makes elements draggable
- **react-to-print** — Print React components
- **react-timeago** — Displays relative timestamps ("3 minutes ago")
- **react-error-boundary** — Declarative error boundary component
- **event-source-polyfill** — Polyfill for Server-Sent Events (SSE)

### Micro-frontends & Architecture

- **@harness/microfrontends** — Module Federation wrapper — this app runs as a micro-frontend inside the Harness platform
- **external-remotes-plugin** — Webpack plugin for loading remote micro-frontend modules at runtime

### Build Toolchain

- **webpack 5** — Module bundler — bundles JS, CSS, assets
- **webpack-cli** — CLI for running webpack commands
- **webpack-dev-server** — Local dev server with HMR (Hot Module Replacement)
- **webpack-merge** — Merges webpack config objects (dev vs prod)
- **ts-loader** — Webpack loader for TypeScript
- **css-loader** — Resolves CSS imports in JS
- **style-loader** — Injects CSS into the DOM at dev time
- **sass-loader** — Compiles SCSS to CSS in webpack
- **mini-css-extract-plugin** — Extracts CSS into separate files for production
- **html-webpack-plugin** — Generates the HTML file with script tags
- **fork-ts-checker-webpack-plugin** — Runs TypeScript type-checking in a separate process
- **tsconfig-paths-webpack-plugin** — Resolves TypeScript path aliases in webpack
- **circular-dependency-plugin** — Detects circular `import` dependencies
- **webpack-retry-chunk-load-plugin** — Retries failed chunk loads (network resilience)

### Code Generation

- **@graphql-codegen/cli** — Generates TypeScript types and hooks from `.gql` files
- **@graphql-codegen/typescript** — Plugin to generate base TS types from GraphQL schema
- **@graphql-codegen/typescript-operations** — Generates types for GraphQL operations
- **@graphql-codegen/typescript-react-apollo** — Generates React Apollo hooks from GraphQL operations
- **@harnessio/oats-cli** — Generates REST API clients from OpenAPI specs
- **@harnessio/oats-plugin-react-query** — Generates React Query hooks from OpenAPI specs

### Testing

- **jest** — Test runner and assertion library
- **ts-jest** — TypeScript preprocessor for Jest
- **@testing-library/react** — Renders React components for testing, queries the DOM
- **@testing-library/react-hooks** — Tests custom React hooks in isolation
- **@testing-library/user-event** — Simulates realistic user interactions in tests
- **@testing-library/jest-dom** — Custom Jest matchers for DOM assertions
- **@apollo/react-testing** — MockedProvider for testing Apollo GraphQL components
- **fake-indexeddb** — In-memory IndexedDB for testing
- **identity-obj-proxy** — Mocks CSS module imports in tests

### Code Quality

- **eslint** — Linter for catching code issues
- **prettier** — Code formatter for consistent style
- **husky** — Git hooks (runs lint/format/typecheck on commit)
- **lint-staged** — Runs linters only on staged files for speed
- **@typescript-eslint/parser + plugin** — ESLint support for TypeScript

### Internationalization (i18n)

- **YAML strings files** (custom) — Translations live in `strings.en.yaml`, loaded via `useStrings()` hook
- **yaml-loader** — Webpack loader for importing YAML files
- **yaml-sort** — Sorts YAML keys alphabetically

---

## Learning Path

> [!info] How to use this
> Each tier builds on the previous one. Master each tier before moving to the next. You can start contributing to the codebase after **Tier 5** (~8 weeks), and be fully proficient by **~15 weeks**.

---

### Tier 1 — Foundations `Weeks 1–3`

> [!abstract] Without these, nothing else makes sense.

1. **TypeScript**
	- Types, interfaces, generics, union types
	- Utility types: `Partial`, `Pick`, `Omit`, `Record`
	- This codebase is 100% TypeScript
2. **React 17**
	- Functional components, JSX, props, state, lifecycle
	- Hooks: `useState`, `useEffect`, `useRef`, `useMemo`, `useCallback`, `useContext`
3. **React Router v5**
	- `Route`, `Switch`, `Redirect`, `Link`
	- `useHistory`, `useParams`, `useLocation`
	- Nested routing

> [!example] Practice
> Build a multi-page app with TypeScript, React, and React Router.

---

### Tier 2 — Styling & UI `Weeks 3–4`

> [!abstract] How things look.

4. **SCSS & CSS Modules**
	- Variables, nesting, mixins, `@import`/`@use`
	- CSS Modules scoping (`styles.container`)
5. **classnames**
	- Conditional class application: `cx(styles.base, { [styles.active]: isActive })`
6. **Blueprint.js**
	- Go through `@blueprintjs/core` docs
	- Component patterns: Intent, controlled vs uncontrolled
7. **Harness UICore**
	- Wraps Blueprint with Harness-specific patterns
	- Explore once you know Blueprint

> [!example] Practice
> Rebuild a settings page using Blueprint components and SCSS modules.

---

### Tier 3 — Forms & Validation `Week 5`

> [!abstract] How users input data.

8. **Formik**
	- `useFormik`, `<Formik>`, `<Form>`, `<Field>`, `<FieldArray>`
	- Form state, touched, errors, submission
9. **Yup**
	- Schema definition: `.string()`, `.number()`, `.required()`, `.when()`, `.test()`
	- Integration with Formik's `validationSchema`

> [!example] Practice
> Build a multi-step form with dynamic fields, conditional validation, and error display.

---

### Tier 4 — Data Fetching: GraphQL `Weeks 6–7`

> [!abstract] How the app talks to the backend (GraphQL side).

10. **GraphQL basics**
	- Queries, mutations, fragments, variables, schema
	- The type system
11. **Apollo Client**
	- `useQuery`, `useMutation`, `useApolloClient`
	- Cache policies: `cache-first`, `network-only`
	- Optimistic updates, `refetchQueries`
12. **GraphQL Code Generator**
	- How `.gql` files become typed hooks
	- Read the `graphql-codegen.yml` config

> [!example] Practice
> Write a query and mutation, generate hooks, and use them in a component.

---

### Tier 5 — Data Fetching: REST `Weeks 7–8`

> [!abstract] How the app talks to the backend (REST side).

13. **TanStack React Query v4**
	- `useQuery`, `useMutation`, `queryClient`
	- Cache invalidation, `staleTime`, `cacheTime`
	- `onSuccess` / `onError`
14. **restful-react / oats-cli**
	- REST hooks are auto-generated from OpenAPI specs
	- Read a generated service file to see the pattern

> [!example] Practice
> Use React Query to fetch, cache, and mutate REST data with loading/error states.

---

### Tier 6 — State Management Patterns `Week 9`

> [!abstract] How app-wide state flows.

15. **React Context**
	- `createContext`, `useContext`, Provider pattern
	- Used extensively: `AppStore`, `Strings`, `License`, `ParentContext`
16. **Immer**
	- `produce()` for immutable updates with mutable syntax
	- Structural sharing with Proxies under the hood

> [!example] Practice
> Build a global app store with Context + Immer that multiple components consume.

---

### Tier 7 — Tables & Lists `Week 10`

> [!abstract] Displaying structured data.

17. **@tanstack/react-table v8**
	- Column definitions, `useReactTable`
	- Sorting, filtering, pagination
	- Headless — you control all rendering
18. **react-virtuoso**
	- Virtual scrolling for large lists/tables
	- Why virtualization matters and how it works

> [!example] Practice
> Build a paginated, sortable, filterable data table with virtualized rows.

---

### Tier 8 — Charts & Visualization `Weeks 11–12`

> [!abstract] Visual representations of data.

19. **Highcharts**
	- Series, axes, tooltips, events
	- The Highcharts options object model
	- `highcharts-react-official` wrapper
20. **ReactFlow**
	- Nodes, edges, handles, custom node types
	- Event handlers, viewport controls
	- Used for the chaos experiment workflow canvas
21. **elkjs**
	- Automatic graph layout
	- How ELK computes positions for nodes

> [!example] Practice
> Build a dashboard with Highcharts and a workflow editor with ReactFlow.

---

### Tier 9 — Advanced Patterns `Weeks 12–14`

> [!abstract] Production-grade patterns used in this codebase.

22. **Monaco Editor** — Embedding the VS Code editor; language config, themes, controlled value
23. **Error Boundaries** — `react-error-boundary` for graceful failure handling
24. **Micro-frontends / Module Federation** — How this app loads inside the Harness platform; shared deps, remote entry points
25. **IndexedDB (idb)** — Client-side storage for offline data / caching

---

### Tier 10 — Build & Tooling `Weeks 14–15`

> [!abstract] How it all gets bundled and shipped.

26. **Webpack 5** — Entry points, loaders, plugins, code splitting, Module Federation, dev server
27. **ESLint + Prettier** — Custom rules, plugin ecosystem, editor integration
28. **Jest + Testing Library** — Unit tests, mocking Apollo/React Query, snapshot tests, `userEvent`
29. **Husky + lint-staged** — Pre-commit hooks, staged-only linting

> [!example] Practice
> Write tests for a component that fetches GraphQL data, uses Formik, and renders a table.

---

### Tier 11 — Utilities `Ongoing`

> [!abstract] Reach for these as needed.

30. **lodash-es** — `get`, `set`, `debounce`, `throttle`, `cloneDeep`, `groupBy`, `isEmpty`
31. **moment / moment-timezone** — Date formatting, relative time, timezone conversion
32. **qs** — Parsing/stringifying URL query params
33. **yaml** — Parsing YAML to JS objects and back
34. **cron-parser / cronstrue** — Cron expression handling
35. **marked** — Markdown to HTML conversion
36. **uuid** — Generating unique identifiers

---

## Resources

> [!tip] Bookmarks
> - **TypeScript** — [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)
> - **React** — [react.dev](https://react.dev)
> - **React Router v5** — [v5.reactrouter.com](https://v5.reactrouter.com/)
> - **Formik** — [formik.org](https://formik.org/docs/overview)
> - **Apollo Client** — [apollographql.com/docs/react](https://www.apollographql.com/docs/react/)
> - **TanStack React Query** — [tanstack.com/query](https://tanstack.com/query/v4/docs/)
> - **TanStack React Table** — [tanstack.com/table](https://tanstack.com/table/v8/docs/)
> - **Highcharts** — [highcharts.com/docs](https://www.highcharts.com/docs/)
> - **ReactFlow** — [reactflow.dev](https://reactflow.dev/docs/)
> - **Webpack** — [webpack.js.org](https://webpack.js.org/concepts/)
> - **Blueprint.js** — [blueprintjs.com](https://blueprintjs.com/docs/)
> - **Jest + Testing Library** — [testing-library.com](https://testing-library.com/docs/react-testing-library/intro/)