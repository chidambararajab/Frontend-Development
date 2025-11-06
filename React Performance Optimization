## React Performance Optimization

Let me walk you through the essential performance topics that come up in senior React interviews and real-world applications.

---

## 1. **React.memo - Component Memoization**

**Explanation:**`React.memo` is a higher-order component that prevents re-renders when props haven't changed. It performs a shallow comparison of props by default, similar to `PureComponent` for class components.

**Real-world use case:**

```jsx
*// Expensive list item that shouldn't re-render unless its data changes*
const ListItem = React.memo(({ item, onDelete }) => {
  console.log('Rendering item:', item.id);
  return (
    <div>
      <h3>{item.title}</h3>
      <button onClick={() => onDelete(item.id)}>Delete</button>
    </div>
  );
});
```

**Common pitfalls:**

- Passing inline objects/arrays as props breaks memoization: `<ListItem data={{ id: 1 }} />` creates a new object every render
- Passing non-memoized callbacks causes re-renders even with React.memo
- Overusing it on cheap components adds more overhead than it saves

**Best practices:**

- Use for components with expensive render logic (complex calculations, large lists)
- Combine with `useMemo`/`useCallback` for reference stability
- Custom comparison: `React.memo(Component, (prevProps, nextProps) => customEquality)`
- Metrics: Measure with React DevTools Profiler - look for components with high "Render duration" (>16ms causes frame drops)

---

## 2. **useMemo - Value Memoization**

**Explanation:**`useMemo` caches computed values between renders, recalculating only when dependencies change. Returns the memoized value itself.

**Real-world use case:**

```jsx
function DataTable({ data, filters }) {
  *// Expensive filtering/sorting operation*
  const processedData = useMemo(() => {
    return data
      .filter(item => matchesFilters(item, filters))
      .sort((a, b) => a.priority - b.priority)
      .slice(0, 100);
  }, [data, filters]); *// Only recompute when these change*

  return <Table rows={processedData} />;
}
```

**Common pitfalls:**

- Empty dependency array `[]` - value never updates
- Missing dependencies - stale closures and bugs
- Using for primitive values or cheap operations (premature optimization)
- Dependency bloat causing frequent recalculations

**Best practices:**

- Use for expensive computations (>5ms execution time)
- Use for maintaining referential equality of objects/arrays passed as props
- Don't memoize everything - profile first
- Metrics: Chrome DevTools Performance tab - look for long JavaScript execution blocks

---

## 3. **useCallback - Function Memoization**

**Explanation:**`useCallback` returns a memoized callback function, preventing function recreation on every render. Essential for maintaining reference equality.

**Real-world use case:**

```jsx
function SearchableList({ items }) {
  const [query, setQuery] = useState('');
  
  *// Without useCallback, this function is recreated every render// causing child components to re-render unnecessarily*
  const handleSearch = useCallback((searchTerm) => {
    setQuery(searchTerm);
    *// Could include API calls, analytics, etc.*
  }, []); *// No dependencies - function never changes*

  return <SearchBar onSearch={handleSearch} />; *// SearchBar can be memoized now*
}
```

**Common pitfalls:**

- Forgetting to memoize callbacks passed to memoized children (negates React.memo benefits)
- Over-depending on external values, causing function to change anyway
- Not using when parent re-renders frequently but callback logic is stable

**Best practices:**

- Always use with event handlers passed to memoized children
- Combine with React.memo for maximum benefit
- Use when passing callbacks to dependency arrays of other hooks
- Metrics: Check React DevTools Profiler "Why did this render?" section

---

## 4. **Code Splitting with React.lazy & Suspense**

**Explanation:**
Dynamically import components to split bundle into smaller chunks, loaded on-demand. Reduces initial JavaScript payload.

**Real-world use case:**

```jsx
const AdminDashboard = React.lazy(() => import('./AdminDashboard'));
const UserProfile = React.lazy(() => import('./UserProfile'));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <Routes>
        <Route path="/admin" element={<AdminDashboard />} />
        <Route path="/profile" element={<UserProfile />} />
      </Routes>
    </Suspense>
  );
}
```

**Common pitfalls:**

- Not using route-based splitting (splitting too granularly)
- Missing Suspense boundary causes errors
- No loading state strategy (jarring UX)
- Creating too many tiny chunks (network overhead)

**Best practices:**

- Split at route level first
- Split heavy third-party libraries (charts, editors, maps)
- Use named chunks: `import(/* webpackChunkName: "admin" */ './Admin')`
- Preload critical chunks: `<link rel="preload" as="script" href="/chunk.js">`
- Metrics: Target <200KB initial bundle, Lighthouse "First Contentful Paint" <1.8s

---

## 5. **Virtualization (Windowing)**

**Explanation:**
Only render visible items in long lists/grids. Libraries like `react-window` or `@tanstack/react-virtual` render a "window" of items based on scroll position.

**Real-world use case:**

```jsx
import { useVirtualizer } from '@tanstack/react-virtual';

function VirtualList({ items }) {
  const parentRef = useRef();
  
  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 50, *// Estimated row height*
  });

  return (
    <div ref={parentRef} style={{ height: '600px', overflow: 'auto' }}>
      <div style={{ height: virtualizer.getTotalSize() }}>
        {virtualizer.getVirtualItems().map(virtualRow => (
          <div key={virtualRow.index} style={{
            position: 'absolute',
            top: 0,
            left: 0,
            width: '100%',
            transform: `translateY(${virtualRow.start}px)`
          }}>
            {items[virtualRow.index].name}
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Common pitfalls:**

- Not handling dynamic item sizes properly
- Missing key props on virtualized items
- Ignoring horizontal virtualization for wide tables
- Breaking accessibility (screen readers lose context)

**Best practices:**

- Use for lists >100 items
- Implement overscan for smoother scrolling
- Measure actual item sizes when possible
- Add ARIA labels for accessibility
- Metrics: 60fps scroll (16.67ms frame budget), Long Tasks <50ms

---

## 6. **useTransition & useDeferredValue (React 18+)**

**Explanation:**
Concurrent features that mark updates as non-urgent, keeping UI responsive during heavy operations. `useTransition` wraps state updates; `useDeferredValue` defers value changes.

**Real-world use case:**

```jsx
function SearchResults() {
  const [query, setQuery] = useState('');
  const [isPending, startTransition] = useTransition();
  const deferredQuery = useDeferredValue(query);

  const handleChange = (e) => {
    *// Urgent: update input immediately*
    setQuery(e.target.value);
    
    *// Or use startTransition for the state update itself:// startTransition(() => setQuery(e.target.value));*
  };

  *// Use deferredQuery for expensive operations*
  const results = useMemo(
    () => searchItems(deferredQuery),
    [deferredQuery]
  );

  return (
    <>
      <input value={query} onChange={handleChange} />
      {isPending && <Spinner />}
      <Results data={results} />
    </>
  );
}
```

**Common pitfalls:**

- Wrapping all state updates (not everything needs to be deferred)
- Not showing loading indicators (users don't know system is working)
- Confusing with debouncing (different use cases)

**Best practices:**

- Use for search, filtering, complex UI updates
- Show `isPending` state to users
- Combine with `useMemo` for expensive derived state
- Metrics: Input Delay <100ms for urgent updates, total interaction <200ms

---

## 7. **Bundle Size Optimization**

**Explanation:**
Reducing JavaScript payload through tree-shaking, lazy loading, and proper imports. Critical for mobile users and slow networks.

**Real-world use case:**

```jsx
*// ❌ Bad - imports entire lodash (70KB)*
import _ from 'lodash';
const result = _.debounce(fn, 300);

