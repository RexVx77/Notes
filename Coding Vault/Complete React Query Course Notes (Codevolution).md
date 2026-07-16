## 1. Introduction & Setup (Videos 1-2)
**React Query** (now TanStack Query) is a library for fetching, caching, synchronizing, and updating server state in React applications. It replaces complex `useEffect` and `useState` boilerplate.

**Setup Requirements:**
- Install React Query: `npm install react-query`
- Set up a mock backend (e.g., `json-server`) to serve REST API endpoints.
- Wrap the app in `QueryClientProvider`.

```jsx
import { QueryClientProvider, QueryClient } from 'react-query';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/super-heroes" element={<RQSuperHeroesPage />} />
        </Routes>
      </BrowserRouter>
    </QueryClientProvider>
  );
}
export default App;
```

---

## 2. Basic Data Fetching (Video 3)
The `useQuery` hook requires two main arguments:
1. **Query Key:** A unique string or array identifying the query.
2. **Fetcher Function:** A function returning a Promise (e.g., an Axios GET request).

```jsx
import { useQuery } from 'react-query';
import axios from 'axios';

const fetchSuperHeroes = () => axios.get('http://localhost:4000/superheroes');

export const RQSuperHeroesPage = () => {
  // isLoading: initial fetch only. 
  // isFetching: true whenever a background fetch happens.
  const { isLoading, isError, error, data, isFetching } = useQuery(
    'super-heroes', 
    fetchSuperHeroes
  );

  if (isLoading) return <h2>Loading...</h2>;
  if (isError) return <h2>{error.message}</h2>;

  return (
    <div>
      {data?.data.map((hero) => <div key={hero.name}>{hero.name}</div>)}
    </div>
  );
};
```

---

## 3. React Query Devtools (Video 4)
Devtools provide a visual UI to inspect the cache, query states (fresh, fetching, stale, inactive), and manually trigger refetches.

```jsx
import { ReactQueryDevtools } from 'react-query/devtools';

// Place this right before closing </QueryClientProvider>
<ReactQueryDevtools initialIsOpen={false} position="bottom-right" />
```

---

## 4. Caching & Stale Time (Videos 5-6)

### Cache Time (`cacheTime`)
- Determines how long inactive queries remain in memory before being garbage collected.
- **Default:** 5 minutes.

### Stale Time (`staleTime`)
- Determines how long data is considered "fresh" before React Query triggers a background refetch upon component remount or window focus.
- **Default:** `0` (Data is instantly stale, triggering background refetches).

```jsx
useQuery('super-heroes', fetchSuperHeroes, {
  cacheTime: 300000, // 5 minutes
  staleTime: 30000,  // Data remains fresh for 30 seconds
});
```

---

## 5. Refetching Options (Videos 7-8)

### Refetch Defaults
- `refetchOnMount`: Refetch when component mounts (true / false / 'always'). Default: `true`.
- `refetchOnWindowFocus`: Refetch when the user switches browser tabs and comes back. Default: `true`.

### Polling (Interval Refetching)
Polling fetches data at regular intervals automatically.

```jsx
useQuery('super-heroes', fetchSuperHeroes, {
  refetchInterval: 2000, // Refetch every 2 seconds
  refetchIntervalInBackground: true, // Keep polling even when tab is out of focus
});
```

---

## 6. Fetching on Click (Video 9)
To prevent `useQuery` from firing automatically on mount, set `enabled: false`. You can then manually trigger it using the `refetch` function provided by the hook.

```jsx
const { data, isLoading, isFetching, refetch } = useQuery(
  'super-heroes', 
  fetchSuperHeroes, 
  { enabled: false }
);

return <button onClick={refetch}>Fetch Heroes</button>;
```

---

## 7. Callbacks & Data Transformation (Videos 10-11)

### Success & Error Callbacks
Execute side effects after a query finishes.

