# 03. Control Flow

Conditionals, loops, pattern matching, `defer`, and panic/recover mechanisms.

---

## 1. `if` / `else` Statements

Go `if` statements do not use parentheses around conditions.

### With Initialization Statement:
Declare a temporary variable scoped strictly to the `if` / `else` block:
```go
if val, err := calculate(); err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println("Result:", val)
}
// 'val' and 'err' are NOT accessible here
```

---

## 2. `for` Loops (The Only Loop Construct in Go)

Go has **no `while` or `do-while` loops**. `for` does everything.

```go
// 1. Standard 3-component loop
for i := 0; i < 5; i++ {
    fmt.Print(i, " ")
}

// 2. While-style loop (condition only)
count := 10
for count > 0 {
    count -= 2
}

// 3. Infinite loop
for {
    // break when done
    break
}
```

### `range` Loop
Used to iterate over collections:
```go
// Slice / Array: index and value (copy)
for idx, val := range []int{10, 20, 30} {
    fmt.Printf("[%d]=%d ", idx, val)
}

// Map: key and value
for k, v := range userMap {
    fmt.Printf("%s: %s\n", k, v)
}

// String: byte index and rune (Unicode character)
for i, ch := range "Go!" {
    fmt.Printf("%d:%c ", i, ch)
}

// Ignore index or value using blank identifier _
for _, val := range items { ... }
```

> **Warning**: The `val` in `range` is a copy of the element, not a pointer to the original memory!

---

## 3. `break`, `continue`, and Labels

- `break`: Exits the current loop or switch.
- `continue`: Skips to the next iteration.
- **Labels**: Break out of nested loops directly:
```go
OuterLoop:
for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        if i == 1 && j == 1 {
            break OuterLoop // Terminates outer loop
        }
    }
}
```

---

## 4. `switch` Statements

No implicit `fallthrough`; cases break automatically. Multiple match values allowed.

```go
// Expression Switch
switch day := "Mon"; day {
case "Mon", "Tue", "Wed", "Thu", "Fri":
    fmt.Println("Weekday")
case "Sat", "Sun":
    fmt.Println("Weekend")
default:
    fmt.Println("Unknown")
}

// Conditionless (Clean alternative to if-else-if chains)
score := 85
switch {
case score >= 90:
    fmt.Println("A")
case score >= 80:
    fmt.Println("B")
default:
    fmt.Println("C")
}
```

---

## 5. Type Switches

Checks dynamic type of an `interface{}` / `any`:
```go
func inspect(v any) {
    switch val := v.(type) {
    case int:
        fmt.Println("Integer:", val*2)
    case string:
        fmt.Println("String:", val)
    case bool:
        fmt.Println("Boolean:", val)
    default:
        fmt.Printf("Unknown type: %T\n", val)
    }
}
```

---

## 6. `defer` Statements

Schedules a function call to run **right before the enclosing function returns**.

```go
func processFile(filename string) error {
    f, err := os.Open(filename)
    if err != nil {
        return err
    }
    defer f.Close() // Guaranteed to run when processFile returns

    // Perform file work...
    return nil
}
```

### Key Defer Rules:
1. **LIFO Order**: Deferred calls execute in Last-In, First-Out order (stack).
2. **Immediate Evaluation**: Deferred arguments are evaluated when the `defer` line is encountered, not when it runs.

---

## 7. `panic` and `recover`

- `panic()`: Halts normal program execution, runs deferred functions, and crashes with stack trace.
- `recover()`: Recovers from a panic; only effective inside a `defer` function.

```go
func safeDivide(a, b int) (result int, err error) {
    defer func() {
        if r := recover(); r != nil {
            err = fmt.Errorf("recovered from panic: %v", r)
        }
    }()

    if b == 0 {
        panic("division by zero")
    }
    return a / b, nil
}
```

> **Best Practice**: Use standard `error` return values for expected errors. Reserve `panic` for unrecoverable programmer errors.