*// ✅ Good - imports only debounce (~5KB)*
import debounce from 'lodash/debounce';
const result = debounce(fn, 300);

*// ✅ Even better - use native or smaller alternatives*
const debounce = (fn, delay) => {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
};
```

**Common pitfalls:**

- Importing entire libraries instead of specific functions
- Not analyzing bundle with tools like webpack-bundle-analyzer
- Including unused dependencies
- Duplicate dependencies (different versions in bundle)

**Best practices:**

- Use `import { specific } from 'library'` with tree-shaking
- Replace heavy libraries: moment.js (288KB) → date-fns (13KB) → native Intl
- Enable production builds (minification, dead code elimination)
- Check bundle with: `npx webpack-bundle-analyzer build/static/js/*.js`
- Metrics: Target <200KB gzipped total JavaScript, Time to Interactive <3.8s

---

## 8. **Image Optimization**

**Explanation:**
Properly sized, formatted, and lazy-loaded images reduce bandwidth and improve perceived performance.

**Real-world use case:**

```jsx
function OptimizedImage({ src, alt }) {
  return (
    <picture>
      <source 
        type="image/webp" 
        srcSet={`${src}-small.webp 400w, ${src}-large.webp 800w`}
      />
      <img
        src={`${src}-small.jpg`}
        srcSet={`${src}-small.jpg 400w, ${src}-large.jpg 800w`}
        sizes="(max-width: 600px) 400px, 800px"
        alt={alt}
        loading="lazy"
        decoding="async"
      />
    </picture>
  );
}
```

**Common pitfalls:**

- Serving full-resolution images for thumbnails
- Not using modern formats (WebP, AVIF)
- Loading all images upfront
- Missing width/height attributes (layout shift)

**Best practices:**

- Use `loading="lazy"` for below-fold images
- Serve responsive images with `srcset`
- Convert to WebP (30% smaller than JPEG)
- Add explicit dimensions to prevent CLS (Cumulative Layout Shift)
- Use CDN with automatic optimization (Cloudinary, Imgix)
- Metrics: Largest Contentful Paint <2.5s, CLS <0.1

---

## 9. **Debouncing & Throttling**

**Explanation:**
Control function execution frequency. Debouncing delays execution until activity stops; throttling limits execution rate.

**Real-world use case:**

```jsx
function SearchInput() {
  const [query, setQuery] = useState('');
  
  *// Debounce: wait for user to stop typing*
  const debouncedSearch = useMemo(
    () => debounce((value) => {
      fetchSearchResults(value);
    }, 300),
    []
  );

  const handleChange = (e) => {
    const value = e.target.value;
    setQuery(value);
    debouncedSearch(value);
  };

  useEffect(() => {
    return () => debouncedSearch.cancel(); *// Cleanup*
  }, [debouncedSearch]);

  return <input value={query} onChange={handleChange} />;
}

*// Throttle example: scroll events*
function InfiniteScroll() {
  const handleScroll = useCallback(
    throttle(() => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        loadMore();
      }
    }, 200),
    []
  );

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [handleScroll]);
}
```

**Common pitfalls:**

- Creating new debounced functions every render (use `useMemo`/`useCallback`)
- Not canceling on unmount (memory leaks)
- Using wrong technique (debounce for search, throttle for scroll)
- Debounce delay too long (feels laggy) or too short (too many calls)

**Best practices:**

- Debounce: input fields, search, resize events (300-500ms)
- Throttle: scroll, mousemove, animations (100-200ms)
- Always cleanup in useEffect
- Consider `useTransition` as alternative for React 18+
- Metrics: Reduce API calls by 80-90%, keep input responsive <100ms

---

## 10. **Key Optimization in Lists**

**Explanation:**
Unique, stable keys help React identify which items changed, added, or removed, minimizing DOM operations.

**Real-world use case:**

```jsx
*// ❌ Bad - using index (breaks on reorder/filter)*
items.map((item, index) => <Item key={index} {...item} />)

*// ❌ Bad - non-unique keys*
items.map(item => <Item key={item.type} {...item} />)

*// ✅ Good - stable unique identifier*
items.map(item => <Item key={item.id} {...item} />)

*// ✅ Good - composite key when no single unique field*
items.map(item => <Item key={`${item.userId}-${item.postId}`} {...item} />)
```

**Common pitfalls:**

- Using array index as key (causes state bugs on reorder)
- Using `Math.random()` (forces full re-render every time)
- Non-unique keys (React warning, incorrect reconciliation)
- Changing key format (treats as new element)

**Best practices:**

- Use database IDs or UUIDs
- Generate stable IDs on data creation, not render
- For static lists only, index is acceptable
- Test with list mutations (add/remove/sort)
- Metrics: Reduces unnecessary DOM operations by 50-70% in dynamic lists

---

## 11. **Context Optimization**

**Explanation:**
React Context triggers re-renders in all consumers when value changes. Improper use causes performance issues in large component trees.

**Real-world use case:**

```jsx
*// ❌ Bad - entire object changes every render*
function BadProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  
  *// New object every render!*
  const value = { user, setUser, theme, setTheme };
  
  return <AppContext.Provider value={value}>{children}</AppContext.Provider>;
}

*// ✅ Good - split contexts by update frequency*
const UserContext = createContext();
const ThemeContext = createContext();

function OptimizedProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  
  const userValue = useMemo(() => ({ user, setUser }), [user]);
  const themeValue = useMemo(() => ({ theme, setTheme }), [theme]);
  
  return (
    <UserContext.Provider value={userValue}>
      <ThemeContext.Provider value={themeValue}>
        {children}
      </ThemeContext.Provider>
    </UserContext.Provider>
  );
}

*// Even better - separate state and setters*
const UserStateContext = createContext();
const UserDispatchContext = createContext();
```

**Common pitfalls:**

- Creating new value objects without `useMemo`
- Putting too much in one context
- Placing provider too high in tree
- Not splitting frequently-updated values from static ones

**Best practices:**

- Split contexts by concern and update frequency
- Memoize context values
- Use separate contexts for state and dispatch functions
- Consider state management libraries (Zustand, Jotai) for complex needs
- Metrics: Profiler shows unnecessary re-renders of context consumers

---

## 12. **React DevTools Profiler**

**Explanation:**
Built-in performance measurement tool that records component render times, reasons for re-renders, and interaction timing.

**Real-world use case:**

```jsx
*// Programmatic profiling in production*
import { Profiler } from 'react';

function onRenderCallback(
  id, *// "phase" of the tree that committed*
  phase, *// "mount" or "update"*
  actualDuration, *// time spent rendering*
  baseDuration, *// estimated time without memoization*
  startTime,
  commitTime,
  interactions *// Set of interactions for this update*
) {
  *// Send to analytics*
  if (actualDuration > 100) {
    logSlowRender({ id, phase, actualDuration });
  }
}

<Profiler id="Dashboard" onRender={onRenderCallback}>
  <Dashboard />
</Profiler>
```

**Common pitfalls:**

- Not enabling "Record why each component rendered"
- Profiling in development mode (React is slower)
- Not filtering out negligible renders (<5ms)
- Ignoring "Rendered at" timestamps

**Best practices:**

- Profile in production build with `?react_perf` URL param
- Focus on components >16ms (frame budget)
- Look for unnecessary renders (same props, no visual change)
- Use "Ranked" view to find slowest components
- Metrics: Target <16ms per component, <50ms total per interaction

---

## 13. **Web Vitals & Performance Metrics**

**Explanation:**
Google's Core Web Vitals measure real user experience. Critical for SEO and actual performance perception.

**Real-world use case:**

```jsx
import { onCLS, onFID, onLCP, onFCP, onTTFB } from 'web-vitals';

function sendToAnalytics({ name, value, id }) {
  *// Send to your analytics service*
  gtag('event', name, {
    value: Math.round(name === 'CLS' ? value * 1000 : value),
    event_category: 'Web Vitals',
    event_label: id,
    non_interaction: true,
  });
}

*// Measure all vitals*
onLCP(sendToAnalytics);
onFID(sendToAnalytics);
onCLS(sendToAnalytics);
onFCP(sendToAnalytics);
onTTFB(sendToAnalytics);
```

**Core metrics:**

- **LCP (Largest Contentful Paint)**: <2.5s (good), <4s (needs improvement), >4s (poor)
- **FID (First Input Delay)**: <100ms (good), <300ms (needs improvement)
- **CLS (Cumulative Layout Shift)**: <0.1 (good), <0.25 (needs improvement)
- **FCP (First Contentful Paint)**: <1.8s (good)
- **TTI (Time to Interactive)**: <3.8s (good)

**Common pitfalls:**

- Only testing on fast machines/networks
- Ignoring real user monitoring (RUM)
- Not testing mobile performance
- Focusing on lab tests only (Lighthouse) vs field data

**Best practices:**

- Use `web-vitals` library for accurate measurement
- Monitor 75th percentile of real users (not averages)
- Test on 3G networks and mid-tier devices
- Fix CLS by reserving space for images/ads
- Metrics dashboard: Track all Core Web Vitals weekly

---

## 14. **Server-Side Rendering (SSR) & Static Generation**

**Explanation:**
Pre-render React on server for faster initial load. Next.js provides `getServerSideProps` (SSR) and `getStaticProps` (SSG).

**Real-world use case:**

```jsx
*// Static Site Generation - best performance*
export async function getStaticProps() {
  const posts = await fetchPosts();
  return {
    props: { posts },
    revalidate: 3600, *// ISR: regenerate every hour*
  };
}

*// Server-Side Rendering - dynamic data*
export async function getServerSideProps(context) {
  const userId = context.params.id;
  const user = await fetchUser(userId);
  return {
    props: { user },
  };
}

*// React Server Components (RSC) - streaming HTML*
async function ServerComponent() {
  const data = await fetchData(); *// Server-only*
  return <ClientComponent data={data} />;
}
```

**Common pitfalls:**

- Using SSR for everything (slower than SSG)
- Not implementing proper hydration
- Sending too much data in initial HTML
- Forgetting to handle loading states during hydration
- Client-server mismatch errors

**Best practices:**

- Use SSG with ISR (Incremental Static Regeneration) when possible
- Stream HTML with Suspense boundaries (React 18)
- Minimize serialized props in HTML
- Separate server-only code from client code
- Metrics: TTFB <600ms, FCP <1.8s, hydration <2s

---

## 15. **Asset Preloading & Resource Hints**

**Explanation:**
Browser hints that optimize resource loading: preload (critical resources), prefetch (future navigation), preconnect (DNS/TCP).

**Real-world use case:**

```jsx
*// In Next.js Head or index.html*
<head>
  {*/* Preload critical resources */*}
  <link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossOrigin />
  <link rel="preload" href="/hero.jpg" as="image" />
  
  {*/* Prefetch next likely page */*}
  <link rel="prefetch" href="/dashboard.js" />
  
  {*/* Preconnect to external domains */*}
  <link rel="preconnect" href="https://api.example.com" />
  <link rel="dns-prefetch" href="https://analytics.google.com" />
  
  {*/* Preload API data with modulepreload */*}
  <link rel="modulepreload" href="/api/user-data.js" />
</head>

*// Programmatic prefetching in React*
function ProductLink({ href, productId }) {
  const prefetchProduct = () => {
    const link = document.createElement('link');
    link.rel = 'prefetch';
    link.href = `/products/${productId}.json`;
    document.head.appendChild(link);
  };

  return (
    <Link href={href} onMouseEnter={prefetchProduct}>
      View Product
    </Link>
  );
}
```

**Common pitfalls:**

- Preloading too many resources (wastes bandwidth)
- Wrong resource type (`as` attribute)
- Missing CORS attributes for fonts
- Preloading resources that aren't actually used

**Best practices:**

- Preload: critical fonts, above-fold images, initial JS chunks
- Prefetch: next page routes, likely user actions
- Preconnect: 3rd party APIs used on page load
- Limit to 3-4 preloaded resources
- Metrics: Reduces LCP by 200-500ms for critical resources

---

## 16. **State Management Performance**

**Explanation:**
Efficient state architecture prevents unnecessary renders. Choose the right tool: Context, Redux, Zustand, Jotai, etc.

**Real-world use case:**

```jsx
*// Zustand - minimal boilerplate, good performance*
import create from 'zustand';

