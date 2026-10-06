# 10. Error Handling

Go's explicit error handling, custom errors, wrapping with `%w`, and unwrapping.

---

## 1. Error Basics & The `error` Interface

Errors in Go are plain values representing abnormal conditions. The built-in interface is:

```go
type error interface {
    Error() string
}
```

### Standard Error Creation:
```go
import "errors"

// 1. Simple static error
var ErrNotFound = errors.New("resource not found")

// 2. Dynamic formatted error
func findUser(id int) (*User, error) {
    if id <= 0 {
        return nil, fmt.Errorf("invalid user ID: %d", id)
    }
    // ...
    return nil, ErrNotFound
}
```

### Standard Handling Pattern:
```go
val, err := findUser(-1)
if err != nil {
    // Handle error immediately
    log.Println("Operation failed:", err)
    return err
}
```

---

## 2. Custom Error Types

Define custom structs that implement `Error() string` when rich context (e.g. status codes, query strings, failed fields) is needed:

```go
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation failed on '%s': %s", e.Field, e.Message)
}

func validateAge(age int) error {
    if age < 0 {
        return &ValidationError{Field: "Age", Message: "cannot be negative"}
    }
    return nil
}
```

---

## 3. Error Wrapping & Unwrapping (Go 1.13+)

Use `%w` verb in `fmt.Errorf` to wrap an underlying error with contextual information.

```go
func readConfig(path string) error {
    _, err := os.Open(path)
    if err != nil {
        return fmt.Errorf("reading config from %s: %w", path, err)
    }
    return nil
}
```

### Checking Wrapped Errors:

| Function | Purpose | Example |
| :--- | :--- | :--- |
| `errors.Is(err, target)` | Checks if any error in the tree matches target (sentinel value) | `errors.Is(err, os.ErrNotExist)` |
| `errors.As(err, &target)` | Finds the first error matching target type and binds it | `var valErr *ValidationError`<br>`errors.As(err, &valErr)` |
| `errors.Unwrap(err)` | Unwraps one layer to expose underlying error | `underlying := errors.Unwrap(err)` |

```go
err := readConfig("missing.yaml")

// Check against sentinel error
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("Config file does not exist!")
}

// Extract custom error type from wrapped chain
var vErr *ValidationError
if errors.As(err, &vErr) {
    fmt.Printf("Validation error on field: %s\n", vErr.Field)
}
```
