---
title: Julia
description: Core Julia syntax for functions, multiple dispatch, arrays, types, broadcasting, and packages.
tags:
  - scientific
  - data-science
  - high-performance
---

## Variables and Types

```julia
name = "Ada"          # string
age = 36              # integer
height = 1.72         # float
active = true         # Bool

# Types may be annotated with :: (used for method dispatch and conversions)
x::Int64 = 42
pi_approx::Float64 = 3.14

# Type hierarchy
typeof(42)        # Int64
supertype(Int64)  # Signed
```

- Julia is dynamically typed but supports optional annotations.
- On 64-bit platforms integer literals default to `Int64` and float literals to `Float64`.
- Julia is 1-indexed.

## Arrays

```julia
v = [1, 2, 3]              # Vector{Int64}
v[1]                       # 1 (indexing starts at 1)
v[end]                     # last element
v[2:3]                     # sub-vector -> [2, 3]
push!(v, 4)                # append in place -> [1, 2, 3, 4]
length(v)                  # 4
sum(v)                     # 10

# Matrices
m = [1 2; 3 4]             # 2x2 matrix
m[2, 1]                    # 3

# Ranges
collect(1:5)               # [1, 2, 3, 4, 5]
```

- `!` suffix conventionally marks a mutating function.
- `end` refers to the last index.

## Broadcasting

```julia
x = [1, 2, 3]
x .* 2              # [2, 4, 6]  (element-wise via dot)
x .+ 10             # [11, 12, 13]
sin.(x)             # element-wise function application

# Combining with broadcasting
a = [1, 2, 3]
b = [10, 20, 30]
a .+ b              # [11, 22, 33]

# In-place broadcast
x .= x .* 2
```

- The dot (`.`) broadcasts a function or operator element-wise over arrays.
- `.=` overwrites an array in place.

## Control Flow

```julia
n = 10

if n > 0
    println("positive")
elseif n < 0
    println("negative")
else
    println("zero")
end

for i in 1:5
    println(i)
end

# while
i = 0
while i < 5
    global i
    i += 1
end

# Comprehensions
squares = [x^2 for x in 1:5]     # [1, 4, 9, 16, 25]
```

- `if`/`for`/`while` end with `end`.
- At top-level/global scope, a loop that wants to modify a global must declare `global`; the same loop inside a function shares the function's scope and needs no such declaration.

## Functions

```julia
function add(a, b)
    a + b
end

# One-liner
square(x) = x * x

# Return type annotation
function twice(x::Int)::Int
    2x
end

println(add(2, 3))       # 5
println(square(4))       # 16

# Anonymous functions
map(x -> x * 2, [1, 2, 3])       # [2, 4, 6]
```

- The last expression is the return value.
- `x -> expr` is an anonymous function; `map` applies it.
- Type annotations on arguments participate in dispatch.

## Multiple Dispatch

```julia
area(length::Number, width::Number) = length * width
area(radius::Number) = 3.14159 * radius^2

println(area(2, 3))       # 6   (length, width)
println(area(2))          # 12.56636  (radius)
```

- Functions specialize on the types of all arguments — this is multiple dispatch.
- Defining the same function name for different argument types adds a method.

## Structs

```julia
struct Point
    x::Float64
    y::Float64
end

mutable struct Counter
    value::Int
end

p = Point(1.0, 2.0)
p.x            # 1.0

c = Counter(0)
c.value = 5    # allowed because Counter is mutable
```

- `struct` defines an immutable type; `mutable struct` allows field mutation.
- Structs allocate efficiently and dispatch on their types.

## Types and Parameters

```julia
abstract type Shape end
struct Circle <: Shape
    radius::Float64
end
struct Square <: Shape
    side::Float64
end

area(s::Circle) = 3.14159 * s.radius^2
area(s::Square) = s.side^2

area(Circle(2.0))    # ~12.566
area(Square(4.0))    # 16.0

# Parametric types
struct Box{T}
    content::T
end
Box(1)     # Box{Int64}(1)
```

- `abstract type` declares a family; `struct ... <: Shape` subtypes it.
- Methods dispatch on the concrete type.
- Parametric types (`struct Foo{T}`) carry a type parameter.

## Packages

```julia
# Activate the default environment
import Pkg
Pkg.add("DataFrames")    # install once
using DataFrames         # load in each session

# Or add to a specific environment
Pkg.activate(".")        # use the current directory's Project.toml
Pkg.add("Plots")
```

- Install packages with `Pkg.add`; load them with `using`.
- Each project uses a `Project.toml` to track dependencies.

## Idiomatic Modern Patterns

```julia
# Vectorized / broadcast operations
data = [1.0, 2.0, 3.0]
normalized = (data .- 2.0) ./ 1.0      # [-1.0, 0.0, 1.0]

# String interpolation
n = 42
msg = "value: $n"

# FizzBuzz via comprehension (illustrative)
fizzbuzz = [i % 15 == 0 ? "FizzBuzz" :
            i % 3 == 0 ? "Fizz" :
            i % 5 == 0 ? "Buzz" : i for i in 1:15]

# Find values satisfying a condition
findall(x -> x > 2, [1, 3, 2, 5])      # [2, 4] (indices)
```

- Broadcasting with `.` is the idiomatic way to apply operations to arrays.
- Ternary conditionals and comprehensions compose data easily.

## References

- [Julia Documentation](https://docs.julialang.org/)
- [Julia Manual](https://docs.julialang.org/en/v1/manual/)
- [Pkg.jl Documentation](https://pkgdocs.julialang.org/)