const useStore = create((set) => ({
  todos: [],
  filter: 'all',
  addTodo: (todo) => set((state) => ({ todos: [...state.todos, todo] })),
  setFilter: (filter) => set({ filter }),
}));

*// Component only re-renders when its specific slice changes*
function TodoList() {
  const todos = useStore((state) => state.todos); *// Fine-grained subscription*
  return <div>{todos.map(todo => <TodoItem key={todo.id} {...todo} />)}</div>;
}

function FilterBar() {
  const setFilter = useStore((state) => state.setFilter); *// Doesn't re-render when todos change*
  return <button onClick={() => setFilter('active')}>Active</button>;
}

*// Jotai - atomic state*
import { atom, useAtom } from 'jotai';

const todosAtom = atom([]);
const filterAtom = atom('all');
const filteredTodosAtom = atom((get) => {
  const todos = get(todosAtom);
  const filter = get(filterAtom);
  return todos.filter(todo => filter === 'all' || todo.status === filter);
});
```

**Common pitfalls:**

- Using Redux for everything (overkill for small apps)
- Not normalizing state (nested updates cause issues)
- Putting derived state in store (should be computed)
- Not using selectors with memoization

**Best practices:**

- Use Context for rarely-changing data (theme, auth)
- Use Zustand/Jotai for frequent updates with fine-grained subscriptions
- Normalize relational data: `{ users: { 1: {...} }, posts: { 1: { userId: 1 } } }`
- Colocate state close to where it's used
- Metrics: Redux DevTools shows action → render time, target <16ms

---

## 17. **Web Workers for Heavy Computation**

**Explanation:**
Offload CPU-intensive tasks to background threads, keeping main thread responsive for UI.

**Real-world use case:**

```jsx
*// worker.js*
self.addEventListener('message', (e) => {
  const { data, operation } = e.data;
  
  let result;
  switch(operation) {
    case 'processLargeDataset':
      result = data.map(item => heavyCalculation(item));
      break;
    case 'parseCSV':
      result = parseCSV(data);
      break;
  }
  
  self.postMessage({ result });
});

*// Component using worker*
function DataProcessor() {
  const [result, setResult] = useState(null);
  const workerRef = useRef(null);

  useEffect(() => {
    workerRef.current = new Worker(new URL('./worker.js', import.meta.url));
    
    workerRef.current.onmessage = (e) => {
      setResult(e.data.result);
    };

    return () => workerRef.current?.terminate();
  }, []);

  const processData = (data) => {
    workerRef.current.postMessage({ data, operation: 'processLargeDataset' });
  };

  return <button onClick={() => processData(largeArray)}>Process</button>;
}

*// Comlink makes it easier*
import { wrap } from 'comlink';

const worker = new Worker(new URL('./worker.js', import.meta.url));
const api = wrap(worker);

const result = await api.processLargeDataset(data);
```

**Common pitfalls:**

- Using for small tasks (overhead not worth it)
- Not handling worker errors
- Transferring too much data (slow serialization)
- Forgetting browser support check

**Best practices:**

- Use for: image processing, data parsing, complex calculations (>50ms)
- Transfer ArrayBuffers when possible (zero-copy)
- Use Comlink for cleaner API
- Consider WASM for extreme performance needs
- Metrics: Long Tasks <50ms after offloading, TBT (Total Blocking Time) <200ms

---

## 18. **Request Batching & Deduplication**

**Explanation:**
Combine multiple API requests into one, or deduplicate identical simultaneous requests to reduce network overhead.

**Real-world use case:**

```jsx
*// Using React Query for automatic deduplication*
import { useQuery } from '@tanstack/react-query';

function UserProfile({ userId }) {
  const { data } = useQuery(['user', userId], () => fetchUser(userId));
  *// Multiple components requesting same userId only fetch once*
  return <div>{data.name}</div>;
}

*// Manual batching with DataLoader pattern*
import DataLoader from 'dataloader';

const userLoader = new DataLoader(async (userIds) => {
  const users = await batchFetchUsers(userIds); *// Single API call*
  return userIds.map(id => users.find(u => u.id === id));
});

*// GraphQL batching*
const batchLink = new BatchHttpLink({
  uri: '/graphql',
  batchMax: 10,
  batchInterval: 20, *// Wait 20ms for more requests*
});

*// Manual request queue*
class RequestQueue {
  constructor(batchSize = 10, delay = 50) {
    this.queue = [];
    this.batchSize = batchSize;
    this.delay = delay;
    this.timeout = null;
  }

  add(request) {
    return new Promise((resolve, reject) => {
      this.queue.push({ request, resolve, reject });
      
      if (this.queue.length >= this.batchSize) {
        this.flush();
      } else if (!this.timeout) {
        this.timeout = setTimeout(() => this.flush(), this.delay);
      }
    });
  }

  async flush() {
    clearTimeout(this.timeout);
    this.timeout = null;
    
    const batch = this.queue.splice(0, this.batchSize);
    const requests = batch.map(b => b.request);
    
    try {
      const results = await batchFetch(requests);
      batch.forEach((b, i) => b.resolve(results[i]));
    } catch (error) {
      batch.forEach(b => b.reject(error));
    }
  }
}
```

**Common pitfalls:**

- Not handling partial failures in batches
- Batch window too long (increases latency)
- Batching requests that should be prioritized differently
- No error boundaries for failed batch requests

**Best practices:**

- Use React Query/SWR for automatic request deduplication
- Batch GraphQL queries with `@defer` directive
- Set reasonable batch windows (20-50ms)
- Implement retry logic for failed batches
- Metrics: Reduces requests by 60-80%, faster overall load time

---

## 19. **Optimistic Updates**

**Explanation:**
Update UI immediately before server confirms, assuming success. Rollback on failure. Creates instant-feeling interfaces.

**Real-world use case:**

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query';

function TodoItem({ todo }) {
  const queryClient = useQueryClient();

  const mutation = useMutation(
    (updatedTodo) => updateTodoAPI(updatedTodo),
    {
      *// Optimistic update*
      onMutate: async (updatedTodo) => {
        *// Cancel outgoing refetches*
        await queryClient.cancelQueries(['todos']);

        *// Snapshot previous value*
        const previousTodos = queryClient.getQueryData(['todos']);

        *// Optimistically update*
        queryClient.setQueryData(['todos'], (old) =>
          old.map(t => t.id === updatedTodo.id ? updatedTodo : t)
        );

        return { previousTodos };
      },
      
      *// Rollback on error*
      onError: (err, updatedTodo, context) => {
        queryClient.setQueryData(['todos'], context.previousTodos);
        showErrorToast('Failed to update');
      },
      
      *// Refetch on success*
      onSettled: () => {
        queryClient.invalidateQueries(['todos']);
      },
    }
  );

  const handleToggle = () => {
    mutation.mutate({ ...todo, completed: !todo.completed });
  };

  return (
    <div>
      <input type="checkbox" checked={todo.completed} onChange={handleToggle} />
      {todo.title}
    </div>
  );
}
```

