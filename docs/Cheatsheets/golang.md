# Go

## Basic Syntax

### Hello World
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Running Go Programs
```bash
go run main.go           # Run program
go build main.go         # Compile to binary
go build -o app main.go  # Compile with custom name
go install               # Install binary to $GOPATH/bin
go fmt ./...             # Format all files
go vet ./...             # Check for common mistakes
go test ./...            # Run tests
go mod init module-name  # Initialize module
go mod tidy              # Clean up dependencies
go get package-name      # Download dependency
```

## Variables & Constants

### Variable Declaration
```go
// Explicit type
var name string = "John"
var age int = 30
var isActive bool = true

// Type inference
var name = "John"
var age = 30

// Short declaration (inside functions only)
name := "John"
age := 30

// Multiple variables
var x, y int = 1, 2
a, b := 3, 4
var (
    name string = "John"
    age  int    = 30
)

// Zero values (default values)
var s string  // ""
var i int     // 0
var f float64 // 0.0
var b bool    // false
var p *int    // nil
```

### Constants
```go
const Pi = 3.14159
const Greeting = "Hello"

// Multiple constants
const (
    StatusOK       = 200
    StatusNotFound = 404
    StatusError    = 500
)

// iota (auto-incrementing)
const (
    Sunday = iota  // 0
    Monday         // 1
    Tuesday        // 2
    Wednesday      // 3
)

const (
    _  = iota             // Skip 0
    KB = 1 << (10 * iota) // 1024
    MB                     // 1048576
    GB                     // 1073741824
)
```

## Data Types

### Basic Types
```go
// Boolean
bool

// String
string

// Integers
int   int8   int16   int32   int64
uint  uint8  uint16  uint32  uint64  uintptr

// Byte (alias for uint8)
byte

// Rune (alias for int32, represents Unicode code point)
rune

// Floating point
float32  float64

// Complex numbers
complex64  complex128
```

### Type Conversion
```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// String conversions
s := string(65)         // "A" (from ASCII)
s = strconv.Itoa(42)    // "42"
i, _ = strconv.Atoi("42") // 42

// Other conversions
f, _ = strconv.ParseFloat("3.14", 64)
b, _ = strconv.ParseBool("true")
```

## Operators

### Arithmetic Operators
```go
+   // Addition
-   // Subtraction
*   // Multiplication
/   // Division
%   // Modulus
++  // Increment (postfix only)
--  // Decrement (postfix only)
```

### Comparison Operators
```go
==  // Equal to
!=  // Not equal to
<   // Less than
>   // Greater than
<=  // Less than or equal to
>=  // Greater than or equal to
```

### Logical Operators
```go
&&  // Logical AND
||  // Logical OR
!   // Logical NOT
```

### Bitwise Operators
```go
&   // AND
|   // OR
^   // XOR
<<  // Left shift
>>  // Right shift
&^  // AND NOT
```

## Control Flow

### If-Else
```go
// Basic if
if x > 0 {
    fmt.Println("Positive")
}

// If-else
if x > 0 {
    fmt.Println("Positive")
} else {
    fmt.Println("Non-positive")
}

// If-else if-else
if x > 0 {
    fmt.Println("Positive")
} else if x < 0 {
    fmt.Println("Negative")
} else {
    fmt.Println("Zero")
}

// If with initialization
if err := doSomething(); err != nil {
    fmt.Println("Error:", err)
}

// Short circuit evaluation
if user != nil && user.IsActive {
    // user.IsActive only evaluated if user != nil
}
```

### Switch
```go
// Basic switch
switch day {
case "Monday":
    fmt.Println("Start of week")
case "Friday":
    fmt.Println("End of week")
default:
    fmt.Println("Middle of week")
}

// Switch with multiple cases
switch day {
case "Saturday", "Sunday":
    fmt.Println("Weekend")
default:
    fmt.Println("Weekday")
}

// Switch with initialization
switch err := doSomething(); err {
case nil:
    fmt.Println("Success")
default:
    fmt.Println("Error:", err)
}

// Switch without condition (like if-else)
switch {
case x < 0:
    fmt.Println("Negative")
case x > 0:
    fmt.Println("Positive")
default:
    fmt.Println("Zero")
}

// Type switch
switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
default:
    fmt.Printf("Unknown type\n")
}

// Fallthrough
switch x {
case 1:
    fmt.Println("One")
    fallthrough
case 2:
    fmt.Println("Two or less")
}
```

