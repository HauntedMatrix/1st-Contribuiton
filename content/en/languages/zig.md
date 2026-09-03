---
title: Zig
description: Core Zig syntax for variables, optionals, error unions, structs, enums, and pointers.
tags:
  - systems
  - low-level
  - performance
---

## Variables

```zig
const x: i32 = 5;        // immutable (compile-time known if literal)
var y: i32 = 10;         // mutable
y += 1;

// Comptime constant
const MAX = 100;         // comptime_int inferred

// Types: i8..i128, u8..u128, f16/f32/f64, bool, usize, isize
const n: u32 = 42;
const f: f64 = 3.14;
const flag: bool = true;
```

- `const` values are immutable; `var` are mutable.
- Integer-size types are explicit (`u8`, `i32`, `usize`).
- `const` at scope can be a runtime value; `comptime` is evaluated at compile time.

## Optionals

```zig
var maybe: ?i32 = null;   // optional: may hold a value or null
maybe = 42;

if (maybe) |value| {
    // value is i32 here (unwrapped)
}

const fallback: i32 = maybe orelse 0;
```

- `?T` is an optional type that is either a `T` or `null`.
- `if (opt) |v| { }` unwraps within the block.
- `orelse` provides a default when the optional is null.

## Error Unions

```zig
const MyError = error{NotFound, Invalid};

fn find(id: i32) MyError!i32 {
    if (id < 0) return error.Invalid;
    return id;
}

const result = find(5) catch 0;        // 5 (catch supplies a fallback)

// try propagates the error to the calling function, which must also
// return (or be able to return) the error union
fn lookup(id: i32) MyError!i32 {
    const value = try find(id);   // propagates MyError on failure
    return value + 1;
}

// if/else on errors
if (find(-1)) |ok| {
    // ok is i32
} else |err| {
    // handle err
}
```

- `E!T` is an error union: either a value of type `T` or an error.
- `catch` handles the error inline; `try` propagates it to the calling function, so it is only valid inside a function that can return the error.
- Use `if (expr) |value| else |err|` to handle both branches.

## Structs

```zig
const Point = struct {
    x: f64,
    y: f64,
};

const origin = Point{ .x = 0, .y = 0 };

// Methods via struct definition
const Point2 = struct {
    x: f64,
    y: f64,

    fn distance(self: *const Point2) f64 {
        return @sqrt(self.x * self.x + self.y * self.y);
    }
};

var p = Point2{ .x = 3, .y = 4 };
const d = p.distance();   // 5
```

- A `struct` groups named fields.
- Fields are initialized with `.{ field = value }`.
- Functions defined in a struct can take `self`.

## Enums

```zig
const Direction = enum { north, south, east, west };

const dir = Direction.north;

// Switch on an enum
const label = switch (dir) {
    .north => "N",
    .south => "S",
    .east => "E",
    .west => "W",
};
```

- Enums are compile-time sets of named values.
- `switch` must be exhaustive for enums (or include `else`).

## Pointers

```zig
var val: i32 = 5;
const ptr: *i32 = &val;   // single-item pointer
ptr.* = 10;               // dereference to assign

// arrays and slices
const arr = [_]i32{ 1, 2, 3 };
const slice: []const i32 = arr[0..2];   // slice referencing arr[0..2]

// many-item pointer (length not known at compile time)
var elems = [_]i32{ 1, 2, 3 };
const ptr: *[3]i32 = &elems;   // pointer to the whole array
const many: [*]i32 = ptr;      // many-item pointer, no length bound
```

- `*T` is a single-item pointer; `[*]T` is a many-item pointer.
- `[]const u8` / `[]T` denotes a slice — a pointer plus a length.
- Dereference with `.*`.

## Functions

```zig
fn add(a: i32, b: i32) i32 {
    return a + b;
}

fn greet(name: []const u8) []const u8 {
    return "Hi";
}

// Functions are values passed with `&`
fn apply(f: *const fn (i32) i32, n: i32) i32 {
    return f(n);
}
```

- Return types are explicit; the last expression is not implicitly returned (`return` required).
- Slices (`[]const u8`) are the idiomatic string type.

## Control Flow

```zig
const n: i32 = 10;

if (n > 0) {
    // positive
} else {
    // non-positive
}

var i: usize = 0;
while (i < 5) : (i += 1) {
    // body
}

for ([_]i32{ 1, 2, 3 }) |v| {
    // v is each element
}

// switch
const grade = switch (n) {
    1...5 => "low",
    6...10 => "high",
    else => "other",
};
```

- `while` and `for` end with `)` and `}`.
- `switch` supports ranges (`1...5`) and must be exhaustive.
- `for` iterates over arrays and slices.

## Comptime

```zig
fn square(comptime n: i32) i32 {
    return n * n;
}

const k = comptime 5 * 5;   // 25, evaluated at compile time
```

- `comptime` evaluates expressions at compile time.
- `fn` parameters marked `comptime` are resolved at compile time.

## Idiomatic Modern Patterns

```zig
// Generic functions (comptime types)
fn max(comptime T: type, a: T, b: T) T {
    return if (a > b) a else b;
}

const g = max(i32, 3, 5);       // 5
const g64 = max(f64, 2.5, 1.5); // 2.5

// comptime blocks run at compile time
const fixed = comptime blk: {
    break :blk 21 * 2;          // 42, computed before runtime
};
```

- Generics are expressed with `comptime T: type` parameters.
- Zig resolves such functions at compile time for each concrete type.

## References

- [Zig Language Reference](https://ziglang.org/documentation/master/)
- [Zig Standard Library](https://ziglang.org/documentation/master/std/)
- [Learn Zig](https://ziglearn.org/)