**Common pitfalls:**

- Not storing previous state for rollback
- Forgetting to cancel in-flight queries before optimistic update
- No visual indication when rollback occurs
- Complex nested updates without proper state snapshots
- Not handling concurrent mutations

**Best practices:**

- Always store previous state in `onMutate`
- Show subtle loading indicator during server confirmation
- Implement clear error states with retry options
- Use libraries like React Query that handle edge cases
- Test offline scenarios and race conditions
- Metrics: Perceived interaction time reduced to <50ms (instant feedback)

---

## 20. **Error Boundaries & Lazy Loading Fallbacks**

**Explanation:**
Error boundaries catch JavaScript errors in component tree, preventing full app crashes. Critical for production resilience.

**Real-world use case:**

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    *// Log to error reporting service*
    logErrorToService(error, errorInfo);
    
    *// Track performance impact*
    if (window.performance) {
      performance.mark('error-boundary-triggered');
    }
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div>
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}

*// Strategic placement*
function App() {
  return (
    <ErrorBoundary fallback={<AppError />}>
      <Header /> {*/* Critical - no error boundary */*}
      
      <ErrorBoundary fallback={<SidebarError />}>
        <Sidebar />
      </ErrorBoundary>
      
      <ErrorBoundary fallback={<ContentError />}>
        <Suspense fallback={<Spinner />}>
          <MainContent />
        </Suspense>
      </ErrorBoundary>
    </ErrorBoundary>
  );
}

*// React 19 will have useErrorBoundary hook*
function ModernErrorHandling() {
  const [error, resetError] = useErrorBoundary();
  
  if (error) {
    return <ErrorFallback error={error} onReset={resetError} />;
  }
  
  return <App />;
}
```

**Common pitfalls:**

- Single error boundary at root (entire app fails)
- Not providing recovery mechanisms
- Missing error logging/monitoring
- Error boundaries don't catch: event handlers, async code, SSR errors, errors in error boundary itself

**Best practices:**

- Multiple boundaries at feature boundaries
- Granular boundaries for independent features
- Always include retry/reset functionality
- Log errors with stack traces to monitoring service (Sentry, LogRocket)
- Combine with Suspense for loading states
- Metrics: Track error rates, affected users, mean time to recovery

---

## 21. **CSS-in-JS Performance**

**Explanation:**
Runtime CSS-in-JS libraries (styled-components, Emotion) have performance costs. Modern solutions use compile-time or atomic CSS.

**Real-world use case:**

```jsx
*// ❌ Slow - runtime CSS generation, dynamic styles*
import styled from 'styled-components';

const Button = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
  font-size: ${props => props.size}px;
  */* Generates new class on every render */*
`;

*// ✅ Better - Emotion with static extraction*
import { css } from '@emotion/react';

const buttonStyles = css`
  background: blue;
  font-size: 16px;
`;

*// ✅ Best - Zero-runtime CSS-in-JS (Vanilla Extract, Linaria)*
import { style } from '@vanilla-extract/css';

const button = style({
  background: 'blue',
  fontSize: 16,
}); *// Extracts to CSS file at build time// ✅ Atomic CSS (Tailwind)*
function Button({ primary }) {
  return (
    <button className={primary ? 'bg-blue-500' : 'bg-gray-500'}>
      Click me
    </button>
  );
}

*// ✅ CSS Modules*
import styles from './Button.module.css';
function Button() {
  return <button className={styles.button}>Click</button>;
}
```

**Performance comparison (bundle impact):**
```
styled-components: ~16KB + runtime serialization
Emotion: ~11KB + runtime
Vanilla Extract: ~1KB (no runtime)
Tailwind: 0KB runtime, ~10KB base CSS (purgeable)
CSS Modules: 0KB runtime
```

**Common pitfalls:**

- Creating styled components inside render (massive perf hit)
- Excessive prop interpolation causing style recalculation
- Large theme objects passed through context
- Not using static CSS extraction when available

**Best practices:**

- Define styled components outside render functions
- Use compile-time CSS-in-JS (Vanilla Extract, Linaria)
- Or use atomic CSS (Tailwind) for best performance
- Avoid complex calculations in interpolations
- Use CSS variables for themeable values
- Metrics: Reduces style recalculation time by 70%, smaller bundle

---

## 22. **Intersection Observer for Lazy Loading**

**Explanation:**
API to efficiently detect when elements enter viewport. Essential for lazy loading images, infinite scroll, analytics.

**Real-world use case:**

```jsx
function LazyImage({ src, alt, placeholder }) {
  const [imageSrc, setImageSrc] = useState(placeholder);
  const [isLoaded, setIsLoaded] = useState(false);
  const imgRef = useRef();

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            setImageSrc(src);
            observer.disconnect();
          }
        });
      },
      {
        rootMargin: '50px', *// Start loading 50px before visible*
        threshold: 0.01,
      }
    );

    if (imgRef.current) {
      observer.observe(imgRef.current);
    }

    return () => observer.disconnect();
  }, [src]);

  return (
    <img
      ref={imgRef}
      src={imageSrc}
      alt={alt}
      onLoad={() => setIsLoaded(true)}
      style={{ opacity: isLoaded ? 1 : 0, transition: 'opacity 0.3s' }}
    />
  );
}

*// Infinite scroll*
function InfiniteList({ items, loadMore, hasMore }) {
  const loaderRef = useRef();

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting && hasMore) {
          loadMore();
        }
      },
      { threshold: 1.0 }
    );

    if (loaderRef.current) {
      observer.observe(loaderRef.current);
    }

    return () => observer.disconnect();
  }, [loadMore, hasMore]);

  return (
    <div>
      {items.map((item) => <ItemCard key={item.id} {...item} />)}
      {hasMore && <div ref={loaderRef}>Loading...</div>}
    </div>
  );
}

*// Analytics tracking*
function AnalyticsTracker({ children, eventName }) {
  const ref = useRef();
  const [hasTracked, setHasTracked] = useState(false);

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting && !hasTracked) {
          trackEvent(eventName, { visible: true });
          setHasTracked(true);
        }
      },
      { threshold: 0.5 } *// 50% visible*
    );

    if (ref.current) observer.observe(ref.current);
    return () => observer.disconnect();
  }, [eventName, hasTracked]);

  return <div ref={ref}>{children}</div>;
}
```

