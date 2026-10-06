# 05. Arrays

Fixed-size, contiguous memory sequences in Go.

---

## 1. Array Basics & Value Semantics

- Arrays have a **fixed size** determined at compile time.
- The length is **part of the type**: `[5]int` and `[10]int` are completely distinct, incompatible types.
- **Value Semantics**: In Go, assigning or passing an array **copies all elements**, not a pointer reference.

```go
// Declarations
var a [5]int                    // [0 0 0 0 0]
b := [3]string{"Go", "Rust", "C"}
c := [...]int{1, 2, 3, 4}       // Compiler counts elements (length = 4)

// Specific index initialization
d := [5]int{0: 10, 4: 50}       // [10 0 0 0 50]
```

---

## 2. Iteration

```go
nums := [3]int{10, 20, 30}

// Range loop
for idx, val := range nums {
    fmt.Printf("nums[%d] = %d\n", idx, val)
}

// Length check
fmt.Println("Length:", len(nums))
```

---

## 3. Multidimensional Arrays

Arrays of arrays (e.g., matrices, grids):

```go
var matrix [2][3]int = [2][3]int{
    {1, 2, 3},
    {4, 5, 6},
}

// Access: matrix[row][col]
val := matrix[1][2] // 6
```

---

## 4. Searching & Sorting

Because array sizes are fixed, sorting is typically performed by slicing the array (`arr[:]`), which provides a slice view to standard sorting utilities.

```go
import "sort"

arr := [...]int{5, 2, 6, 3, 1}

// Sort in-place by creating a slice view
sort.Ints(arr[:]) // arr is now [1, 2, 3, 5, 6]

// Binary search (array slice must be sorted first)
idx := sort.SearchInts(arr[:], 3) // idx = 2
```

> **Takeaway**: Slices (`[]T`) are far more common than arrays (`[N]T`) in Go, but arrays serve as the fixed-capacity backing storage for slices.
