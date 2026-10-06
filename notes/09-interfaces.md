# 09. Interfaces

Contracts defined by method sets, implicit satisfaction, empty interfaces, and type assertions.

---

## 1. Interface Basics & Implicit Satisfaction

An interface specifies a set of method signatures. In Go, types **satisfy interfaces implicitly** — there is no `implements` keyword.

```go
type Speaker interface {
    Speak() string
}

type Dog struct{}

// Implicitly implements Speaker
func (d Dog) Speak() string {
    return "Woof!"
}

func MakeSound(s Speaker) {
    fmt.Println(s.Speak())
}
```

---

## 2. Empty Interface (`any` / `interface{}`)

The empty interface specifies zero methods, meaning **every type satisfies it**. Go 1.18+ introduced `any` as an alias for `interface{}`.

```go
func PrintValue(val any) {
    fmt.Println(val)
}
```

> **Design Tip**: Keep interfaces small (1–3 methods). Idiomatic Go favors single-method interfaces like `io.Reader`, `io.Writer`, `fmt.Stringer`.

---

## 3. Type Assertions

Extracts the concrete value stored inside an interface.

```go
var val any = "hello world"

// 1. Unsafe assertion (PANICS if incorrect type)
s := val.(string)

// 2. Safe assertion (Comma-OK idiom)
s, ok := val.(string)
if ok {
    fmt.Println("String:", s)
} else {
    fmt.Println("Not a string!")
}

// Failing assertion with comma-ok does not panic:
n, ok := val.(int) // ok = false, n = 0
```

---

## 4. Interface Composition

Interfaces can embed other interfaces to build larger contracts:

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// Composed interface
type ReadWriter interface {
    Reader
    Writer
    // Can also add new methods here if needed
}
```

Any type that implements both `Read` and `Write` automatically satisfies `ReadWriter`.