### Loops
```go
// Basic for loop
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-style loop
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// Infinite loop
for {
    // Break to exit
    if condition {
        break
    }
}

// Range over slice/array
nums := []int{1, 2, 3, 4, 5}
for index, value := range nums {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// Range (ignore index)
for _, value := range nums {
    fmt.Println(value)
}

// Range (index only)
for index := range nums {
    fmt.Println(index)
}

// Range over map
m := map[string]int{"a": 1, "b": 2}
for key, value := range m {
    fmt.Printf("%s: %d\n", key, value)
}

// Range over string (iterates over runes)
for index, char := range "Hello" {
    fmt.Printf("%d: %c\n", index, char)
}

// Break and continue
for i := 0; i < 10; i++ {
    if i == 5 {
        break    // Exit loop
    }
    if i%2 == 0 {
        continue // Skip to next iteration
    }
    fmt.Println(i)
}

// Labeled break/continue
outer:
for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        if i*j > 3 {
            break outer
        }
        fmt.Printf("%d * %d = %d\n", i, j, i*j)
    }
}
```

### Defer, Panic, Recover
```go
// Defer (executes after function returns, LIFO order)
func example() {
    defer fmt.Println("Third")
    defer fmt.Println("Second")
    fmt.Println("First")
    // Output: First, Second, Third
}

// Defer with cleanup
func readFile() {
    f, err := os.Open("file.txt")
    if err != nil {
        return
    }
    defer f.Close() // Ensures file is closed
    
    // Read file...
}

// Panic (stop execution, run defers, propagate up)
func causesPanic() {
    panic("Something went wrong!")
}

// Recover (catch panic, only works in defer)
func safeFunction() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from:", r)
        }
    }()
    
    // Code that might panic
    causesPanic()
    
    fmt.Println("This won't execute")
}
```

## Functions

### Basic Functions
```go
// No parameters, no return
func sayHello() {
    fmt.Println("Hello")
}

// With parameters
func greet(name string) {
    fmt.Println("Hello", name)
}

// Multiple parameters
func add(a int, b int) int {
    return a + b
}

// Same type parameters (shorthand)
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return // naked return
}

// Variadic functions
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}
// Call: sum(1, 2, 3, 4)

// Function as parameter
func apply(f func(int) int, value int) int {
    return f(value)
}

// Anonymous function
func() {
    fmt.Println("Anonymous")
}()

// Function variable
var multiply = func(a, b int) int {
    return a * b
}

// Closure
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}
// Usage:
// c := counter()
// c() // 1
// c() // 2
```

### Methods
```go
// Method (function with receiver)
type Rectangle struct {
    Width, Height float64
}

// Value receiver (receives copy)
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Pointer receiver (can modify original)
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Usage
rect := Rectangle{Width: 10, Height: 5}
area := rect.Area()      // 50
rect.Scale(2)            // Modifies rect
```

## Arrays & Slices

### Arrays
```go
// Fixed size, cannot be resized
var arr [5]int                    // [0 0 0 0 0]
arr := [5]int{1, 2, 3, 4, 5}     // [1 2 3 4 5]
arr := [...]int{1, 2, 3}         // Size inferred: [1 2 3]
arr := [5]int{0: 1, 4: 5}        // [1 0 0 0 5]

// Access elements
arr[0] = 10
val := arr[0]

// Length
len(arr) // 5

// Iterate
for i, v := range arr {
    fmt.Printf("Index %d: %d\n", i, v)
}
```

### Slices
```go
// Dynamic size, reference to underlying array
var s []int                      // nil slice
s := []int{1, 2, 3}             // [1 2 3]
s := make([]int, 5)             // [0 0 0 0 0] (length 5)
s := make([]int, 5, 10)         // length 5, capacity 10

// Slice from array/slice
arr := [5]int{1, 2, 3, 4, 5}
s := arr[1:4]    // [2 3 4] (index 1 to 3)
s := arr[:3]     // [1 2 3] (start to index 2)
s := arr[2:]     // [3 4 5] (index 2 to end)
s := arr[:]      // [1 2 3 4 5] (entire array)

// Append
s = append(s, 6)               // Add one element
s = append(s, 7, 8, 9)         // Add multiple
s = append(s, anotherSlice...) // Add another slice

// Copy
dest := make([]int, len(s))
copy(dest, s)

// Length and capacity
len(s)  // Number of elements
cap(s)  // Capacity of underlying array

// 2D slice
matrix := [][]int{
    {1, 2, 3},
    {4, 5, 6},
}

// Common operations
// Remove element at index i
s = append(s[:i], s[i+1:]...)

// Insert element at index i
s = append(s[:i], append([]int{newElement}, s[i:]...)...)

// Check if slice is empty
if len(s) == 0 { }
```

## Maps

### Basic Map Operations
```go
// Declaration
var m map[string]int                    // nil map
m := make(map[string]int)               // Empty map
m := map[string]int{"a": 1, "b": 2}    // Initialized map

// Set value
m["key"] = 42

// Get value
value := m["key"]

// Check if key exists
value, exists := m["key"]
if exists {
    fmt.Println("Found:", value)
}

// Delete key
delete(m, "key")

// Length
len(m)

// Iterate
for key, value := range m {
    fmt.Printf("%s: %d\n", key, value)
}

// Keys only
for key := range m {
    fmt.Println(key)
}

// Check if map is empty
if len(m) == 0 { }

// Map of maps
m := make(map[string]map[string]int)
m["outer"] = make(map[string]int)
m["outer"]["inner"] = 42
```

