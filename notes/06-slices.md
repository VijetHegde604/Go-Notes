# 06. Slices

Dynamically sized, flexible views into backing arrays. The primary collection type in Go.

---

## 1. Slice Header & Creation

A slice consists of 3 fields internally (the **slice header**):
1. **Pointer**: address of the first element in the backing array.
2. **Length (`len`)**: number of elements currently in the slice.
3. **Capacity (`cap`)**: maximum number of elements from the slice start to the end of the backing array.

```go
// 1. Literal
s1 := []int{1, 2, 3}

// 2. make(type, len, cap)
s2 := make([]int, 3, 5) // len=3, cap=5 -> [0, 0, 0]

// 3. Slicing an existing array/slice
arr := [5]int{10, 20, 30, 40, 50}
s3 := arr[1:4] // elements at indices 1, 2, 3 -> [20, 30, 40]
// len = 3 (4 - 1), cap = 4 (5 - 1)
```

---

## 2. `append()` & Dynamic Growth

When `append()` exceeds the backing array's `cap`, Go allocates a new, larger backing array and copies elements over.

```go
var nums []int // nil slice, len=0, cap=0

nums = append(nums, 1)          // [1]
nums = append(nums, 2, 3)       // [1, 2, 3]
nums = append(nums, []int{4, 5}...) // Append another slice with '...'
```

> **Important**: Always reassign the result back: `s = append(s, val)`.

---

## 3. Deep Copy with `copy()`

Assignment (`b := a`) or slicing (`b := a[1:3]`) shares the same backing array. Modifying `b` mutates `a`!  
Use `copy()` for independent deep copies:

```go
src := []int{1, 2, 3}
dst := make([]int, len(src)) // Must allocate destination capacity first!

n := copy(dst, src) // Copies min(len(dst), len(src))
```

---

## 4. Custom Sorting (`sort.Slice`)

Sort arbitrary slices using a custom comparison function:

```go
import "sort"

type Person struct {
    Name string
    Age  int
}

people := []Person{{"Bob", 30}, {"Alice", 25}, {"Charlie", 25}}

// Sort by Age ascending, then Name
sort.Slice(people, func(i, j int) bool {
    if people[i].Age == people[j].Age {
        return people[i].Name < people[j].Name
    }
    return people[i].Age < people[j].Age
})
```

---

## 5. Common Slice Manipulation Tricks

```go
s := []int{0, 1, 2, 3, 4, 5}

// Delete element at index i (e.g., i = 2)
i := 2
s = append(s[:i], s[i+1:]...) // [0, 1, 3, 4, 5]

// Insert element val at index i
val := 99
s = append(s[:i], append([]int{val}, s[i:]...)...)

// Reverse a slice in-place
for left, right := 0, len(s)-1; left < right; left, right = left+1, right-1 {
    s[left], s[right] = s[right], s[left]
}

// Cut / Remove sub-range [from:to]
from, to := 1, 3
s = append(s[:from], s[to:]...)
```
