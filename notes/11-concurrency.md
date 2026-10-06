# 11. Concurrency

Goroutines, channels, synchronization primitives (`WaitGroup`, `Mutex`), and channel orchestration.

---

## 1. Goroutines

Goroutines are lightweight, concurrently executed functions managed by the Go runtime (starting with just ~2KB stack).

```go
func printMessage(msg string) {
    fmt.Println(msg)
}

func main() {
    // Launch a concurrent goroutine
    go printMessage("running concurrently")
    
    // Anonymous goroutine
    go func(x int) {
        fmt.Println("Value:", x)
    }(42)
}
```

> **Note**: When `main()` terminates, all active background goroutines terminate immediately. Use synchronization to wait for them.

---

## 2. Channels

Channels provide safe communication and synchronization between goroutines without explicit locks ("*Do not communicate by sharing memory; instead, share memory by communicating*").

```go
// 1. Unbuffered channel (Synchronous: send blocks until receiver is ready)
ch := make(chan int)

// 2. Buffered channel (Asynchronous up to buffer capacity)
bufCh := make(chan string, 3)

// Sending & Receiving
go func() {
    ch <- 42 // Send
}()
val := <-ch  // Receive (blocks until value is sent)
```

### Channel Operations & Properties:

| Operation | Nil Channel | Open Channel | Closed Channel |
| :--- | :--- | :--- | :--- |
| **Send** `ch <- v` | Blocks forever | Sends value (or blocks if full) | **Panics!** |
| **Receive** `<-ch` | Blocks forever | Receives value (or blocks if empty) | Returns zero value immediately |
| **Close** `close(ch)`| **Panics!** | Closes channel | **Panics!** |

### Closing and Iterating with `range`:
```go
jobs := make(chan int, 5)

go func() {
    for i := 1; i <= 3; i++ {
        jobs <- i
    }
    close(jobs) // Only sender closes the channel!
}()

// Loops until channel is closed and drained
for job := range jobs {
    fmt.Println("Job:", job)
}

// Checking if channel is closed (comma-ok)
val, ok := <-jobs // ok is false if channel is empty & closed
```

---

## 3. Synchronization Primitives (`sync`)

### `sync.WaitGroup`
Waits for a collection of goroutines to finish:
```go
import "sync"

var wg sync.WaitGroup

for i := 1; i <= 3; i++ {
    wg.Add(1) // Increment counter
    go func(id int) {
        defer wg.Done() // Decrement counter when done
        fmt.Printf("Worker %d finished\n", id)
    }(i)
}

wg.Wait() // Blocks until counter reaches 0
```

### `sync.Mutex` & `sync.RWMutex`
Protects shared state from data races:
```go
type SafeCounter struct {
    mu    sync.Mutex
    count int
}

func (c *SafeCounter) Inc() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.count++
}

func (c *SafeCounter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.count
}
```

- **`sync.RWMutex`**: Allows multiple concurrent readers (`RLock()` / `RUnlock()`) but exclusive access for writers (`Lock()` / `Unlock()`).
- **Data Race Detection**: Run tests and binaries with `go test -race` or `go run -race main.go`.