## Structs

### Basic Structs
```go
// Define struct
type Person struct {
    Name    string
    Age     int
    Email   string
}

// Create instance
p1 := Person{Name: "John", Age: 30, Email: "john@example.com"}
p2 := Person{"Jane", 25, "jane@example.com"} // Order matters
p3 := Person{Name: "Bob"}  // Other fields get zero values

// Anonymous struct
person := struct {
    Name string
    Age  int
}{
    Name: "John",
    Age:  30,
}

// Access fields
p1.Name = "John Doe"
age := p1.Age

// Pointer to struct
p := &Person{Name: "John"}
p.Name = "Jane"  // No need to dereference

// Embedded structs (composition)
type Address struct {
    Street string
    City   string
}

type Employee struct {
    Person   // Embedded
    Address  // Embedded
    Position string
}

e := Employee{
    Person:   Person{Name: "John", Age: 30},
    Address:  Address{Street: "123 Main", City: "NYC"},
    Position: "Developer",
}

// Access embedded fields
e.Name = "Jane"      // Direct access
e.Person.Name = "Jane" // Explicit access

// Struct tags (for JSON, etc.)
type User struct {
    ID       int    `json:"id"`
    Username string `json:"username"`
    Password string `json:"-"` // Ignored in JSON
}

// Anonymous fields
type Employee struct {
    string // Anonymous field
    int    // Anonymous field
}
```

### Struct Methods
```go
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Constructor pattern
func NewRectangle(width, height float64) *Rectangle {
    return &Rectangle{
        Width:  width,
        Height: height,
    }
}
```

## Interfaces

### Basic Interfaces
```go
// Define interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Implement interface (implicit)
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Use interface
func printShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f\n", s.Area())
    fmt.Printf("Perimeter: %.2f\n", s.Perimeter())
}

// Empty interface (accepts any type)
func printAnything(v interface{}) {
    fmt.Println(v)
}

// Type assertion
var i interface{} = "hello"
s := i.(string)  // Assert i is string
s, ok := i.(string) // Safe assertion

// Type switch
func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s\n", v)
    case bool:
        fmt.Printf("Boolean: %t\n", v)
    default:
        fmt.Printf("Unknown type\n")
    }
}

// Common interfaces
type Stringer interface {
    String() string  // For fmt.Println
}

type Error interface {
    Error() string   // For error handling
}

type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}
```

## Pointers

```go
// Pointer basics
var p *int      // Pointer to int
i := 42
p = &i          // Address of i
*p = 21         // Dereference and set value
fmt.Println(*p) // Dereference and read value

// Zero value of pointer is nil
var p *int
if p == nil {
    fmt.Println("Pointer is nil")
}

// new() allocates memory
p := new(int)  // Returns *int pointing to zero value
*p = 42

// Pointers to structs
type Person struct {
    Name string
}

p := &Person{Name: "John"}
p.Name = "Jane"  // Automatic dereferencing

// Pass by value vs pointer
func modifyValue(x int) {
    x = 100  // Doesn't affect original
}

func modifyPointer(x *int) {
    *x = 100  // Modifies original
}

i := 42
modifyValue(i)      // i is still 42
modifyPointer(&i)   // i is now 100
```

## Error Handling

```go
// errors package
import "errors"

// Create error
err := errors.New("something went wrong")
err := fmt.Errorf("error: %s", "details")

// Return error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Check error
result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
    return
}

// Custom error type
type MyError struct {
    Code    int
    Message string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("Error %d: %s", e.Code, e.Message)
}

// Error wrapping (Go 1.13+)
import "fmt"

err := doSomething()
if err != nil {
    return fmt.Errorf("failed to do something: %w", err)
}

// Unwrap error
import "errors"

var ErrNotFound = errors.New("not found")

err := findUser()
if errors.Is(err, ErrNotFound) {
    // Handle not found
}

// Type assertion for errors
var myErr *MyError
if errors.As(err, &myErr) {
    fmt.Println("Custom error code:", myErr.Code)
}

// Panic for unrecoverable errors
if criticalError {
    panic("unrecoverable error")
}
```

## Goroutines & Concurrency

### Goroutines
```go
// Start goroutine
go functionName()

go func() {
    fmt.Println("Anonymous goroutine")
}()

// Example
func printNumbers() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)
        time.Sleep(100 * time.Millisecond)
    }
}

func main() {
    go printNumbers()  // Runs concurrently
    printNumbers()     // Runs in main goroutine
}
```