**Common pitfalls:**

- Creating new observer for each element (should reuse)
- Not disconnecting observers (memory leak)
- Wrong threshold values causing premature/late loading
- Missing rootMargin for smooth UX

**Best practices:**

- Reuse single observer instance for similar elements
- Use rootMargin for preloading (100-200px)
- Disconnect observers after use
- Combine with `loading="lazy"` for images as fallback
- Metrics: Reduces initial page load by 40-60%, improves LCP

---

## 23. **Service Workers & Offline Performance**

**Explanation:**
Background scripts that intercept network requests, enabling offline functionality and caching strategies.

**Real-world use case:**

```jsx
*// Register service worker*
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker
      .register('/sw.js')
      .then(reg => console.log('SW registered'))
      .catch(err => console.log('SW registration failed'));
  });
}

*// sw.js - Service Worker*
const CACHE_NAME = 'app-v1';
const urlsToCache = [
  '/',
  '/static/css/main.css',
  '/static/js/main.js',
  '/offline.html'
];

*// Install - cache critical assets*
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => cache.addAll(urlsToCache))
  );
});

*// Fetch - Network first, fallback to cache*
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then(response => {
        *// Clone response for cache*
        const responseClone = response.clone();
        caches.open(CACHE_NAME)
          .then(cache => cache.put(event.request, responseClone));
        return response;
      })
      .catch(() => {
        *// Network failed, try cache*
        return caches.match(event.request)
          .then(response => response || caches.match('/offline.html'));
      })
  );
});

*// Cache strategies with Workbox*
import { registerRoute } from 'workbox-routing';
import { CacheFirst, NetworkFirst, StaleWhileRevalidate } from 'workbox-strategies';

*// Images - Cache First*
registerRoute(
  ({ request }) => request.destination === 'image',
  new CacheFirst({ cacheName: 'images' })
);

*// API - Network First*
registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new NetworkFirst({ cacheName: 'api' })
);

*// Static assets - Stale While Revalidate*
registerRoute(
  ({ request }) => request.destination === 'script' || request.destination === 'style',
  new StaleWhileRevalidate({ cacheName: 'static' })
);
```

**Caching strategies:**

- **Cache First**: Images, fonts (rarely change)
- **Network First**: API calls, user data (fresh data priority)
- **Stale While Revalidate**: Static assets (instant load, update background)
- **Network Only**: POST requests, analytics
- **Cache Only**: App shell (critical offline assets)

**Common pitfalls:**

- Overly aggressive caching (serving stale content)
- Not versioning cache names (old content persists)
- Caching too much (storage limits)
- Forgetting to unregister old service workers

**Best practices:**

- Use Workbox for production (handles edge cases)
- Implement cache versioning and cleanup
- Show offline indicator in UI
- Precache app shell and critical routes
- Test offline scenarios thoroughly
- Metrics: Repeat visits load 10x faster, works offline

---

## 24. **requestIdleCallback & Time Slicing**

**Explanation:**
Schedule low-priority work during browser idle time, keeping UI responsive. React's Concurrent Mode uses this internally.

**Real-world use case:**

```jsx
*// Low-priority analytics processing*
function useIdleCallback(callback, options) {
  useEffect(() => {
    if ('requestIdleCallback' in window) {
      const handle = requestIdleCallback(callback, options);
      return () => cancelIdleCallback(handle);
    } else {
      *// Fallback for Safari*
      const timeout = setTimeout(callback, 1);
      return () => clearTimeout(timeout);
    }
  }, [callback, options]);
}

*// Process logs during idle time*
function AnalyticsProcessor() {
  const [logs, setLogs] = useState([]);

  useIdleCallback(() => {
    if (logs.length > 0) {
      const batch = logs.splice(0, 50);
      processAndSendLogs(batch);
      setLogs([...logs]);
    }
  }, { timeout: 2000 }); *// Max 2s delay*

  return null; *// Background component*
}

*// Time slicing - break heavy work into chunks*
function processLargeDataset(data, callback) {
  let index = 0;
  const chunkSize = 100;

  function processChunk(deadline) {
    while (
      index < data.length &&
      (deadline.timeRemaining() > 0 || deadline.didTimeout)
    ) {
      *// Process items*
      for (let i = 0; i < chunkSize && index < data.length; i++, index++) {
        processItem(data[index]);
      }
    }

    if (index < data.length) {
      requestIdleCallback(processChunk);
    } else {
      callback();
    }
  }

  requestIdleCallback(processChunk);
}

*// React concurrent features (built on similar concepts)*
function SearchResults({ query }) {
  const deferredQuery = useDeferredValue(query);
  
  *// Expensive operation deferred to idle time*
  const results = useMemo(
    () => searchData(deferredQuery),
    [deferredQuery]
  );

  return <ResultsList results={results} />;
}
```

**Common pitfalls:**

- Using for critical tasks (may be delayed indefinitely)
- Not providing timeout option
- Blocking main thread with long tasks even in idle callback
- Safari doesn't support (need polyfill)

**Best practices:**

- Use for: analytics, prefetching, background sync, non-critical logging
- Always set timeout for important-but-not-urgent work
- Keep chunks small (<50ms)
- Polyfill for Safari: `setTimeout` fallback
- Prefer React's `useTransition`/`useDeferredValue` for UI updates
- Metrics: Main thread idle time >50%, Long Tasks eliminated

---

## 25. **Hydration Optimization (SSR/SSG)**

**Explanation:**
Process of attaching React event listeners to server-rendered HTML. Poor hydration causes delays and mismatches.

**Real-world use case:**

