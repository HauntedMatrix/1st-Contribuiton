---
title: Lua
description: Core Lua syntax for variables, tables, functions, closures, and modules.
tags:
  - scripting
  - embedded
  - game-dev
---

## Variables and Types

```lua
local name = "Ada"      -- string
local count = 10        -- number
local active = true     -- boolean
local nillable = nil    -- nil (absence of value)

-- Basic types: nil, boolean, number, string, function, table, thread, userdata
local pi = 3.14
local msg = 'single quoted too'
```

- `local` makes a variable local to its scope; omit it to declare a global.
- Reading an undeclared global or a missing table field yields `nil`; a `local` exists only from its declaration to the end of its block.
- `nil` and `false` are the only "falsy" values; everything else (`0`, `""`, `{}`) is truthy.

## Tables

```lua
local t = {}                 -- empty table
t.name = "Ada"               -- string key
t[1] = "first"               -- numeric index (Lua arrays start at 1)
print(t.name)                -- "Ada"
print(t[1])                  -- "first"
print(t.missing)             -- nil

-- Table constructor
local person = { name = "Ada", age = 36 }
local list = { "a", "b", "c" }   -- list[1]="a", list[2]="b"

-- Mixed keys
local mixed = { weekday = "mon", 10, 20 }
```

- A table is Lua's primary composite data structure; it can model arrays, maps, records, and object-like structures rather than being strictly an array or object.
- Array usage conventionally starts at index 1, but tables can use arbitrary key types.
- Missing keys evaluate to `nil`.

## Control Flow

```lua
local n = 10

if n > 0 then
    -- positive
elseif n < 0 then
    -- negative
else
    -- zero
end

local i = 1
while i <= 5 do
    i = i + 1
end

for j = 1, 5 do          -- 1 to 5 inclusive
    print(j)
end

for j = 5, 1, -1 do      -- 5 to 1 step -1
    print(j)
end

for k, v in pairs(t) do
    -- iterate all keys/values (unordered)
end

for index, value in ipairs(list) do
    -- iterate array portion in order
end
```

- `if`/`while`/`for` terminate with `end`.
- `pairs` iterates all keys; `ipairs` iterates sequential integer keys in order.

## Functions

```lua
local function add(a, b)
    return a + b
end

-- Multiple returns
local function minmax(...)
    return math.min(...), math.max(...)
end

local lo, hi = minmax(5, 2, 9)

-- Anonymous functions
local square = function(x) return x * x end

-- Variadic
local function sum_all(...)
    local total = 0
    for _, v in ipairs({ ... }) do
        total = total + v
    end
    return total
end
```

- Functions are first-class values and can be assigned to variables.
- Multiple values can be returned and assigned.
- `...` captures variadic arguments.

## Closures

```lua
local function make_counter(step)
    local localCount = 0
    return function()
        localCount = localCount + step
        return localCount
    end
end

local counter = make_counter(2)
print(counter())   -- 2
print(counter())   -- 4
```

- A closure captures variables from its enclosing scope.
- Each call to `make_counter` creates an independent counter.

## Metatables

```lua
local vec = { x = 1, y = 2 }

local mt = {
    __add = function(a, b)
        return { x = a.x + b.x, y = a.y + b.y }
    end,
}

setmetatable(vec, mt)
local sum = vec + vec
print(sum.x, sum.y)   -- 2 4
```

- A metatable customizes table behavior (operators, indexing, `__call`, etc.).
- `setmetatable`/`getmetatable` manage the metatable.

## Modules

```lua
-- mathutil.lua
local M = {}

function M.add(a, b)
    return a + b
end

function M.double(x)
    return x * 2
end

return M

-- usage elsewhere
local mathutil = require("mathutil")
print(mathutil.add(2, 3))    -- 5
```

- A module is a table returned from a file; `require` loads and caches it.
- Name the file to match the module (e.g. `mathutil.lua` required as `mathutil`).

## Standard Library

```lua
-- Strings
local s = "Hello, World"
string.upper(s)              -- "HELLO, WORLD"
#s                           -- 12 (length in bytes)
string.sub(s, 1, 5)          -- "Hello"
string.format("%d", 42)      -- "42"

-- Tables
local t = { 3, 1, 2 }
table.sort(t)                -- { 1, 2, 3 }
table.concat({ "a", "b" }, "-")   -- "a-b"

-- Math
math.floor(3.7)              -- 3
math.max(1, 5, 3)            -- 5
```

- The standard library (`string`, `table`, `math`, etc.) provides common utilities.
- `#s` is the byte length of a string (careful with multibyte UTF-8). On tables `#t` returns the length of a sequence — i.e. it is only well-defined when keys are a contiguous `1..n` run; it is not a reliable length for tables with "holes" in the integer keys.

## Idiomatic Modern Patterns

```lua
-- Guard against nil with a default
local options = {}                       -- config may be empty
local value = options and options.key or "default"   -- "default" (key is nil)

-- Basic module pattern with local aliases
local M = {}
local scratch = {}

-- Object-style with `self`
local Account = {}
Account.__index = Account

function Account.new(balance)
    return setmetatable({ balance = balance }, Account)
end

function Account:deposit(amount)
    self.balance = self.balance + amount
    return self.balance
end

local acc = Account.new(100)
print(acc:deposit(50))   -- 150
```

- `a and b or c` is a common idiom for a fallback value (works only when `b` is truthy).
- The `:` syntax passes the receiver as `self`.
- Metatables enable class-like object orientation.

## References

- [Lua 5.4 Reference Manual](https://www.lua.org/manual/5.4/)
- [Programming in Lua](https://www.lua.org/pil/)