### Channels
```go
// Create channel
ch := make(chan int)           // Unbuffered
ch := make(chan int, 10)       // Buffered (capacity 10)

// Send to channel
ch <- 42

// Receive from channel
value := <-ch

// Close channel
close(ch)

// Check if channel is closed
value, ok := <-ch
if !ok {
    fmt.Println("Channel closed")
}

// Range over channel
for value := range ch {
    fmt.Println(value)
}

// Select statement (multiplex channels)
select {
case msg := <-ch1:
    fmt.Println("Received from ch1:", msg)
case msg := <-ch2:
    fmt.Println("Received from ch2:", msg)
case ch3 <- 42:
    fmt.Println("Sent to ch3")
default:
    fmt.Println("No communication")
}

// Timeout with select
select {
case msg := <-ch:
    fmt.Println("Received:", msg)
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
}

// Worker pool pattern
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        results <- j * 2
    }
}

jobs := make(chan int, 100)
results := make(chan int, 100)

// Start workers
for w := 1; w <= 3; w++ {
    go worker(w, jobs, results)
}

// Send jobs
for j := 1; j <= 5; j++ {
    jobs <- j
}
close(jobs)

// Collect results
for a := 1; a <= 5; a++ {
    <-results
}
```

### Synchronization

```go
import "sync"

// WaitGroup
var wg sync.WaitGroup

for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        fmt.Println("Worker", id)
    }(i)
}

wg.Wait() // Wait for all goroutines

// Mutex
var mu sync.Mutex
var counter int

func increment() {
    mu.Lock()
    counter++
    mu.Unlock()
}

// RWMutex (multiple readers, single writer)
var rwmu sync.RWMutex
var data map[string]string

func read(key string) string {
    rwmu.RLock()
    defer rwmu.RUnlock()
    return data[key]
}

func write(key, value string) {
    rwmu.Lock()
    defer rwmu.Unlock()
    data[key] = value
}

// Once (execute only once)
var once sync.Once

func initialize() {
    once.Do(func() {
        fmt.Println("Initialized")
    })
}

// Atomic operations
import "sync/atomic"

var counter int64
atomic.AddInt64(&counter, 1)
value := atomic.LoadInt64(&counter)
atomic.StoreInt64(&counter, 0)
```

## Packages & Imports

### Package Structure
```go
// In file math/math.go
package math

// Exported (starts with capital letter)
func Add(a, b int) int {
    return a + b
}

// Unexported (starts with lowercase)
func subtract(a, b int) int {
    return a - b
}

// In main.go
package main

import "yourmodule/math"

func main() {
    result := math.Add(5, 3)
}
```

### Import Variations
```go
// Standard import
import "fmt"
import "os"

// Multiple imports
import (
    "fmt"
    "os"
    "strings"
)

// Alias import
import f "fmt"
f.Println("Hello")

// Dot import (not recommended)
import . "fmt"
Println("Hello")  // No package prefix needed

// Blank import (for side effects only)
import _ "github.com/lib/pq"
```

### Init Function
```go
package mypackage

func init() {
    // Runs automatically before main()
    // Used for initialization
    fmt.Println("Package initialized")
}
```

## File I/O

### Reading Files
```go
import (
    "io/ioutil"
    "os"
    "bufio"
)

// Read entire file
data, err := ioutil.ReadFile("file.txt")
if err != nil {
    panic(err)
}
fmt.Println(string(data))

// Read file line by line
file, err := os.Open("file.txt")
if err != nil {
    panic(err)
}
defer file.Close()

scanner := bufio.NewScanner(file)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}

// Read with buffer
file, _ := os.Open("file.txt")
defer file.Close()

reader := bufio.NewReader(file)
for {
    line, err := reader.ReadString('\n')
    if err != nil {
        break
    }
    fmt.Print(line)
}
```

### Writing Files
```go
// Write entire file
data := []byte("Hello, World!")
err := ioutil.WriteFile("file.txt", data, 0644)

// Write with os.Create
file, err := os.Create("file.txt")
if err != nil {
    panic(err)
}
defer file.Close()

file.WriteString("Hello, World!\n")

// Buffered writer
file, _ := os.Create("file.txt")
defer file.Close()

writer := bufio.NewWriter(file)
writer.WriteString("Hello, World!\n")
writer.Flush() // Important!

// Append to file
file, err := os.OpenFile("file.txt", 
    os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
if err != nil {
    panic(err)
}
defer file.Close()

file.WriteString("Appended text\n")
```

### File Operations
```go
// Check if file exists
if _, err := os.Stat("file.txt"); os.IsNotExist(err) {
    fmt.Println("File does not exist")
}

// Delete file
err := os.Remove("file.txt")

// Rename file
err := os.Rename("old.txt", "new.txt")

// Create directory
err := os.Mkdir("mydir", 0755)
err := os.MkdirAll("path/to/dir", 0755) // Create parent dirs

// Remove directory
err := os.Remove("mydir")
err := os.RemoveAll("mydir") // Recursive

// List directory
files, err := ioutil.ReadDir(".")
for _, file := range files {
    fmt.Println(file.Name(), file.IsDir())
}

// Walk directory tree
filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
    if err != nil {
        return err
    }
    fmt.Println(path)
    return nil
})
```

## JSON

