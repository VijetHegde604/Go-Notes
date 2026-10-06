# 08. Structs

User-defined composite types, methods, composition via embedding, and field tags.

---

## 1. Struct Definition & Instantiation

```go
type User struct {
    ID       int
    Username string
    Email    string
    IsActive bool
}

// Instantiation:
u1 := User{ID: 1, Username: "alex", Email: "alex@example.com", IsActive: true}
u2 := User{Username: "sam"} // Other fields take zero values

// Pointer to struct
u3 := &User{ID: 2, Username: "taylor"}
u3.Email = "t@t.com" // Go automatically dereferences pointers for struct fields
```

---

## 2. Methods: Value vs Pointer Receivers

Methods are functions attached to a specific receiver type.

```go
// 1. Value Receiver: Operates on a COPY (cannot modify original)
func (u User) Display() string {
    return fmt.Sprintf("%s <%s>", u.Username, u.Email)
}

// 2. Pointer Receiver: Can MUTATE original, avoids copying large structs
func (u *User) Deactivate() {
    u.IsActive = false
}
```

### When to Use Pointer Receivers:
- When the method needs to mutate the struct fields.
- When the struct is large and copying overhead is expensive.
- **Consistency**: If any method on a struct requires a pointer receiver, make all methods on that struct pointer receivers.

---

## 3. Struct Embedding (Composition over Inheritance)

Go has **no class inheritance**. Instead, it uses **composition via struct embedding** (anonymous fields).

```go
type Engine struct {
    Horsepower int
}

func (e Engine) Start() {
    fmt.Println("Vroom! HP:", e.Horsepower)
}

type Car struct {
    Make  string
    Model string
    Engine // Embedded struct (no field name)
}

func main() {
    c := Car{
        Make:  "Ford",
        Model: "Mustang",
        Engine: Engine{Horsepower: 450},
    }

    // Promoted fields & methods:
    fmt.Println(c.Horsepower) // Direct access (promoted from Engine)
    c.Start()                 // Promoted method call
}
```

---

## 4. Struct Tags

Metadata strings attached to fields, widely used by serialization packages (`encoding/json`, ORMs, validators).

```go
type Product struct {
    ID        int      `json:"id"`
    Name      string   `json:"name"`
    Price     float64  `json:"price,string"`   // Serializes float as string
    SecretKey string   `json:"-"`              // Ignored in JSON
    Discount  *float64 `json:"discount,omitempty"` // Omitted if nil or zero
}
```

> **Syntax Rule**: Tags are enclosed in backticks `` `key:"value"` ``. Fields must be **Exported** (capitalized) for `encoding/json` to access them!