```jsx
*// Progressive/Selective Hydration (React 18)*
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App({ serverData }) {
  return (
    <div>
      {*/* Static content - no hydration needed */*}
      <header>Static Header</header>

      {*/* Critical - hydrates immediately */*}
      <SearchBar />

      {*/* Non-critical - hydrates when visible */*}
      <Suspense fallback={<div>Loading...</div>}>
        <HeavyComponent data={serverData} />
      </Suspense>

      {*/* Interactive island - hydrates on interaction */*}
      <ClientOnly>
        <InteractiveWidget />
      </ClientOnly>
    </div>
  );
}

*// Client-only rendering (skip hydration)*
function ClientOnly({ children }) {
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  return mounted ? children : null;
}

*// Avoid hydration mismatches*
function SafeDate() {
  const [date, setDate] = useState(null);

  useEffect(() => {
    setDate(new Date().toLocaleDateString());
  }, []);

  *// Don't render until client-side*
  if (!date) return <div suppressHydrationWarning>{*/* placeholder */*}</div>;

  return <div>{date}</div>;
}

*// Next.js optimizations*
export default function Page({ data }) {
  return (
    <>
      {*/* Prevents this from hydrating */*}
      <NoSSR>
        <ClientOnlyMap />
      </NoSSR>

      {*/* Normal hydration */*}
      <Content data={data} />
    </>
  );
}
```

**Hydration issues:**

- **Mismatch warnings**: Client HTML differs from server HTML
- **Slow hydration**: Large components block interactivity
- **Flash of unstyled content**: CSS not loaded before hydration

**Common pitfalls:**

- Using browser-only APIs during SSR (`window`, `localStorage`)
- Date/time rendering causing mismatches
- Random values or IDs generated differently server/client
- Not splitting large components for progressive hydration

**Best practices:**

- Use `useEffect` for browser-only code
- Suppress warnings only when necessary: `suppressHydrationWarning`
- Progressive hydration with React 18 Suspense
- Streaming SSR to reduce TTFB
- Lazy load non-critical components
- Metrics: TTI <3.8s, hydration time <500ms for critical UI

---

## 26. **Font Loading Optimization**

**Explanation:**
Web fonts block rendering (FOIT) or cause layout shift (FOUT). Proper loading improves perceived performance.

**Real-world use case:**

```jsx
*// Optimal font loading strategy*
<head>
  {*/* Preload critical fonts */*}
  <link
    rel="preload"
    href="/fonts/main-regular.woff2"
    as="font"
    type="font/woff2"
    crossOrigin="anonymous"
  />

  {*/* Use font-display for fallback strategy */*}
  <style>
    @font-face {
      font-family: 'MainFont';
      src: url('/fonts/main-regular.woff2') format('woff2');
      font-weight: 400;
      font-style: normal;
      font-display: swap; */* Show fallback immediately */*
    }
    
    @font-face {
      font-family: 'MainFont';
      src: url('/fonts/main-bold.woff2') format('woff2');
      font-weight: 700;
      font-style: normal;
      font-display: optional; */* Only use if cached */*
    }
  </style>
</head>

*// Font loading with JavaScript*
function useFontLoading() {
  const [fontsLoaded, setFontsLoaded] = useState(false);

  useEffect(() => {
    if ('fonts' in document) {
      Promise.all([
        document.fonts.load('400 1em MainFont'),
        document.fonts.load('700 1em MainFont')
      ]).then(() => {
        setFontsLoaded(true);
        document.documentElement.classList.add('fonts-loaded');
      });
    }
  }, []);

  return fontsLoaded;
}

*// Adjust UI based on font loading*
function App() {
  const fontsLoaded = useFontLoading();

  return (
    <div className={fontsLoaded ? 'fonts-ready' : 'fonts-loading'}>
      <Content />
    </div>
  );
}

*// CSS to minimize layout shift*
.fonts-loading {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  */* Size adjustments to match custom font */*
  font-size: 16px;
  letter-spacing: 0.01em;
}

.fonts-ready {
  font-family: 'MainFont', sans-serif;
  font-size: 16px;
}
```

**font-display strategies:**

- **swap**: Show fallback immediately, swap when loaded (best for most cases)
- **optional**: Use custom font only if cached (avoid layout shift)
- **fallback**: 100ms block, 3s swap period (compromise)
- **block**: Wait for font up to 3s (avoid unless critical)

**Common pitfalls:**

- Loading too many font weights/styles
- Not preloading critical fonts
- Using `font-display: block` (delays rendering)
- Large font files (not subsetting)
- Missing fallback font metrics adjustment

**Best practices:**

- Preload 1-2 critical fonts only
- Use `font-display: swap` for body text
- Use system fonts for UI elements
- Subset fonts to needed characters (save 70-80%)
- Use variable fonts (single file, multiple weights)
- Host fonts locally (avoid Google Fonts latency)
- Metrics: Eliminate FOIT, reduce CLS <0.05, fonts load <1s

---

## 27. **Memory Leak Prevention**

**Explanation:**
Memory leaks occur when unused objects aren't garbage collected, degrading performance over time. Critical for SPAs.

**Real-world use case:**

```jsx
*// ❌ Common memory leaks// 1. Event listeners not cleaned up*
function BadComponent() {
  useEffect(() => {
    window.addEventListener('scroll', handleScroll);
    *// Missing cleanup!*
  }, []);
}

*// 2. Timers not cleared*
function BadTimer() {
  useEffect(() => {
    setInterval(() => {
      console.log('tick');
    }, 1000);
    *// Missing clearInterval!*
  }, []);
}

*// 3. Subscriptions not unsubscribed*
function BadSubscription() {
  useEffect(() => {
    const subscription = eventEmitter.subscribe(handleEvent);
    *// Missing unsubscribe!*
  }, []);
}

*// 4. Async operations updating unmounted components*
function BadAsync() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetchData().then(result => {
      setData(result); *// Component might be unmounted!*
    });
  }, []);
}

*// ✅ Proper cleanup patterns*

function GoodComponent() {
  const [data, setData] = useState(null);
  const [isScrolling, setIsScrolling] = useState(false);

  useEffect(() => {
    *// 1. Event listener cleanup*
    const handleScroll = () => setIsScrolling(true);
    window.addEventListener('scroll', handleScroll);
    
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  useEffect(() => {
    *// 2. Timer cleanup*
    const timer = setInterval(() => {
      console.log('tick');
    }, 1000);
    
    return () => clearInterval(timer);
  }, []);

  useEffect(() => {
    *// 3. Subscription cleanup*
    const subscription = eventEmitter.subscribe(handleEvent);
    
    return () => subscription.unsubscribe();
  }, []);

  useEffect(() => {
    *// 4. Abort async operations*
    let isMounted = true;
    const controller = new AbortController();

    fetchData({ signal: controller.signal })
      .then(result => {
        if (isMounted) {
          setData(result);
        }
      })
      .catch(err => {
        if (err.name !== 'AbortError') {
          console.error(err);
        }
      });

    return () => {
      isMounted = false;
      controller.abort();
    };
  }, []);

  return <div>{data}</div>;
}

*// Custom hook for safe async state*
function useSafeState(initialState) {
  const [state, setState] = useState(initialState);
  const mountedRef = useRef(true);

  useEffect(() => {
    return () => {
      mountedRef.current = false;
    };
  }, []);

  const setSafeState = useCallback((value) => {
    if (mountedRef.current) {
      setState(value);
    }
  }, []);

  return [state, setSafeState];
}
```

