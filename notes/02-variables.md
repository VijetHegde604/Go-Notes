# 02. Variables & Types

Variable declarations, Go primitive types, zero values, type conversions, and scoping rules.

---

## 1. Variable Declarations

### `var` Keyword
Usable at package level or inside functions:
```go
var name string = "Alice" // Explicit type & value
var age int               // Initialized to zero value (0)
var city = "Bangalore"    // Type inferred (string)

// Grouped declaration block
var (
    isActive bool
    counter  int = 10
)
```

### Short Declaration `:=`
Only usable **inside functions**. Infers the type automatically:
```go
func main() {
    count := 42
    x, y := 10, "hello" // Multiple short declaration
    
    // At least one variable on LHS must be new:
    x, z := 20, true    // Allowed: reassigns x, declares z
}
```

---

## 2. Basic Data Types

| Category | Types | Default Zero Value |
| :--- | :--- | :--- |
| **Boolean** | `bool` | `false` |
| **Integer** | `int`, `int8`, `int16`, `int32`, `int64` | `0` |
| **Unsigned Int** | `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr` | `0` |
| **Floating Point**| `float32`, `float64` | `0.0` |
| **Complex** | `complex64`, `complex128` | `(0+0i)` |
| **String** | `string` (UTF-8 immutable sequence of bytes) | `""` (empty string) |
| **Byte** | `byte` (alias for `uint8`) | `0` |
| **Rune** | `rune` (alias for `int32`, represents Unicode code point) | `0` |
| **Pointers/Ref** | pointers, slices, maps, channels, interfaces, functions | `nil` |

---

## 3. Type Conversion vs Type Inference

Go has **no implicit type conversion**. Types must be explicitly converted:
```go
var i int = 42
var f float64 = float64(i) // Explicit cast required
var u uint = uint(f)

// String conversion
s := string(rune(65)) // "A" (ASCII code point)
// For integer to string representation, use strconv:
// s := strconv.Itoa(42) // "42"
```

Type inference happens when initializing without an explicit type:
```go
num := 42       // inferred as int
pi := 3.14      // inferred as float64
flag := true    // inferred as bool
```

---

## 4. Constants & `iota`

Constants are evaluated at compile time and cannot change.

```go
const Pi = 3.14159
const (
    StatusActive   = "active"
    StatusInactive = "inactive"
)

// Untyped constants have arbitrary precision until assigned
const Big = 1 << 100
```

### `iota` (Enumerator)
Auto-increments starting from `0` in a `const` group:
```go
const (
    Sunday = iota // 0
    Monday        // 1
    Tuesday       // 2
    Wednesday     // 3
)

// Bit-mask flags with iota
const (
    Readable   = 1 << iota // 1 (1 << 0)
    Writable               // 2 (1 << 1)
    Executable             // 4 (1 << 2)
)
```

---

## 5. Variable Scope & Shadowing

- **Package scope**: Declared outside functions; accessible throughout the entire package.
- **Function / Block scope**: Declared inside a function or `{ ... }` block; only visible within that block.
- **Shadowing**: Declaring a variable with the same name in an inner block hides the outer variable.

```go
var count = 100 // Package level

func main() {
    count := 10 // Shadows package variable 'count'
    if count > 5 {
        count := 1 // Shadows main's 'count' inside this if-block only
        fmt.Println(count) // 1
    }
    fmt.Println(count) // 10
}
```
