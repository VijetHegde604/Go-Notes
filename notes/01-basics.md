# 01. Go Basics

Core foundation of Go programs: package declaration, structure, entry point, and output.

---

## 1. Package Declaration & Structure

Every Go source file starts with a `package` declaration:
- Executable programs **must** use `package main`.
- Libraries use the directory/module name as package name.

```go
package main // Entry package for executables

import "fmt" // Imported packages

func main() { // Execution starts here
    fmt.Println("Hello, World!")
}
```

- **File execution**: `go run main.go`
- **Build binary**: `go build`

---

## 2. Imports

Import standard library or third-party packages.

```go
// Factored import statement (idiomatic)
import (
    "fmt"
    "math"
    mbrand "math/rand" // Alias import
    _ "net/http/pprof"  // Blank import (runs init() only)
)
```

> **Rule**: Unused imports cause compile-time errors in Go.

---

## 3. Comments & Formatting

- Single-line: `// comment`
- Multi-line: `/* comment */`
- Package & exported function doc comments precede the declaration directly:
  ```go
  // Add returns the sum of two integers.
  func Add(a, b int) int { return a + b }
  ```
- **Automatic Formatting**: Always use `gofmt` (`go fmt ./...`). Go enforces consistent formatting across the ecosystem.

---

## 4. Semicolons & Syntax Rules

- Go automatically inserts semicolons at the end of statements based on grammar rules.
- **Critical rule**: Opening braces `{` must stay on the same line as statements (`func`, `if`, `for`), never on a new line.

```go
// Correct
func greet() {
}

// Syntax Error!
func greet()
{
}
```

---

## 5. Printing & Formatting (`fmt`)

| Function | Behavior |
| :--- | :--- |
| `fmt.Print()` | Prints without trailing newline or spaces between args |
| `fmt.Println()` | Prints with spaces between args and trailing newline `\n` |
| `fmt.Printf()` | Formatted output using format specifiers (verbs) |
| `fmt.Sprintf()` | Returns the formatted string instead of printing |

### Common Format Verbs:
- `%v` : Default format representation
- `%+v`: Struct field names + values
- `%#v`: Go-syntax representation of the value
- `%T` : Type of the value
- `%t` : Boolean (`true` or `false`)
- `%d` : Base 10 integer
- `%f` / `%.2f` : Floating point / 2 decimal places
- `%s` : Raw string
- `%q` : Double-quoted string safe escape
- `%p` : Pointer address (hex)

```go
name, score := "Gopher", 98.5
fmt.Printf("User %s scored %.1f%% (Type: %T)\n", name, score, score)
// Output: User Gopher scored 98.5% (Type: float64)
```
