---
title: C#
description: Core C# syntax for types, classes, collections, async, and modern features.
tags:
  - object-oriented
  - dotnet
---

## Variables and Types

```csharp
int age = 25;
double price = 19.99;
bool active = true;
string name = "Ada";           // reference type
var count = 5;                 // type inferred by compiler

int? nullable = null;          // nullable value type
string? maybeNull = null;      // nullable reference type
```

- Value types store their data directly; reference types store a reference to an object.
- Whether a value is stored on the stack, heap, or elsewhere depends on context and runtime implementation.
- `var` lets the compiler infer the type; use when the type is obvious.

## Control Flow

```csharp
int n = 10;

if (n > 0)
{
    // positive
}
else
{
    // non-positive
}

// switch expression
string label = n switch
{
    > 0 => "positive",
    < 0 => "negative",
    _   => "zero",
};

for (int i = 0; i < n; i++)
{
    // iterate 0..9
}

foreach (int item in new[] { 1, 2, 3 })
{
    // item is read-only
}
```

## Methods

```csharp
int Add(int a, int b) => a + b;

// out parameters
bool parsed = int.TryParse("42", out int result); // result = 42 on success

// out parameters in custom methods
bool TryParse(string input, out int result)
{
    if (int.TryParse(input, out result))
    {
        return true;
    }
    result = 0;
    return false;
}

// default parameter values
void Log(string message, bool verbose = false) { }
```

- Use `=>` for single-expression methods.
- Return `out` parameters via the return type when practical.
- Default parameters must come after required parameters.

## Classes and Properties

```csharp
class User
{
    public string Name { get; set; }              // auto-property
    public int Age { get; init; }                 // init-only (C# 9+)
    private string? _email;

    public User(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public string? Email
    {
        get => _email;
        set => _email = value?.Trim();
    }
}

// Top-level access modifiers
//   public    — accessible everywhere
//   internal  — accessible within the assembly (default for classes)
//   protected — accessible within the class and derived classes
//   private   — accessible only within the class (default for members)
```

- `init` setters allow assignment only during object initialization.
- Auto-properties generate a hidden backing field; use them when no extra logic is needed.

## Collections

```csharp
using System.Collections.Generic;

List<string> names = new() { "Ada", "Grace" };
names.Add("Linus");

Dictionary<string, int> ages = new()
{
    ["Ada"] = 36,
    ["Grace"] = 85,
};

foreach (string name in names)
{
    Console.WriteLine(name);
}

int count = names.Count;  // List<T> property
```

- `List<T>` is a dynamic array; use `Add`, `Remove`, and indexer `[]`.
- `Dictionary<TKey, TValue>` maps keys to values; use `TryGetValue` for safe lookup.
- Prefer `var` when the right-hand side makes the type obvious.

## Async and Tasks

```csharp
using System.Net.Http;

async Task<string> FetchAsync(string url)
{
    using HttpClient client = new();
    return await client.GetStringAsync(url);
}

// Call from an async context
string html = await FetchAsync("https://example.com");
```

- Methods returning `Task` or `Task<T>` can use `await`.
- `using` declarations (no braces) dispose resources at the end of scope.
- Never block on `Task` with `.Result` or `.Wait()` — use `await`.

## Common Modern Features

```csharp
// Records — immutable reference types with value equality
public record Person(string Name, int Age);

var ada = new Person("Ada", 36);
var older = ada with { Age = 37 };  // non-destructive mutation

// Pattern matching
string Classify(int temp) => temp switch
{
    < 0   => "freezing",
    <= 15 => "cold",
    <= 25 => "comfortable",
    _     => "hot",
};

// Null-conditional and null-coalescing
string? name = "Ada";
int? length = name?.Length;          // null if name is null
int safeLen = name?.Length ?? 0;     // fallback if null
```

- Records generate `Equals`, `GetHashCode`, `ToString`, and deconstruct automatically.
- `with` expressions create a copy with specified properties changed.
- `?.` short-circuits to `null` if the left side is null.
- `??` returns the right-hand side when the left is null.

## References

- [C# Documentation](https://learn.microsoft.com/en-us/dotnet/csharp/)
- [C# Language Reference](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/)
- [What's New in C#](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/)