### Encoding & Decoding
```go
import "encoding/json"

type Person struct {
    Name  string `json:"name"`
    Age   int    `json:"age"`
    Email string `json:"email,omitempty"` // Omit if empty
}

// Marshal (Go to JSON)
person := Person{Name: "John", Age: 30}
jsonData, err := json.Marshal(person)
if err != nil {
    panic(err)
}
fmt.Println(string(jsonData)) // {"name":"John","age":30}

// Marshal with indentation
jsonData, err := json.MarshalIndent(person, "", "  ")

// Unmarshal (JSON to Go)
jsonString := `{"name":"John","age":30}`
var person Person
err := json.Unmarshal([]byte(jsonString), &person)

// Unmarshal to map
var result map[string]interface{}
json.Unmarshal([]byte(jsonString), &result)

// Encode to writer
file, _ := os.Create("data.json")
defer file.Close()
encoder := json.NewEncoder(file)
encoder.Encode(person)

// Decode from reader
file, _ := os.Open("data.json")
defer file.Close()
decoder := json.NewDecoder(file)
var person Person
decoder.Decode(&person)
```

## Testing

### Basic Tests
```go
// In math.go
package math

func Add(a, b int) int {
    return a + b
}

// In math_test.go
package math

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
        name     string
        a, b     int
        expected int
    }{
        {"positive", 2, 3, 5},
        {"negative", -1, -1, -2},
        {"zero", 0, 0, 0},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("got %d, want %d", result, tt.expected)
            }
        })
    }
}

// Benchmark
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}

// Example (for documentation)
func ExampleAdd() {
    fmt.Println(Add(2, 3))
    // Output: 5
}

// Setup and teardown
func TestMain(m *testing.M) {
    // Setup
    fmt.Println("Setting up tests")
    
    // Run tests
    code := m.Run()
    
    // Teardown
    fmt.Println("Cleaning up")
    
    os.Exit(code)
}

// Run tests
go test                    # Run tests in current package
go test ./...              # Run all tests in module
go test -v                 # Verbose output
go test -run TestAdd       # Run specific test
go test -bench .           # Run benchmarks
go test -cover             # Show coverage
go test -coverprofile=c.out # Generate coverage profile
go tool cover -html=c.out   # View coverage in browser
```

## HTTP & Web

### HTTP Server
```go
import (
    "fmt"
    "net/http"
    "encoding/json"
)

// Basic server
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}

// JSON response
type Response struct {
    Message string `json:"message"`
    Status  int    `json:"status"`
}

func jsonHandler(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    response := Response{Message: "Success", Status: 200}
    json.NewEncoder(w).Encode(response)
}

// Handle different methods
func userHandler(w http.ResponseWriter, r *http.Request) {
    switch r.Method {
    case "GET":
        // Handle GET
        fmt.Fprintf(w, "GET request")
    case "POST":
        // Handle POST
        fmt.Fprintf(w, "POST request")
    case "PUT":
        // Handle PUT
        fmt.Fprintf(w, "PUT request")
    case "DELETE":
        // Handle DELETE
        fmt.Fprintf(w, "DELETE request")
    default:
        w.WriteHeader(http.StatusMethodNotAllowed)
    }
}

// Read request body
func postHandler(w http.ResponseWriter, r *http.Request) {
    if r.Method != "POST" {
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
        return
    }
    
    var data map[string]interface{}
    decoder := json.NewDecoder(r.Body)
    if err := decoder.Decode(&data); err != nil {
        http.Error(w, err.Error(), http.StatusBadRequest)
        return
    }
    
    fmt.Fprintf(w, "Received: %v", data)
}

// URL parameters
func paramHandler(w http.ResponseWriter, r *http.Request) {
    // Query parameters: /api?name=John&age=30
    name := r.URL.Query().Get("name")
    age := r.URL.Query().Get("age")
    fmt.Fprintf(w, "Name: %s, Age: %s", name, age)
}

// Middleware
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        fmt.Printf("%s %s\n", r.Method, r.URL.Path)
        next.ServeHTTP(w, r)
    })
}

// Use middleware
func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/", handler)
    
    loggedMux := loggingMiddleware(mux)
    http.ListenAndServe(":8080", loggedMux)
}

// Custom server with timeouts
server := &http.Server{
    Addr:         ":8080",
    Handler:      mux,
    ReadTimeout:  15 * time.Second,
    WriteTimeout: 15 * time.Second,
    IdleTimeout:  60 * time.Second,
}
server.ListenAndServe()

// Serve static files
http.Handle("/static/", http.StripPrefix("/static/", 
    http.FileServer(http.Dir("./static"))))

// Redirect
func redirectHandler(w http.ResponseWriter, r *http.Request) {
    http.Redirect(w, r, "/new-path", http.StatusMovedPermanently)
}
```

