# Optimization Techniques Catalog

**Document ID:** `perf-optimizer/docs/optimization-techniques`  
**Version:** 1.0.0  
**Purpose:** Complete reference of optimization techniques for web applications

---

## 1. JavaScript/TypeScript

### 1.1 Avoid Creating Objects in Loops

```typescript
// BAD: New object each iteration
for (let i = 0; i < 1000; i++) {
  processItem({ id: i, data: items[i] }); // 1000 allocations
}

// GOOD: Reuse object
const temp = { id: 0, data: null };
for (let i = 0; i < 1000; i++) {
  temp.id = i;
  temp.data = items[i];
  processItem(temp); // 0 allocations in loop
}
```

### 1.2 Object Pooling Pattern

```typescript
class ObjectPool<T> {
  private pool: T[] = [];
  private factory: () => T;

  constructor(factory: () => T, initialSize = 10) {
    this.factory = factory;
    for (let i = 0; i < initialSize; i++) {
      this.pool.push(factory());
    }
  }

  acquire(): T {
    return this.pool.pop() ?? this.factory();
  }

  release(obj: T): void {
    this.pool.push(obj);
  }
}

// Usage
const vectorPool = new ObjectPool(() => ({ x: 0, y: 0 }));
const v = vectorPool.acquire();
// ... use v ...
vectorPool.release(v);
```

### 1.3 Manual Memoization

```typescript
const cache = new Map<string, Result>();

function expensiveComputation(input: string): Result {
  if (cache.has(input)) {
    return cache.get(input)!;
  }
  
  const result = /* heavy calculation */;
  cache.set(input, result);
  return result;
}
```

---

## 2. React Specific

### 2.1 React.memo with Custom Comparator

```typescript
interface Props {
  data: ComplexObject;
  onUpdate: (id: string) => void;
}

export const OptimizedComponent = React.memo(
  ({ data, onUpdate }: Props) => {
    return <div>{/* render */}</div>;
  },
  (prevProps, nextProps) => {
    // Return true if should NOT re-render
    return prevProps.data.id === nextProps.data.id &&
           prevProps.data.version === nextProps.data.version;
  }
);
```

### 2.2 useMemo for Heavy Calculations

```typescript
function DataGrid({ items, filter }: Props) {
  // Recalculates ONLY when items or filter change
  const filteredItems = useMemo(() => {
    return items.filter(item => matchesFilter(item, filter));
  }, [items, filter]);

  return <Grid items={filteredItems} />;
}
```

### 2.3 useCallback for Stable Handlers

```typescript
function Parent() {
  const [count, setCount] = useState(0);
  
  // GOOD: Stable function
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []); // Empty deps = never recreates

  return <ExpensiveChild onClick={handleClick} />;
}
```

### 2.4 Context Splitting

```typescript
// BAD: One giant context
const AppContext = createContext({ user, settings, theme, data });

// GOOD: Granular contexts
const UserContext = createContext(user);
const ThemeContext = createContext(theme);
const SettingsContext = createContext(settings);

// Component only re-renders when ITS context changes
function ThemeButton() {
  const theme = useContext(ThemeContext);
  return <button style={{ background: theme.primary }}>Click</button>;
}
```

---

## 3. Memory Management

### 3.1 Cleanup in useEffect

```typescript
useEffect(() => {
  const controller = new AbortController();
  
  fetchData({ signal: controller.signal });
  
  const timer = setInterval(update, 1000);
  const handler = (e) => handleResize(e);
  window.addEventListener('resize', handler);

  return () => {
    controller.abort();           // Cancel fetch
    clearInterval(timer);         // Clear timer
    window.removeEventListener('resize', handler); // Remove listener
  };
}, []);
```

### 3.2 WeakRef for Cache

```typescript
class WeakCache<K extends object, V> {
  private cache = new WeakMap<K, V>();

  get(key: K): V | undefined {
    return this.cache.get(key);
  }

  set(key: K, value: V): void {
    this.cache.set(key, value);
    // Automatically cleaned when key is GC'd
  }
}
```

---

## 4. Network Optimization

### 4.1 Request Coalescing

```typescript
class RequestCoalescer {
  private pending = new Map<string, Promise<any>>();

  async fetch(url: string): Promise<any> {
    if (this.pending.has(url)) {
      return this.pending.get(url);
    }

    const promise = fetch(url)
      .then(r => r.json())
      .finally(() => this.pending.delete(url));

    this.pending.set(url, promise);
    return promise;
  }
}
```

### 4.2 Stale-While-Revalidate

```typescript
async function fetchWithSWR(url: string): Promise<Data> {
  const cached = cache.get(url);
  
  if (cached) {
    // Revalidate in background
    fetch(url).then(r => r.json()).then(data => cache.set(url, data));
    return cached;
  }
  
  const data = await fetch(url).then(r => r.json());
  cache.set(url, data);
  return data;
}
```

---

## 5. Bundle Optimization

### 5.1 Tree Shaking Friendly Imports

```typescript
// BAD: Imports everything
import _ from 'lodash';
_.debounce(fn, 300);

// GOOD: Imports only what's needed
import debounce from 'lodash-es/debounce';
debounce(fn, 300);
```

### 5.2 Heavy Dependency Replacements

| Heavy | Light | Savings |
|-------|-------|---------|
| moment.js (300kb) | date-fns (30kb) | ~270kb |
| lodash (70kb) | lodash-es + tree shake | ~60kb |
| axios (15kb) | native fetch | ~15kb |
| uuid (9kb) | crypto.randomUUID() | ~9kb |

---

## 6. Rendering Performance

### 6.1 Virtual Scrolling

```typescript
function VirtualList({ items, itemHeight, containerHeight }) {
  const [scrollTop, setScrollTop] = useState(0);
  
  const startIndex = Math.floor(scrollTop / itemHeight);
  const endIndex = Math.min(
    startIndex + Math.ceil(containerHeight / itemHeight) + 1,
    items.length
  );
  
  const visibleItems = items.slice(startIndex, endIndex);
  
  return (
    <div 
      style={{ height: containerHeight, overflow: 'auto' }}
      onScroll={e => setScrollTop(e.target.scrollTop)}
    >
      <div style={{ height: items.length * itemHeight }}>
        <div style={{ transform: `translateY(${startIndex * itemHeight}px)` }}>
          {visibleItems.map(item => (
            <div key={item.id} style={{ height: itemHeight }}>
              {item.content}
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

---

## 7. CSS Performance

### 7.1 Avoid Layout Thrashing

```typescript
// BAD: Reads and writes alternately
elements.forEach(el => {
  const width = el.offsetWidth; // Forces layout
  el.style.width = width * 2 + 'px'; // Invalidates layout
});

// GOOD: Batch reads, then batch writes
const widths = elements.map(el => el.offsetWidth);
elements.forEach((el, i) => {
  el.style.width = widths[i] * 2 + 'px';
});
```

### 7.2 CSS Containment

```css
/* Isolates layout recalculations */
.card {
  contain: layout style paint;
}

/* For fixed-size elements */
.fixed-box {
  contain: strict;
  width: 200px;
  height: 200px;
}
```

---

> **Caution:** Never optimize without measuring first! Use Chrome DevTools Performance tab to identify the real bottleneck before applying any technique.
