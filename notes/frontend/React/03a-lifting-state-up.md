# Lifting State Up

## The Problem

When two sibling components need to share the same data, neither can own the state independently. One component updates the data, but the other doesn't see the change.

```jsx
// ❌ Broken: each component owns its own state
function SearchInput() {
    const [query, setQuery] = useState('');
    return (
        <input 
            value={query} 
            onChange={e => setQuery(e.target.value)} 
            placeholder="Search..."
        />
    );
}

function FilteredList() {
    const [query, setQuery] = useState(''); // independent state!
    const items = ['Apple', 'Banana', 'Cherry', 'Date'];
    const filtered = items.filter(item => 
        item.toLowerCase().includes(query.toLowerCase())
    );
    return (
        <ul>
            {filtered.map(item => <li key={item}>{item}</li>)}
        </ul>
    );
}

function App() {
    return (
        <div>
            <SearchInput />
            <FilteredList />
        </div>
    );
}
// Problem: typing in SearchInput doesn't filter FilteredList
// because each has its own `query` state
```

## The Solution

Move the shared state up to the **closest common parent**. The parent owns the state and passes it down via props.

```jsx
// ✅ Correct: parent owns the state
function App() {
    const [query, setQuery] = useState('');

    return (
        <div>
            <SearchInput query={query} onChange={setQuery} />
            <FilteredList query={query} />
        </div>
    );
}

function SearchInput({ query, onChange }) {
    return (
        <input 
            value={query} 
            onChange={e => onChange(e.target.value)} 
            placeholder="Search..."
        />
    );
}

function FilteredList({ query }) {
    const items = ['Apple', 'Banana', 'Cherry', 'Date'];
    const filtered = items.filter(item => 
        item.toLowerCase().includes(query.toLowerCase())
    );
    return (
        <ul>
            {filtered.map(item => <li key={item}>{item}</li>)}
        </ul>
    );
}
```

> The state lives in `App`. Both children receive it via props. When you type in the input, the parent updates and the list filters automatically.

## How It Works

```
App (owns state: query="")
 ├─ SearchInput (receives query + onChange via props)
 └─ FilteredList (receives query via props)
```

1. State is declared in the common parent
2. Downward flow: parent passes state as props to children
3. Upward flow: children call the parent's setter to update
4. Parent re-renders → both children re-render with new data

## Rules

- **State lives where it's shared.** If two components need the same value, lift it to their closest common ancestor.
- **Single source of truth.** One component owns the state; others just read it.
- **Keep state minimal.** Only store what you need. Derive the rest (e.g., filtered list from query + items).

## Lifting State vs Context

| Pattern | When to use |
|---|---|
| **Lifting State Up** | 2-3 sibling components share state through a common parent |
| **Context API** | Data needed by many components across different branches of the tree |

Lifting State Up is usually sufficient. Context is for when prop passing becomes unwieldy (3+ levels deep).