**Detecting memory leaks:**

```jsx
*// Chrome DevTools Memory Profiler// 1. Take heap snapshot// 2. Interact with component (mount/unmount)// 3. Force garbage collection// 4. Take another snapshot// 5. Compare - look for detached DOM nodes// Performance monitoring*
if (performance.memory) {
  setInterval(() => {
    console.log('Used JS Heap:', performance.memory.usedJSHeapSize / 1048576);
    console.log('Total JS Heap:', performance.memory.totalJSHeapSize / 1048576);
  }, 5000);
}
```

**Common pitfalls:**

- Forgetting cleanup in `useEffect` return function
- Closures holding references to large objects
- Event emitters with accumulating listeners
- Circular references preventing garbage collection
- Large objects in component state not cleared

**Best practices:**

- Always return cleanup function from `useEffect`
- Use `AbortController` for fetch requests
- Clear timers and intervals
- Unsubscribe from event emitters
- Profile memory usage in Chrome DevTools
- Test component mount/unmount cycles
- Metrics: Heap size stable over time, no "Detached" DOM nodes

---

## 28. **Third-Party Script Optimization**

**Explanation:**
External scripts (analytics, ads, chat widgets) often block rendering or consume resources. Load strategically.

**Real-world use case:**

```jsx
*// Lazy load third-party scripts*
function useScript(src, options = {}) {
  const [status, setStatus] = useState('idle');

  useEffect(() => {
    if (!src) return;

    let script = document.querySelector(`script[src="${src}"]`);

    if (!script) {
      script = document.createElement('script');
      script.src = src;
      script.async = options.async !== false;
      script.defer = options.defer || false;
      
      script.setAttribute('data-status', 'loading');
      document.body.appendChild(script);

      const setAttributeFromEvent = (event) => {
        script.setAttribute('data-status', event.type === 'load' ? 'ready' : 'error');
      };

      script.addEventListener('load', setAttributeFromEvent);
      script.addEventListener('error', setAttributeFromEvent);
    } else {
      setStatus(script.getAttribute('data-status'));
    }

    const setStateFromEvent = (event) => {
      setStatus(event.type === 'load' ? 'ready' : 'error');
    };

    script.addEventListener('load', setStateFromEvent);
    script.addEventListener('error', setStateFromEvent);

    return () => {
      if (script) {
        script.removeEventListener('load', setStateFromEvent);
        script.removeEventListener('error', setStateFromEvent);
      }
    };
  }, [src, options.async, options.defer]);

  return status;
}

*// Load analytics after interaction*
function DeferredAnalytics() {
  const [shouldLoad, setShouldLoad] = useState(false);

  useEffect(() => {
    *// Load after user interaction*
    const loadAnalytics = () => {
      setShouldLoad(true);
      cleanup();
    };

    const events = ['mousedown', 'touchstart', 'scroll', 'keydown'];
    events.forEach(event => window.addEventListener(event, loadAnalytics, { once: true }));

    *// Or load after delay*
    const timer = setTimeout(loadAnalytics, 3000);

    const cleanup = () => {
      clearTimeout(timer);
      events.forEach(event => window.removeEventListener(event, loadAnalytics));
    };

    return cleanup;
  }, []);

  useScript(shouldLoad ? 'https://analytics.com/script.js' : null);

  return null;
}

*// Next.js Script component*
import Script from 'next/script';

function App() {
  return (
    <>
      {*/* Load after page interactive */*}
      <Script
        src="https://analytics.com/script.js"
        strategy="lazyOnload"
        onLoad={() => console.log('Analytics loaded')}
      />

      {*/* Critical - load in <head> */*}
      <Script
        src="https://critical.com/script.js"
        strategy="beforeInteractive"
      />

      {*/* Load after hydration */*}
      <Script
        src="https://widget.com/script.js"
        strategy="afterInteractive"
      />
    </>
  );
}

*// Facade pattern for heavy embeds*
function YouTubeEmbed({ videoId }) {
  const [showReal, setShowReal] = useState(false);

  if (!showReal) {
    return (
      <div
        onClick={() => setShowReal(true)}
        style={{
          backgroundImage: `url(https://img.youtube.com/vi/${videoId}/maxresdefault.jpg)`,
          cursor: 'pointer'
        }}
      >
        <PlayButton />
      </div>
    );
  }

  return (
    <iframe
      src={`https://www.youtube.com/embed/${videoId}?autoplay=1`}
      allow="autoplay"
    />
  );
}
```

**Common pitfalls:**
- Loading all scripts on initial page load
- Not deferring non-critical scripts
- Synchronous script tags blocking render
- Missing error handling for failed scripts
- Loading same script multiple times

**Best practices:**
- Load analytics after user interaction or 3s delay
- Use facade pattern for embeds (YouTube, Maps)
- Self-host when possible (avoid third-party latency)
- Use `async` or `defer` for all scripts
- Monitor third-party impact with Request Blocking in DevTools
- Consider web workers for heavy scripts
- Metrics: Reduce blocking time by 1-2s, improve TTI significantly

---

## Summary: Performance Budget & Monitoring

**Establish performance budgets:**
```
JavaScript Bundle: <200KB gzipped
CSS: <50KB gzipped
Images: <500KB total per page
Fonts: <100KB total
Third-party scripts: <100KB

Core Web Vitals:
- LCP: <2.5s
- FID: <100ms
- CLS: <0.1

Custom metrics:
- Time to Interactive: <3.8s
- First Contentful Paint: <1.8s
- Total Blocking Time: <200ms
```

**Continuous monitoring:**

- Real User Monitoring (RUM): Datadog, New Relic
- Synthetic monitoring: Lighthouse CI in pipeline
- Performance budgets in webpack: `performance.maxAssetSize`
- Alerts on metric regression
- Weekly performance reviews

The key to being a senior developer is knowing **when** to apply these optimizations. Always profile first, optimize second. Not every app needs every technique - choose based on actual bottlenecks and user impact.