### HTTP Client
```go
import (
    "bytes"
    "io/ioutil"
    "net/http"
    "time"
)

// GET request
resp, err := http.Get("https://api.example.com/data")
if err != nil {
    panic(err)
}
defer resp.Body.Close()

body, err := ioutil.ReadAll(resp.Body)
fmt.Println(string(body))

// POST request with JSON
data := map[string]string{"name": "John", "email": "john@example.com"}
jsonData, _ := json.Marshal(data)

resp, err := http.Post("https://api.example.com/users",
    "application/json",
    bytes.NewBuffer(jsonData))

// Custom request
req, err := http.NewRequest("PUT", "https://api.example.com/users/1", 
    bytes.NewBuffer(jsonData))
if err != nil {
    panic(err)
}

req.Header.Set("Content-Type", "application/json")
req.Header.Set("Authorization", "Bearer token123")

client := &http.Client{Timeout: 10 * time.Second}
resp, err := client.Do(req)

// Custom client with timeout
client := &http.Client{
    Timeout: 30 * time.Second,
    Transport: &http.Transport{
        MaxIdleConns:       10,
        IdleConnTimeout:    30 * time.Second,
        DisableCompression: true,
    },
}

// Download file
resp, err := http.Get("https://example.com/file.pdf")
if err != nil {
    panic(err)
}
defer resp.Body.Close()

out, err := os.Create("file.pdf")
if err != nil {
    panic(err)
}
defer out.Close()

io.Copy(out, resp.Body)
```

## Context

```go
import "context"

// Background context (never cancelled)
ctx := context.Background()

// TODO context (placeholder)
ctx := context.TODO()

// With cancel
ctx, cancel := context.WithCancel(context.Background())
defer cancel() // Always call cancel

go func() {
    time.Sleep(2 * time.Second)
    cancel() // Cancel after 2 seconds
}()

select {
case <-ctx.Done():
    fmt.Println("Context cancelled:", ctx.Err())
}

// With timeout
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

// With deadline
deadline := time.Now().Add(5 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
defer cancel()

// With value (for request-scoped data)
type key string
const userKey key = "user"

ctx := context.WithValue(context.Background(), userKey, "john")
user := ctx.Value(userKey).(string)

// Use context in HTTP request
req, _ := http.NewRequestWithContext(ctx, "GET", 
    "https://api.example.com/data", nil)

resp, err := client.Do(req)

// Use context in database query
rows, err := db.QueryContext(ctx, "SELECT * FROM users")

// Context in function
func doWork(ctx context.Context) error {
    for {
        select {
        case <-ctx.Done():
            return ctx.Err()
        default:
            // Do work
            time.Sleep(100 * time.Millisecond)
        }
    }
}
```

## Database (SQL)

```go
import (
    "database/sql"
    _ "github.com/lib/pq" // PostgreSQL driver
)

// Connect to database
db, err := sql.Open("postgres", 
    "host=localhost port=5432 user=postgres password=secret dbname=mydb sslmode=disable")
if err != nil {
    panic(err)
}
defer db.Close()

// Test connection
err = db.Ping()
if err != nil {
    panic(err)
}

// Set connection pool settings
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(5)
db.SetConnMaxLifetime(5 * time.Minute)

// Query single row
var name string
var age int
err = db.QueryRow("SELECT name, age FROM users WHERE id = $1", 1).Scan(&name, &age)
if err != nil {
    if err == sql.ErrNoRows {
        fmt.Println("No rows found")
    } else {
        panic(err)
    }
}

// Query multiple rows
rows, err := db.Query("SELECT id, name, age FROM users WHERE age > $1", 18)
if err != nil {
    panic(err)
}
defer rows.Close()

for rows.Next() {
    var id int
    var name string
    var age int
    
    err = rows.Scan(&id, &name, &age)
    if err != nil {
        panic(err)
    }
    fmt.Printf("ID: %d, Name: %s, Age: %d\n", id, name, age)
}

// Check for errors after iteration
if err = rows.Err(); err != nil {
    panic(err)
}

// Insert
result, err := db.Exec("INSERT INTO users (name, age) VALUES ($1, $2)", 
    "John", 30)
if err != nil {
    panic(err)
}

// Get last insert ID (if supported)
id, err := result.LastInsertId()

// Get rows affected
rowsAffected, err := result.RowsAffected()

// Update
result, err := db.Exec("UPDATE users SET age = $1 WHERE id = $2", 31, 1)

// Delete
result, err := db.Exec("DELETE FROM users WHERE id = $1", 1)

// Prepared statements
stmt, err := db.Prepare("INSERT INTO users (name, age) VALUES ($1, $2)")
if err != nil {
    panic(err)
}
defer stmt.Close()

for i := 0; i < 10; i++ {
    _, err = stmt.Exec(fmt.Sprintf("User%d", i), 20+i)
    if err != nil {
        panic(err)
    }
}

// Transactions
tx, err := db.Begin()
if err != nil {
    panic(err)
}

_, err = tx.Exec("INSERT INTO users (name, age) VALUES ($1, $2)", "John", 30)
if err != nil {
    tx.Rollback()
    panic(err)
}

_, err = tx.Exec("UPDATE accounts SET balance = balance - 100 WHERE id = $1", 1)
if err != nil {
    tx.Rollback()
    panic(err)
}

err = tx.Commit()
if err != nil {
    panic(err)
}

// NULL handling
var name sql.NullString
var age sql.NullInt64

err = db.QueryRow("SELECT name, age FROM users WHERE id = $1", 1).Scan(&name, &age)

if name.Valid {
    fmt.Println("Name:", name.String)
} else {
    fmt.Println("Name is NULL")
}
```

