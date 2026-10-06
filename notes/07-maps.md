# 07. Maps

Unordered key-value hash map collections in Go.

---

## 1. Map Initialization & Basic Operations

Keys must be of a **comparable type** (types supporting `==`, like `int`, `string`, structs without slices/maps).

```go
// 1. make(map[KeyType]ValueType, initialCapacityHint)
scores := make(map[string]int)

// 2. Map literal
capitals := map[string]string{
    "India":  "New Delhi",
    "France": "Paris",
}

// Insert / Update
scores["Alice"] = 95

// Delete key
delete(scores, "Alice") // Safe even if key doesn't exist
```

> **Warning**: A `nil` map (`var m map[string]int`) can be read from (returns zero value), but **writing to a nil map triggers a runtime panic**. Always use `make()` or a literal before writing.

---

## 2. The Comma-OK Idiom

Reading a non-existent key returns the value type's **zero value**. To distinguish between a stored zero value and a missing key, use the 2-value lookup:

```go
val, ok := scores["Bob"]
if !ok {
    fmt.Println("Key does not exist!")
} else {
    fmt.Println("Score:", val)
}
```

---

## 3. Map Iteration

- Iteration order over maps is **randomized** by Go's runtime by design to prevent reliance on hash order.

```go
for key, value := range capitals {
    fmt.Printf("%s -> %s\n", key, value)
}
```

If sorted order is needed, extract the keys into a slice, sort the slice, and iterate over keys.

---

## 4. Idiomatic Map Patterns

### Pattern 1: Set Implementation
Go has no built-in `Set`. Use `map[T]struct{}` (`struct{}` uses 0 bytes of memory):
```go
seen := make(map[string]struct{})

// Add to set
seen["apple"] = struct{}{}

// Check membership
if _, exists := seen["apple"]; exists {
    fmt.Println("apple is present")
}
```

### Pattern 2: Frequency Counter
```go
counts := make(map[rune]int)
for _, r := range "banana" {
    counts[r]++ // Defaults to 0 on first occurrence, increments cleanly
}
```

### Pattern 3: Grouping / Bucketing
```go
grouped := make(map[string][]string)
grouped["fruits"] = append(grouped["fruits"], "apple", "mango")
```

---

## 5. Performance Tips

- **Preallocate**: If the expected number of items is known, provide a capacity hint `make(map[K]V, count)` to minimize internal rehashing and memory reallocations.
- **Concurrency**: Go maps are **not safe for concurrent writes**. Use `sync.RWMutex` or `sync.Map` for concurrent access.
