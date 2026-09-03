---
title: Go
description: Core Go syntax for variables, control flow, concurrency, and idiomatic patterns.
tags:
  - concurrency
  - systems
  - backend
---

## Variables and Types

```go
var x int
var name string = "Ada"
count := 42          // short declaration
const Pi = 3.14159

// Basic types: bool, string, int, int8/16/32/64, uint, float32/float64,
// complex64/complex128, byte (uint8), rune (int32)

// Zero values: 0 for numerics, "" for string, false for bool, nil for
// pointers/slices/maps/channels/interfaces/functions
```

## Control Flow

```go
import "fmt"

n := 10

if n > 0 {
    // positive
} else {
    // non-positive
}

// if can have an init statement
if v := n * 2; v > 10 {
    fmt.Println(v)
}

switch n {
case 1:
    fmt.Println("one")
case 2, 3:
    fmt.Println("two or three")
default:
    fmt.Println("other")
}

// switch without expression acts like if-else chain
switch {
case n > 10:
    fmt.Println("big")
}

for i := 0; i < 5; i++ {
    // classic for loop
}

for n > 0 {
    n--
}

for {
    // infinite loop
}

nums := []int{1, 2, 3}
for i, v := range nums {
    fmt.Println(i, v)
}

m := map[string]int{"a": 1, "b": 2}
for k, v := range m {
    fmt.Println(k, v)
}
```

## Functions

```go
import "fmt"

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
    return // bare return uses named values
}

// Variadic parameters
func sum(nums ...int) int {
    total := 0
    for _, v := range nums {
        total += v
    }
    return total
}
```

## Structs and Methods

```go
type Point struct {
    X, Y int
}

func (p Point) Add(q Point) Point {
    return Point{X: p.X + q.X, Y: p.Y + q.Y}
}

// Pointer receiver — modifies original value
func (p *Point) Scale(factor int) {
    p.X *= factor
    p.Y *= factor
}
```

## Interfaces

```go
import "fmt"

type Stringer interface {
    String() string
}

// Implicit satisfaction: any type with String() implements Stringer.
type Celsius float64

func (c Celsius) String() string {
    return fmt.Sprintf("%g°C", float64(c))
}

func printValue(s Stringer) {
    fmt.Println(s.String())
}
```

## Slices and Maps

```go
// Slice literal
nums := []int{1, 2, 3}

// Append
nums = append(nums, 4, 5)

// Make with length and capacity
buf := make([]byte, 0, 1024)

// Map creation
m := map[string]int{
    "one": 1,
    "two": 2,
}

// Comma-ok lookup
_, ok := m["three"]
if !ok {
    // key not found
}

// Delete
delete(m, "one")
```

## Error Handling

```go
import (
    "errors"
    "fmt"
)

var ErrNotFound = errors.New("not found")

func find(id int) (string, error) {
    if id == 0 {
        return "", ErrNotFound
    }
    return "item", nil
}

// Idiomatic: check, then inspect
_, err := find(0)
if err != nil {
    // handle error
}

if errors.Is(err, ErrNotFound) {
    // sentinel comparison
}

// Wrapping with context
func load(id int) error {
    _, err := find(id)
    if err != nil {
        return fmt.Errorf("load: %w", err)
    }
    return nil
}
```

## Goroutines and Channels

```go
import "fmt"

func worker(id int, ch chan<- string) {
    ch <- fmt.Sprintf("done: %d", id)
}

func main() {
    ch := make(chan string)
    go worker(1, ch)
    go worker(2, ch)

    fmt.Println(<-ch) // receive from channel
    fmt.Println(<-ch)
}

// Buffered channel
func buffered() {
    buf := make(chan int, 5)
    buf <- 1 // non-blocking when buffer not full
    <-buf    // receive from channel
}
```

## Common Go Patterns

```go
import (
    "context"
    "os"
    "time"
)

// defer — runs when surrounding function returns (LIFO order)
func readFile(path string) error {
    f, err := os.Open(path)
    if err != nil {
        return err
    }
    defer f.Close()
    // ... read from f
    return nil
}

// Pointers vs values
// - Structs: passed by value (copy). Use *T when you need mutation.
// - Slices, maps, channels, and interfaces are already reference-like.
func scale(p *Point, factor int) {
    p.X *= factor
    p.Y *= factor
}

// Pointer vs value receiver:
// - Value receiver: receives a copy; modifications affect only that copy.
// - Pointer receiver: mutates receiver, avoids copying large structs.

// context — cancel and deadlines
func fetch(ctx context.Context) error {
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    // pass ctx to blocking calls
    return nil
}
```

## References

- [Go Documentation](https://go.dev/doc/)
- [Go Package Reference](https://pkg.go.dev/)
- [Go Language Specification](https://go.dev/ref/spec)
- [Effective Go](https://go.dev/doc/effective_go)
- [Go Blog](https://go.dev/blog/)