## Command-Line Arguments

```go
import (
    "flag"
    "fmt"
    "os"
)

// Using flag package
var (
    name    = flag.String("name", "World", "Name to greet")
    age     = flag.Int("age", 0, "Your age")
    verbose = flag.Bool("verbose", false, "Verbose output")
)

func main() {
    flag.Parse()
    
    fmt.Printf("Hello, %s!\n", *name)
    fmt.Printf("Age: %d\n", *age)
    
    if *verbose {
        fmt.Println("Verbose mode enabled")
    }
    
    // Non-flag arguments
    args := flag.Args()
    fmt.Println("Other arguments:", args)
}
// Run: go run main.go -name=John -age=30 -verbose arg1 arg2

// Using os.Args directly
func main() {
    // os.Args[0] is program name
    // os.Args[1:] are arguments
    
    if len(os.Args) < 2 {
        fmt.Println("Usage: program <arg1> <arg2>")
        os.Exit(1)
    }
    
    fmt.Println("Arguments:", os.Args[1:])
}
```

## String Manipulation

```go
import "strings"

// Common string operations
strings.Contains("hello", "ll")           // true
strings.HasPrefix("hello", "he")          // true
strings.HasSuffix("hello", "lo")          // true
strings.Index("hello", "ll")              // 2
strings.Count("cheese", "e")              // 3
strings.Replace("hello", "l", "L", 2)     // "heLLo"
strings.ReplaceAll("hello", "l", "L")     // "heLLo"
strings.ToUpper("hello")                  // "HELLO"
strings.ToLower("HELLO")                  // "hello"
strings.Title("hello world")              // "Hello World"
strings.TrimSpace("  hello  ")            // "hello"
strings.Trim("!!!hello!!!", "!")          // "hello"
strings.TrimLeft("!!!hello!!!", "!")      // "hello!!!"
strings.TrimRight("!!!hello!!!", "!")     // "!!!hello"
strings.Split("a,b,c", ",")               // ["a", "b", "c"]
strings.Join([]string{"a", "b"}, ",")     // "a,b"
strings.Repeat("ha", 3)                   // "hahaha"

// String builder (efficient concatenation)
var sb strings.Builder
sb.WriteString("Hello")
sb.WriteString(" ")
sb.WriteString("World")
result := sb.String() // "Hello World"

// Format strings
s := fmt.Sprintf("Name: %s, Age: %d", "John", 30)
```

## Regular Expressions

```go
import "regexp"

// Compile pattern
re := regexp.MustCompile(`\d+`)  // One or more digits

// Match
matched := re.MatchString("abc123") // true

// Find
result := re.FindString("abc123def456")  // "123"

// Find all
results := re.FindAllString("abc123def456", -1)  // ["123", "456"]

// Replace
result := re.ReplaceAllString("abc123def456", "X")  // "abcXdefX"

// Capture groups
re := regexp.MustCompile(`(\d+)-(\d+)-(\d+)`)
matches := re.FindStringSubmatch("2024-01-15")
// matches[0] = "2024-01-15" (full match)
// matches[1] = "2024"
// matches[2] = "01"
// matches[3] = "15"

// Named capture groups
re := regexp.MustCompile(`(?P<year>\d+)-(?P<month>\d+)-(?P<day>\d+)`)
matches := re.FindStringSubmatch("2024-01-15")
names := re.SubexpNames()

result := make(map[string]string)
for i, name := range names {
    if i > 0 && name != "" {
        result[name] = matches[i]
    }
}
// result["year"] = "2024"
// result["month"] = "01"
// result["day"] = "15"
```

## Time & Date