```jsx
const onSuccess = (data) => console.log('Fetch successful', data);
const onError = (error) => console.log('Fetch failed', error);

useQuery('super-heroes', fetchSuperHeroes, { onSuccess, onError });
```

### Data Transformation (`select`)
Extract or format specific parts of the fetched data before it reaches the component. This reduces rendering logic.

```jsx
const { data: superHeroNames } = useQuery(
  'super-heroes', 
  fetchSuperHeroes, 
  {
    select: (data) => {
      // Returns ONLY an array of hero names instead of the whole Axios response
      return data.data.map((hero) => hero.name);
    }
  }
);
```

---

## 8. Custom Hooks & Query by ID (Videos 12-13)

### Custom Query Hook
Abstracting `useQuery` into a custom hook keeps components clean and makes the query reusable.

```jsx
// hooks/useSuperHeroesData.js
import { useQuery } from 'react-query';
import axios from 'axios';

export const useSuperHeroesData = (onSuccess, onError) => {
  return useQuery('super-heroes', () => axios.get('http://localhost:4000/superheroes'), {
    onSuccess,
    onError,
  });
};
```

### Query by ID
When fetching a specific item, the query key must be an array. `queryKey` is passed automatically to the fetcher function context.

```jsx
const fetchSuperHero = ({ queryKey }) => {
  const heroId = queryKey[1]; 
  return axios.get(`http://localhost:4000/superheroes/${heroId}`);
};

export const useSuperHeroData = (heroId) => {
  // Query Key includes the ID: ['super-hero', '1']
  return useQuery(['super-hero', heroId], fetchSuperHero);
};
```

---

## 9. Advanced Queries (Videos 14-16)

### Parallel Queries
Simply invoke `useQuery` multiple times in the same component. Use aliases to avoid naming collisions.

```jsx
const { data: superHeroes } = useQuery('super-heroes', fetchSuperHeroes);
const { data: friends } = useQuery('friends', fetchFriends);
```

### Dynamic Parallel Queries
If the number of queries needed is dynamic (e.g., fetching details for an array of IDs), use `useQueries`.

```jsx
import { useQueries } from 'react-query';

export const DynamicParallelPage = ({ heroIds }) => {
  const queryResults = useQueries(
    heroIds.map((id) => ({
      queryKey: ['super-hero', id],
      queryFn: () => axios.get(`http://localhost:4000/superheroes/${id}`),
    }))
  );
};
```

### Dependent Queries
Queries that must wait for another query to finish before they can execute.
Using `!!` can give false when `channelId` can be `0`. Instead use `status === 'success'`as the enabled value.

```jsx
// 1. Fetch user to get channelId
const { data: user } = useQuery(['user', email], () => fetchUserByEmail(email));
const channelId = user?.data.channelId;

// 2. Fetch courses ONLY if channelId exists
useQuery(['courses', channelId], () => fetchCourses(channelId), {
  enabled: !!channelId, // Double bang converts to boolean
});
```

---

## 10. Initial Data & Pagination (Videos 17-18)

### Initial Query Data
Populate a query's initial cache with data from *another* query to prevent a loading state. 

```jsx
import { useQuery, useQueryClient } from 'react-query';

export const useSuperHeroData = (heroId) => {
  const queryClient = useQueryClient();

  return useQuery(['super-hero', heroId], fetchSuperHero, {
    initialData: () => {
      const hero = queryClient.getQueryData('super-heroes')
        ?.data?.find((hero) => hero.id === parseInt(heroId));
      
      return hero ? { data: hero } : undefined;
    }
  });
};
```

### Paginated Queries
Use `keepPreviousData: true` so the UI doesn't jump back to a loading spinner when switching pages.

```jsx
const [page, setPage] = useState(1);
const { data, isPreviousData } = useQuery(
  ['colors', page], 
  () => fetchColors(page), 
  { keepPreviousData: true }
);

// isPreviousData helps disable the 'Next' button if you've hit the end
```

---

## 11. Infinite Queries (Video 19)
Used for "Load More" functionality or infinite scrolling. Instead of tracking the page in state, React Query handles the `pageParam`.

```jsx
import { useInfiniteQuery } from 'react-query';

