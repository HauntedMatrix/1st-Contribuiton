---
title: Elixir
description: Core Elixir syntax for immutability, pattern matching, lists, maps, pipes, and processes.
tags:
  - functional
  - backend
  - concurrency
---

## Values and Immutability

```elixir
name = "Ada"            # string
age = 36                # integer
height = 1.72           # float
active = true           # boolean
nothing = nil           # absence of value

# Reassignment rebinds a variable; values are immutable
first = 1
first = first + 1       # now 2
```

- Values are immutable; `=` is a pattern match, not assignment.
- Rebinding in a case is a new, shadowed variable.
- The only falsy values are `false` and `nil`.

## Pattern Matching

```elixir
# The match operator = destructures
{name, age} = {"Ada", 36}
[a, b, c] = [1, 2, 3]
%{user: u} = %{user: "Ada", role: "admin"}
```

- `=` matches the right side against the left pattern.
- If the pattern does not fit, it raises a `MatchError`.

## Lists and Tuples

```elixir
list = [1, 2, 3, 4]

hd(list)          # 1
tl(list)          # [2, 3, 4]
length(list)      # 4
Enum.map(list, fn x -> x * 2 end)    # [2, 4, 6, 8]
Enum.filter(list, &rem(&1, 2) == 0) # [2, 4]
list ++ [5, 6]    # [1,2,3,4,5,6]

# prepend (fast)
[0 | list]        # [0, 1, 2, 3, 4]

# tuple for fixed-sized values
pair = {:ok, 42}
elem(pair, 1)     # 42
```

- Lists are linked and efficient to prepend; `++` concatenates.
- Tuples are for fixed-size structures; use `List`/`Enum` for sequences.
- `&fun/1` captures an anonymous function; `&rem(&1, 2)` is a shorthand.

## Maps

```elixir
profile = %{name: "Ada", age: 36}

profile.name            # "Ada"
profile[:name]          # "Ada"
Map.get(profile, :age)  # 36
Map.put(profile, :role, "dev")   # new map with added key
Map.fetch(profile, :age)         # {:ok, 36}

# Pattern match to update
%{age: years} = profile   # years is 36
```

- Maps are key-value structures; atom keys allow dot access.
- Updates return a new map; the original is unchanged.
- `Map.fetch` returns `{:ok, value}` or `:error`.

## Control Flow

```elixir
n = 10

if n > 0 do
  "positive"
else
  "non-positive"
end

# case
case n do
  1 -> "one"
  x when x > 5 -> "large"
  _ -> "other"
end

# cond
cond do
  n < 0 -> "negative"
  n > 0 -> "positive"
  true  -> "zero"
end
```

- `if`, `case`, and `cond` all return a value.
- `case` uses pattern matching with optional guards (`when`).
- `_` is the wildcard fallback.

## Pipes

```elixir
result =
  [1, 2, 3, 4]
  |> Enum.map(&(&1 * 2))
  |> Enum.filter(&rem(&1, 4) == 0)
  |> Enum.sum()
  # -> 12
```

- The pipe `|>` passes a value as the first argument of the next function.
- Pipelines compose transformations left to right.

## Functions

```elixir
# Named functions live in modules
defmodule Math do
  def add(a, b) do
    a + b
  end

  def double(x), do: x * 2

  def greet(name, punct \\ "!") do
    "Hello, #{name}#{punct}"
  end
end

Math.add(2, 3)          # 5
Math.double(4)          # 8
Math.greet("Ada")       # "Hello, Ada!"

# Anonymous functions
square = fn x -> x * x end
square.(4)              # 16
```

- Named functions are grouped in modules (`defmodule`).
- `def` defines a function; `defp` defines a private one.
- Default arguments use `\\`.
- Call anonymous functions with `.` (`square.(4)`).

## Recursion and Loops

```elixir
# Iteration via Enum
Enum.each([1, 2, 3], fn x -> IO.puts(x) end)

# Recursion in modules
defmodule Counter do
  def countdown(n) when n <= 0 do
    :done
  end

  def countdown(n) do
    countdown(n - 1)
  end
end

Counter.countdown(3)   # :done
```

- There is no `for` loop; recursion and `Enum` iterate.
- Guards (`when n <= 0`) select the terminating clause.

## Modules and Structs

```elixir
defmodule User do
  defstruct name: "", role: "guest"

  def greeting(%User{name: name}) do
    "Hi, #{name}"
  end
end

user = %User{name: "Ada"}
user.name              # "Ada"
User.greeting(user)    # "Hi, Ada"
```

- `defstruct` defines a struct (a typed map with defaults).
- Pattern matching on structs is idiomatic.
- `%User{}` creates an instance; fields are accessed with `.`.

## Processes and Concurrency

```elixir
# Spawn a lightweight process
pid = spawn(fn -> :ok end)

# Send and receive messages
parent = self()
spawn(fn -> send(parent, {:hello, self()}) end)

receive do
  {:hello, from} -> IO.puts("got hello from #{inspect(from)}")
after
  1000 -> IO.puts("timeout")
end
```

- Processes are isolated, lightweight units running concurrently.
- `send`/`receive` pass messages between processes.
- `after` in `receive` provides a timeout.

## References

- [Elixir Documentation](https://hexdocs.pm/elixir/)
- [Elixir Getting Started](https://elixir-lang.org/getting-started/introduction.html)
- [HexDocs](https://hexdocs.pm/)