```go
import "time"

// Current time
now := time.Now()

// Create specific time
t := time.Date(2024, time.January, 15, 10, 30, 0, 0, time.UTC)

// Parse time string
layout := "2006-01-02 15:04:05"
t, err := time.Parse(layout, "2024-01-15 10:30:00")

// Common layouts
time.RFC3339     // "2006-01-02T15:04:05Z07:00"
time.RFC822      // "02 Jan 06 15:04 MST"
time.Kitchen     // "3:04PM"

// Format time
formatted := now.Format("2006-01-02 15:04:05")
formatted := now.Format(time.RFC3339)

// Time components
year := now.Year()
month := now.Month()
day := now.Day()
hour := now.Hour()
minute := now.Minute()
second := now.Second()
weekday := now.Weekday()

// Time arithmetic
tomorrow := now.Add(24 * time.Hour)
yesterday := now.Add(-24 * time.Hour)
nextWeek := now.AddDate(0, 0, 7)  // years, months, days

// Duration
duration := 5 * time.Second
duration := 10 * time.Minute
duration := 2 * time.Hour

// Compare times
t1.Before(t2)
t1.After(t2)
t1.Equal(t2)

// Difference between times
diff := t2.Sub(t1)  // Returns duration
hours := diff.Hours()
minutes := diff.Minutes()

// Unix timestamp
timestamp := now.Unix()           // Seconds since epoch
millis := now.UnixMilli()         // Milliseconds
nanos := now.UnixNano()           // Nanoseconds

// From Unix timestamp
t := time.Unix(1234567890, 0)

// Sleep
time.Sleep(2 * time.Second)

// Timer (executes once)
timer := time.NewTimer(2 * time.Second)
<-timer.C
fmt.Println("Timer expired")

// Stop timer
timer.Stop()

// Ticker (repeats)
ticker := time.NewTicker(1 * time.Second)
defer ticker.Stop()

for t := range ticker.C {
    fmt.Println("Tick at", t)
}

// Timeout channel
timeout := time.After(5 * time.Second)
select {
case <-timeout:
    fmt.Println("Timeout!")
}

// Time zones
loc, _ := time.LoadLocation("America/New_York")
t := time.Now().In(loc)
```

## Reflection

```go
import "reflect"

// Get type
var x float64 = 3.4
t := reflect.TypeOf(x)
fmt.Println("Type:", t)  // float64

// Get value
v := reflect.ValueOf(x)
fmt.Println("Value:", v)              // 3.4
fmt.Println("Kind:", v.Kind())        // float64
fmt.Println("Type:", v.Type())        // float64
fmt.Println("Float:", v.Float())      // 3.4

// Check if settable
v := reflect.ValueOf(&x).Elem()
if v.CanSet() {
    v.SetFloat(7.1)
}

// Struct reflection
type Person struct {
    Name string `json:"name" validate:"required"`
    Age  int    `json:"age"`
}

p := Person{Name: "John", Age: 30}
t := reflect.TypeOf(p)
v := reflect.ValueOf(p)

// Iterate struct fields
for i := 0; i < t.NumField(); i++ {
    field := t.Field(i)
    value := v.Field(i)
    
    fmt.Printf("Field: %s, Type: %s, Value: %v\n",
        field.Name, field.Type, value)
    
    // Get struct tag
    tag := field.Tag.Get("json")
    fmt.Println("JSON tag:", tag)
}

// Call method via reflection
method := v.MethodByName("MethodName")
if method.IsValid() {
    result := method.Call([]reflect.Value{})
}
```

## Best Practices

### Project Structure
```
myproject/
├── cmd/                    # Main applications
│   └── myapp/
│       └── main.go
├── internal/               # Private application code
│   ├── handlers/
│   ├── models/
│   └── services/
├── pkg/                    # Public libraries
│   └── utils/
├── api/                    # API definitions (OpenAPI, protobuf)
├── web/                    # Static web assets
├── configs/                # Configuration files
├── scripts/                # Build/deploy scripts
├── test/                   # Additional test files
├── docs/                   # Documentation
├── go.mod                  # Module definition
├── go.sum                  # Dependency checksums
├── Makefile               # Build commands
└── README.md
```

### Common Idioms
```go
// Error handling
if err != nil {
    return err
}

// Defer for cleanup
f, err := os.Open("file.txt")
if err != nil {
    return err
}
defer f.Close()

// Empty struct for set
set := make(map[string]struct{})
set["item"] = struct{}{}
_, exists := set["item"]

// Check empty slice/map
if len(slice) == 0 { }
if len(m) == 0 { }

// Avoid nil pointer
if user != nil && user.Address != nil {
    fmt.Println(user.Address.Street)
}

// Use named results for documentation
func divide(a, b float64) (result float64, err error) {
    if b == 0 {
        err = errors.New("division by zero")
        return
    }
    result = a / b
    return
}

// Accept interfaces, return structs
func ProcessReader(r io.Reader) (*Result, error) {
    // ...
}
```

### Performance Tips
```go
// Preallocate slices when size is known
s := make([]int, 0, 100)

// Use sync.Pool for frequently allocated objects
var bufferPool = sync.Pool{
    New: func() interface{} {
        return new(bytes.Buffer)
    },
}

// Avoid allocations in loops
// Bad: creates new slice each iteration
for i := 0; i < n; i++ {
    s := make([]int, size)
}

// Good: reuse slice
s := make([]int, size)
for i := 0; i < n; i++ {
    // Clear and reuse s
}

// Use string builder for concatenation
var sb strings.Builder
for _, s := range strings {
    sb.WriteString(s)
}
result := sb.String()

// Range over slice copies value, use pointer if large
for i := range largeStructs {
    processStruct(&largeStructs[i])
}
```