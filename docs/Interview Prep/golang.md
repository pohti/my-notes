# Golang

## Table of Contents
- [Basic Concepts](#basic-concepts)
- [Data Types & Variables](#data-types--variables)
- [Functions & Methods](#functions--methods)
- [Pointers](#pointers)
- [Structs & Interfaces](#structs--interfaces)
- [Goroutines & Concurrency](#goroutines--concurrency)
- [Channels](#channels)
- [Error Handling](#error-handling)
- [Memory Management](#memory-management)
- [Package Management](#package-management)
- [Testing](#testing)
- [Performance & Optimization](#performance--optimization)

---

## Basic Concepts

### 1. What is Go and what are its key features?

**Answer:**
Go (or Golang) is an open-source programming language developed by Google in 2007. Key features include:

- **Compiled language**: Fast execution and deployment
- **Statically typed**: Type safety with compile-time checking
- **Garbage collected**: Automatic memory management
- **Built-in concurrency**: Goroutines and channels
- **Simple syntax**: Clean, readable code
- **Fast compilation**: Quick build times
- **Cross-platform**: Compiles to multiple architectures
- **Rich standard library**: Comprehensive built-in packages

### 2. What makes Go different from other programming languages?

**Answer:**
- **Simplicity**: Minimal syntax with only 25 keywords
- **Built-in concurrency**: Goroutines are lighter than threads
- **No inheritance**: Uses composition over inheritance
- **Interface satisfaction**: Implicit interface implementation
- **Fast compilation**: Dependency management and linking
- **No generics** (until Go 1.18): Encouraged interface usage
- **Opinionated formatting**: `gofmt` enforces consistent style

### 3. Explain Go's compilation process.

**Answer:**
1. **Lexical Analysis**: Source code → tokens
2. **Parsing**: Tokens → Abstract Syntax Tree (AST)
3. **Type Checking**: Verify types and interface compliance
4. **Code Generation**: AST → machine code
5. **Linking**: Combine object files and dependencies
6. **Output**: Single binary executable

```go
// Build process
go build main.go        // Compile to executable
go run main.go         // Compile and run
go install            // Build and install to GOPATH/bin
```

---

## Data Types & Variables

### 4. What are the basic data types in Go?

**Answer:**
```go
// Numeric types
var i int = 42
var i8 int8 = 127
var ui uint = 42
var f32 float32 = 3.14
var f64 float64 = 3.14159

// String and character
var s string = "hello"
var r rune = 'A'    // Unicode code point
var b byte = 65     // Alias for uint8

// Boolean
var flag bool = true

// Complex numbers
var c complex64 = 1 + 2i
var c128 complex128 = 1 + 2i
```

### 5. What's the difference between `var`, `:=`, and `const`?

**Answer:**
```go
// var - explicit declaration
var name string = "John"
var age int        // Zero value: 0
var isActive bool  // Zero value: false

// := - short variable declaration (type inference)
name := "John"     // Only inside functions
age := 25

// const - compile-time constants
const Pi = 3.14159
const (
    StatusOK = 200
    StatusNotFound = 404
)

// iota - constant generator
const (
    Sunday = iota    // 0
    Monday           // 1
    Tuesday          // 2
)
```

### 6. What are zero values in Go?

**Answer:**
Go automatically initializes variables with zero values:

```go
var i int        // 0
var f float64    // 0.0
var s string     // ""
var b bool       // false
var p *int       // nil
var slice []int  // nil
var m map[string]int // nil
var ch chan int  // nil
var fn func()    // nil
```

---

## Functions & Methods

### 7. How do you define functions in Go?

**Answer:**
```go
// Basic function
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Named return values
func calculate(a, b int) (sum, product int) {
    sum = a + b
    product = a * b
    return // naked return
}

// Variadic functions
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

// Function as type
type Calculator func(int, int) int

func operate(fn Calculator, a, b int) int {
    return fn(a, b)
}
```

### 8. What's the difference between methods and functions?

**Answer:**
```go
// Function - standalone
func area(r float64) float64 {
    return math.Pi * r * r
}

// Method - associated with a type
type Circle struct {
    Radius float64
}

// Method with value receiver
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// Method with pointer receiver
func (c *Circle) SetRadius(r float64) {
    c.Radius = r
}

// Usage
c := Circle{Radius: 5}
fmt.Println(c.Area())  // Method call
c.SetRadius(10)        // Pointer receiver needed for modification
```

### 9. When should you use pointer receivers vs value receivers?

**Answer:**
**Use pointer receivers when:**
- You need to modify the receiver
- The receiver is large (copying is expensive)
- You want to maintain consistency (if one method uses pointer, all should)

**Use value receivers when:**
- The receiver is small and immutable
- The receiver is a basic type, slice, or small struct
- You want to work with a copy

```go
type Person struct {
    Name string
    Age  int
}

// Value receiver - doesn't modify original
func (p Person) String() string {
    return fmt.Sprintf("%s (%d)", p.Name, p.Age)
}

// Pointer receiver - modifies original
func (p *Person) Birthday() {
    p.Age++
}
```

---

## Pointers

### 10. Explain pointers in Go.

**Answer:**
```go
// Pointer declaration and usage
var x int = 42
var p *int = &x    // p points to x
fmt.Println(*p)    // Dereference: prints 42

*p = 100          // Change value through pointer
fmt.Println(x)    // prints 100

// Pointer to pointer
var pp **int = &p
fmt.Println(**pp) // prints 100

// nil pointer
var ptr *int
if ptr == nil {
    fmt.Println("Pointer is nil")
}
```

### 11. What's the difference between passing by value and passing by pointer?

**Answer:**
```go
// Pass by value - creates copy
func modifyValue(x int) {
    x = 100
}

// Pass by pointer - modifies original
func modifyPointer(x *int) {
    *x = 100
}

func main() {
    num := 42
    
    modifyValue(num)
    fmt.Println(num)    // Still 42
    
    modifyPointer(&num)
    fmt.Println(num)    // Now 100
}
```

---

## Structs & Interfaces

### 12. How do you define and use structs in Go?

**Answer:**
```go
// Struct definition
type Person struct {
    Name    string
    Age     int
    Email   string
    private string  // unexported field
}

// Constructor function
func NewPerson(name string, age int) *Person {
    return &Person{
        Name: name,
        Age:  age,
    }
}

// Struct literals
p1 := Person{Name: "John", Age: 30}
p2 := Person{"Jane", 25, "jane@example.com", ""}
p3 := &Person{Name: "Bob"}  // Other fields get zero values

// Anonymous structs
config := struct {
    Host string
    Port int
}{
    Host: "localhost",
    Port: 8080,
}
```

### 13. Explain interfaces in Go.

**Answer:**
```go
// Interface definition
type Writer interface {
    Write([]byte) (int, error)
}

type Reader interface {
    Read([]byte) (int, error)
}

// Interface composition
type ReadWriter interface {
    Reader
    Writer
}

// Implementation (implicit)
type FileWriter struct {
    filename string
}

func (fw FileWriter) Write(data []byte) (int, error) {
    // Implementation here
    return len(data), nil
}

// Empty interface
var anything interface{} = "hello"
anything = 42
anything = []int{1, 2, 3}

// Type assertion
if str, ok := anything.(string); ok {
    fmt.Println("String:", str)
}

// Type switch
switch v := anything.(type) {
case string:
    fmt.Println("String:", v)
case int:
    fmt.Println("Integer:", v)
default:
    fmt.Printf("Unknown type: %T\n", v)
}
```

### 14. What's the difference between empty interface and concrete types?

**Answer:**
```go
// Empty interface can hold any value
var empty interface{}
empty = "string"
empty = 42
empty = []int{1, 2, 3}

// Concrete type is specific
var str string = "hello"
var num int = 42

// Type assertion needed for empty interface
if s, ok := empty.(string); ok {
    fmt.Println(len(s))  // Can use string methods
}

// Concrete types have compile-time type safety
fmt.Println(len(str))  // No type assertion needed
```

---

## Goroutines & Concurrency

### 15. What are goroutines and how do they work?

**Answer:**
Goroutines are lightweight threads managed by Go runtime. They're much cheaper than OS threads.

```go
// Starting a goroutine
go func() {
    fmt.Println("Running in goroutine")
}()

// Named function as goroutine
func worker(id int) {
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    // Start multiple goroutines
    for i := 1; i <= 3; i++ {
        go worker(i)
    }
    
    // Wait for goroutines to finish
    time.Sleep(2 * time.Second)
}
```

### 16. How do you coordinate goroutines?

**Answer:**
```go
import "sync"

// Using WaitGroup
func withWaitGroup() {
    var wg sync.WaitGroup
    
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go func(id int) {
            defer wg.Done()
            fmt.Printf("Worker %d\n", id)
        }(i)
    }
    
    wg.Wait()
}

// Using Mutex for shared data
type Counter struct {
    mu    sync.Mutex
    value int
}

func (c *Counter) Inc() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.value++
}

func (c *Counter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.value
}

// Using channels for communication
func withChannels() {
    done := make(chan bool)
    
    go func() {
        fmt.Println("Working...")
        time.Sleep(time.Second)
        done <- true
    }()
    
    <-done // Wait for completion
}
```

### 17. What's the difference between goroutines and threads?

**Answer:**
| Aspect | Goroutines | OS Threads |
|--------|------------|------------|
| **Memory** | 2KB initial stack | 2MB initial stack |
| **Creation** | Very fast | Slower |
| **Context switching** | Managed by Go runtime | Managed by OS |
| **Communication** | Channels (CSP model) | Shared memory |
| **Scalability** | Hundreds of thousands | Thousands |
| **Scheduling** | Cooperative (Go scheduler) | Preemptive (OS scheduler) |

---

## Channels

### 18. What are channels and how do you use them?

**Answer:**
```go
// Channel creation
ch := make(chan int)           // Unbuffered
buffered := make(chan int, 5)  // Buffered

// Sending and receiving
go func() {
    ch <- 42  // Send
}()

value := <-ch  // Receive

// Directional channels
func sender(ch chan<- int) {   // Send-only
    ch <- 42
}

func receiver(ch <-chan int) { // Receive-only
    value := <-ch
    fmt.Println(value)
}

// Closing channels
close(ch)

// Check if channel is closed
value, ok := <-ch
if !ok {
    fmt.Println("Channel is closed")
}

// Range over channel
for value := range ch {
    fmt.Println(value)
}
```

### 19. What's the difference between buffered and unbuffered channels?

**Answer:**
```go
// Unbuffered channel - synchronous
unbuffered := make(chan int)
go func() {
    unbuffered <- 1  // Blocks until received
}()
value := <-unbuffered  // Blocks until sent

// Buffered channel - asynchronous
buffered := make(chan int, 3)
buffered <- 1  // Doesn't block (buffer has space)
buffered <- 2
buffered <- 3
// buffered <- 4  // Would block (buffer full)

fmt.Println(<-buffered)  // 1
fmt.Println(<-buffered)  // 2
```

### 20. How do you implement worker pools using channels?

**Answer:**
```go
func workerPool() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    
    // Start workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    
    // Send jobs
    for j := 1; j <= 9; j++ {
        jobs <- j
    }
    close(jobs)
    
    // Collect results
    for r := 1; r <= 9; r++ {
        <-results
    }
}

func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second)
        results <- job * 2
    }
}
```

### 21. Explain select statement with channels.

**Answer:**
```go
func selectExample() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "from ch1"
    }()
    
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "from ch2"
    }()
    
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        case <-time.After(3 * time.Second):
            fmt.Println("timeout")
        default:
            fmt.Println("no communication ready")
            time.Sleep(500 * time.Millisecond)
        }
    }
}
```

---

## Error Handling

### 22. How does error handling work in Go?

**Answer:**
```go
// Error interface
type error interface {
    Error() string
}

// Function returning error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide %v by zero", a)
    }
    return a / b, nil
}

// Error handling
result, err := divide(10, 0)
if err != nil {
    log.Fatal(err)
}

// Multiple error checking
file, err := os.Open("file.txt")
if err != nil {
    return err
}
defer file.Close()

data, err := ioutil.ReadAll(file)
if err != nil {
    return err
}
```

### 23. How do you create custom errors?

**Answer:**
```go
// Simple custom error
type ValidationError struct {
    Field   string
    Message string
}

func (e ValidationError) Error() string {
    return fmt.Sprintf("validation failed for %s: %s", e.Field, e.Message)
}

// Using errors.New
var ErrNotFound = errors.New("item not found")

// Using fmt.Errorf
func processUser(id int) error {
    if id < 0 {
        return fmt.Errorf("invalid user ID: %d", id)
    }
    return nil
}

// Error wrapping (Go 1.13+)
func readConfig() error {
    _, err := os.Open("config.json")
    if err != nil {
        return fmt.Errorf("failed to read config: %w", err)
    }
    return nil
}

// Error unwrapping
err := readConfig()
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("Config file doesn't exist")
}
```

### 24. What are the best practices for error handling in Go?

**Answer:**
1. **Always check errors**: Don't ignore returned errors
2. **Handle errors close to source**: Check errors immediately
3. **Provide context**: Wrap errors with additional information
4. **Use appropriate error types**: Custom errors for different scenarios
5. **Don't panic**: Use panic only for unrecoverable errors

```go
// Good error handling
func processFile(filename string) error {
    file, err := os.Open(filename)
    if err != nil {
        return fmt.Errorf("failed to open file %q: %w", filename, err)
    }
    defer func() {
        if cerr := file.Close(); cerr != nil {
            log.Printf("failed to close file: %v", cerr)
        }
    }()
    
    // Process file...
    return nil
}
```

---

## Memory Management

### 25. How does garbage collection work in Go?

**Answer:**
Go uses a **tricolor concurrent mark-and-sweep** garbage collector:

1. **Mark Phase**: Mark all reachable objects
2. **Sweep Phase**: Deallocate unmarked objects
3. **Concurrent**: Runs alongside application code

```go
// Triggering GC manually (usually not needed)
runtime.GC()

// Memory statistics
var m runtime.MemStats
runtime.ReadMemStats(&m)
fmt.Printf("Allocated memory: %d KB\n", m.Alloc/1024)

// Finalizers (use sparingly)
runtime.SetFinalizer(obj, (*Object).cleanup)
```

### 26. What causes memory leaks in Go?

**Answer:**
Common causes:
1. **Goroutine leaks**: Goroutines that never terminate
2. **Channel leaks**: Blocked goroutines waiting on channels
3. **Slice leaks**: Holding references to large underlying arrays
4. **Map leaks**: Not removing entries from maps
5. **Timer leaks**: Not stopping timers

```go
// Goroutine leak example
func leak() {
    go func() {
        for {
            // This goroutine never exits
            time.Sleep(time.Second)
        }
    }()
}

// Fix: provide exit condition
func fixed() {
    done := make(chan bool)
    go func() {
        for {
            select {
            case <-done:
                return
            default:
                time.Sleep(time.Second)
            }
        }
    }()
    
    // Later: done <- true
}
```

---

## Package Management

### 27. How do Go modules work?

**Answer:**
```bash
# Initialize module
go mod init github.com/username/project

# Add dependency
go get github.com/gin-gonic/gin

# Update dependencies
go mod tidy

# Vendor dependencies
go mod vendor

# Download dependencies
go mod download
```

```go
// go.mod file
module github.com/username/project

go 1.19

require (
    github.com/gin-gonic/gin v1.9.1
)

// go.sum contains checksums for security
```

### 28. What's the difference between `go get` and `go install`?

**Answer:**
- **`go get`**: Downloads and adds dependencies to current module
- **`go install`**: Builds and installs binaries to `$GOPATH/bin`

```bash
# go get - adds to dependencies
go get github.com/gin-gonic/gin@v1.9.1

# go install - builds binary
go install github.com/username/tool@latest
```

---

## Testing

### 29. How do you write tests in Go?

**Answer:**
```go
// math.go
func Add(a, b int) int {
    return a + b
}

// math_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    expected := 5
    
    if result != expected {
        t.Errorf("Add(2, 3) = %d; want %d", result, expected)
    }
}

// Table-driven tests
func TestAddTable(t *testing.T) {
    tests := []struct {
        a, b, expected int
    }{
        {2, 3, 5},
        {0, 0, 0},
        {-1, 1, 0},
    }
    
    for _, test := range tests {
        result := Add(test.a, test.b)
        if result != test.expected {
            t.Errorf("Add(%d, %d) = %d; want %d", 
                test.a, test.b, result, test.expected)
        }
    }
}

// Benchmark
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

### 30. How do you test concurrent code?

**Answer:**
```go
func TestConcurrent(t *testing.T) {
    // Use race detector: go test -race
    
    counter := 0
    var mu sync.Mutex
    var wg sync.WaitGroup
    
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            mu.Lock()
            counter++
            mu.Unlock()
        }()
    }
    
    wg.Wait()
    
    if counter != 100 {
        t.Errorf("Expected 100, got %d", counter)
    }
}
```

---

## Performance & Optimization

### 31. How do you profile Go applications?

**Answer:**
```go
import _ "net/http/pprof"

func main() {
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()
    
    // Your application code
}
```

```bash
# CPU profiling
go tool pprof http://localhost:6060/debug/pprof/profile

# Memory profiling
go tool pprof http://localhost:6060/debug/pprof/heap

# Goroutine profiling
go tool pprof http://localhost:6060/debug/pprof/goroutine
```

### 32. What are some Go performance best practices?

**Answer:**
1. **Use pointers for large structs**
2. **Prefer slices over arrays for flexibility**
3. **Pre-allocate slices when size is known**
4. **Use sync.Pool for object reuse**
5. **Avoid unnecessary allocations**
6. **Use buffered I/O**
7. **Profile before optimizing**

```go
// Pre-allocate slices
data := make([]int, 0, 1000)  // len=0, cap=1000

// Object pooling
var pool = sync.Pool{
    New: func() interface{} {
        return make([]byte, 1024)
    },
}

buf := pool.Get().([]byte)
defer pool.Put(buf)

// Use strings.Builder for string concatenation
var builder strings.Builder
for i := 0; i < 100; i++ {
    builder.WriteString("hello ")
}
result := builder.String()
```

---

## Advanced Topics

### 33. What is reflection in Go and when should you use it?

**Answer:**
```go
import "reflect"

func inspectType(x interface{}) {
    t := reflect.TypeOf(x)
    v := reflect.ValueOf(x)
    
    fmt.Printf("Type: %s\n", t.Name())
    fmt.Printf("Kind: %s\n", t.Kind())
    fmt.Printf("Value: %v\n", v.Interface())
    
    // For structs
    if t.Kind() == reflect.Struct {
        for i := 0; i < t.NumField(); i++ {
            field := t.Field(i)
            value := v.Field(i)
            fmt.Printf("Field: %s, Value: %v\n", 
                field.Name, value.Interface())
        }
    }
}

// JSON marshaling uses reflection
type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
```

**Use reflection sparingly**: It's slower and less type-safe than regular code.

### 34. Explain context in Go.

**Answer:**
```go
import "context"

// Context with timeout
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

// Context with cancellation
ctx, cancel := context.WithCancel(context.Background())
defer cancel()

// Context with values
ctx = context.WithValue(ctx, "user-id", 123)

// Using context in functions
func doWork(ctx context.Context) error {
    select {
    case <-time.After(2 * time.Second):
        return nil
    case <-ctx.Done():
        return ctx.Err()  // context.Canceled or context.DeadlineExceeded
    }
}

// HTTP with context
req, _ := http.NewRequestWithContext(ctx, "GET", "https://api.example.com", nil)
```

### 35. What are some common Go interview coding challenges?

**Answer:**

**1. Implement a concurrent safe counter:**
```go
type SafeCounter struct {
    mu sync.Mutex
    v  map[string]int
}

func (c *SafeCounter) Inc(key string) {
    c.mu.Lock()
    c.v[key]++
    c.mu.Unlock()
}

func (c *SafeCounter) Value(key string) int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.v[key]
}
```

**2. Find duplicate elements in slice:**
```go
func findDuplicates(nums []int) []int {
    seen := make(map[int]bool)
    duplicates := make(map[int]bool)
    var result []int
    
    for _, num := range nums {
        if seen[num] {
            if !duplicates[num] {
                result = append(result, num)
                duplicates[num] = true
            }
        } else {
            seen[num] = true
        }
    }
    
    return result
}
```

**3. Implement rate limiter:**
```go
type RateLimiter struct {
    tokens chan struct{}
    ticker *time.Ticker
}

func NewRateLimiter(rate int) *RateLimiter {
    rl := &RateLimiter{
        tokens: make(chan struct{}, rate),
        ticker: time.NewTicker(time.Second / time.Duration(rate)),
    }
    
    go func() {
        for range rl.ticker.C {
            select {
            case rl.tokens <- struct{}{}:
            default:
            }
        }
    }()
    
    return rl
}

func (rl *RateLimiter) Allow() bool {
    select {
    case <-rl.tokens:
        return true
    default:
        return false
    }
}
```

---

## Quick Reference

### Common Gotchas
1. **Loop variable capture**: Use function parameter or local variable
2. **Slice append**: May create new underlying array
3. **Map iteration**: Order is not guaranteed
4. **Nil interface**: Interface containing nil pointer is not nil
5. **Defer execution**: Deferred functions run in LIFO order

### Performance Tips
- Use `make()` with capacity for slices and maps
- Prefer `strings.Builder` for string concatenation
- Use `sync.Pool` for object reuse
- Profile with `go tool pprof`
- Run tests with `-race` flag

### Testing Commands
```bash
go test ./...                    # Run all tests
go test -v                       # Verbose output
go test -race                    # Race detection
go test -bench=.                 # Run benchmarks
go test -cover                   # Coverage report
go test -cpuprofile=cpu.prof     # CPU profiling
```

---

*This document covers the most common Golang interview questions. Practice implementing these concepts and understanding the underlying principles for best results in interviews.*