const fetchColors = ({ pageParam = 1 }) => {
  return axios.get(`http://localhost:4000/colors?_limit=2&_page=${pageParam}`);
};

export const InfiniteQueriesPage = () => {
  const { data, hasNextPage, fetchNextPage } = useInfiniteQuery(
    'colors', 
    fetchColors, 
    {
      getNextPageParam: (_lastPage, pages) => {
        // Condition based on your total pages
        if (pages.length < 4) return pages.length + 1;
        return undefined; // Indicates no more pages
      }
    }
  );

  return (
    <>
      {data?.pages.map((group, i) => (
        <React.Fragment key={i}>
          {group.data.map(color => <h2 key={color.id}>{color.label}</h2>)}
        </React.Fragment>
      ))}
      <button disabled={!hasNextPage} onClick={fetchNextPage}>Load More</button>
    </>
  );
};
```

---

## 12. Mutations & State Updates (Videos 20-23)
`useMutation` is for Create, Update, and Delete operations.

### Basic Mutation & Invalidation
To keep the UI in sync, you must invalidate the `GET` query after a successful mutation so React Query refetches the list.

```jsx
import { useMutation, useQueryClient } from 'react-query';

const addSuperHero = (hero) => axios.post('http://localhost:4000/superheroes', hero);

export const useAddSuperHeroData = () => {
  const queryClient = useQueryClient();

  return useMutation(addSuperHero, {
    onSuccess: () => {
      // Tells React Query the cache is invalid, triggering a background refetch
      queryClient.invalidateQueries('super-heroes');
    }
  });
};
```

### Handling Mutation Responses (Alternative to Invalidation)
Instead of refetching, update the cache directly using the data returned by the POST request. Saves a network call!

```jsx
onSuccess: (response) => {
  queryClient.setQueryData('super-heroes', (oldQueryData) => {
    return {
      ...oldQueryData,
      data: [...oldQueryData.data, response.data]
    };
  });
}
```

### Optimistic Updates
Update the UI *before* the server responds. Roll back if it fails.
1. `onMutate`: Cancel ongoing queries, snapshot old data, update cache optimistically.
2. `onError`: Restore old data using the snapshot.
3. `onSettled`: Invalidate to ensure the server and client are identical.

```jsx
export const useAddSuperHeroData = () => {
  const queryClient = useQueryClient();

  return useMutation(addSuperHero, {
    onMutate: async (newHero) => {
      await queryClient.cancelQueries('super-heroes');
      const previousHeroData = queryClient.getQueryData('super-heroes');
      
      queryClient.setQueryData('super-heroes', (oldQueryData) => ({
        ...oldQueryData,
        data: [...oldQueryData.data, { id: Date.now(), ...newHero }]
      }));
      
      return { previousHeroData };
    },
    onError: (_error, _hero, context) => {
      queryClient.setQueryData('super-heroes', context.previousHeroData);
    },
    onSettled: () => {
      queryClient.invalidateQueries('super-heroes');
    }
  });
};
```

---

## 13. Axios Interceptor Integration (Video 24-25)
To handle authentication or headers globally, you configure an Axios instance rather than calling `axios.get` directly everywhere.

```javascript
// utils/axios-utils.js
import axios from 'axios';

const client = axios.create({ baseURL: 'http://localhost:4000' });

export const request = ({ ...options }) => {
  client.defaults.headers.common.Authorization = `Bearer my-secret-token`;
  
  const onSuccess = (response) => response;
  const onError = (error) => {
    // Optionally catch global errors or redirect to login
    return Promise.reject(error);
  };

  return client(options).then(onSuccess).catch(onError);
};
```

Then use this `request` wrapper in your fetcher functions:
```jsx
// Instead of axios.get(...)
const fetchSuperHeroes = () => request({ url: '/superheroes' });
```