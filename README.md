# Go Notes

Clear, concise, and readable notes on completed Go concepts from [goforgo](https://github.com/VijetHegde604/goforgo.git).

---

## Completed Topics & Notes Index

| # | Topic | Note Link | Completed Exercises | Key Concepts |
| :-: | :--- | :--- | :-: | :--- |
| **01** | **Basics** | [01-basics.md](notes/01-basics.md) | 10/10 | Program structure, `package main`, `main()`, imports, comments, formatting, semicolons, `fmt.Printf` verbs |
| **02** | **Variables & Types** | [02-variables.md](notes/02-variables.md) | 9/9 | `var` vs `:=`, primitive types, zero values, explicit conversion, type inference, `const`, `iota`, scope & shadowing |
| **03** | **Control Flow** | [03-control-flow.md](notes/03-control-flow.md) | 10/10 | `if` with short init, `for` loop patterns, `range`, `switch`, type switch, `break`/`continue` labels, `defer`, `panic`/`recover` |
| **04** | **Functions** | [04-functions.md](notes/04-functions.md) | 8/8 | Parameter pass-by-value, multiple returns, named returns, variadic args (`...T`), first-class functions, closures, recursion |
| **05** | **Arrays** | [05-arrays.md](notes/05-arrays.md) | 5/5 | Fixed size `[N]T`, value copy semantics, iteration, 2D matrices, searching & sorting via slice views |
| **06** | **Slices** | [06-slices.md](notes/06-slices.md) | 6/6 | Slice header (`ptr`, `len`, `cap`), `append()` growth, sub-slicing, deep copy with `copy()`, custom `sort.Slice`, slice tricks |
| **07** | **Maps** | [07-maps.md](notes/07-maps.md) | 5/5 | Hash maps (`make(map[K]V)`), comma-ok idiom, randomized iteration, nil map rules, set patterns (`map[T]struct{}`) |
| **08** | **Structs** | [08-structs.md](notes/08-structs.md) | 4/4 | Struct literals, value vs pointer receivers, composition via embedding, promoted fields/methods, struct tags (`json:"..."`) |
| **09** | **Interfaces** | [09-interfaces.md](notes/09-interfaces.md) | 4/4 | Implicit satisfaction, `any` (`interface{}`), safe type assertions (`v, ok := i.(T)`), interface composition |
| **10** | **Errors** | [10-errors.md](notes/10-errors.md) | 3/3 | `error` interface, `errors.New`, custom error structs, error wrapping (`%w`), `errors.Is`, `errors.As`, `Unwrap()` |
| **11** | **Concurrency** | [11-concurrency.md](notes/11-concurrency.md) | 3/5 | Goroutines (`go func()`), unbuffered & buffered channels, channel lifecycle & `range`, `sync.WaitGroup`, `sync.Mutex` / `RWMutex` |

**Total Completed Exercises**: 67

---

## Quick Navigation

- [01. Basics](notes/01-basics.md)
- [02. Variables & Types](notes/02-variables.md)
- [03. Control Flow](notes/03-control-flow.md)
- [04. Functions](notes/04-functions.md)
- [05. Arrays](notes/05-arrays.md)
- [06. Slices](notes/06-slices.md)
- [07. Maps](notes/07-maps.md)
- [08. Structs](notes/08-structs.md)
- [09. Interfaces](notes/09-interfaces.md)
- [10. Errors](notes/10-errors.md)
- [11. Concurrency](notes/11-concurrency.md)
