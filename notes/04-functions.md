# 04. Functions

Function signatures, multiple returns, variadic args, closures, and first-class functions.

---

## 1. Syntax & Parameters

Functions are declared with `func`. Go parameters are **pass-by-value** (copies are made unless pointers are passed).

```go
// Group parameter types if they are the same
func add(x, y int) int {
    return x + y
}
```

---

## 2. Multiple & Named Return Values

Go functions natively return multiple values (the idiomatic way to return errors).

```go
// Multiple return values (Standard Go pattern: result, error)
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}
```

### Named Return Values
Values are initialized to their zero values. A bare `return` returns them (use sparingly in short functions):
```go
func minMax(a, b int) (min int, max int) {
    if a < b {
        min, max = a, b
    } else {
        min, max = b, a
    }
    return // Naked return: returns min, max
}
```

---

## 3. Variadic Functions (`...T`)

Accepts zero or more arguments of the specified type. Go receives them inside the function as a slice `[]T`.

```go
func sum(numbers ...int) int {
    total := 0
    for _, n := range numbers {
        total += n
    }
    return total
}

func main() {
    _ = sum(1, 2, 3, 4) // Pass multiple args
    
    // Pass existing slice using spread syntax '...'
    vals := []int{10, 20, 30}
    _ = sum(vals...)
}
```

---

## 4. First-Class Functions & Function Types

In Go, functions are first-class citizens: they can be assigned to variables, passed as arguments, or returned from other functions.

```go
// Custom function type definition
type MathOp func(int, int) int

func apply(a, b int, op MathOp) int {
    return op(a, b)
}

func main() {
    multiply := func(x, y int) int { return x * y }
    res := apply(4, 5, multiply) // 20
}
```

---

## 5. Closures & Anonymous Functions

An anonymous function can access and modify variables from its outer scope. The state persists across calls.

```go
func makeCounter() func() int {
    count := 0
    return func() int {
        count++ // Captures and retains 'count'
        return count
    }
}

func main() {
    counter := makeCounter()
    fmt.Println(counter()) // 1
    fmt.Println(counter()) // 2
}
```

---

## 6. Recursive Functions

Functions that call themselves must define an explicit base case to avoid stack overflow.

```go
func factorial(n uint) uint {
    if n <= 1 {
        return 1 // Base case
    }
    return n * factorial(n-1)
}
```
