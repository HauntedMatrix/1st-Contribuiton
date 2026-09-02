---
title: C++
description: Core C++ syntax for classes, references, standard containers, and modern features.
tags:
  - object-oriented
  - systems
---

## Variables and Types

```cpp
#include <string>

int age = 25;
double price = 19.99;
bool active = true;
auto count = 5u;        // deduced unsigned int
std::string name = "Ada";  // safe string type
```

## References vs Pointers

```cpp
int value = 42;
int& ref = value;  // reference: alias to value
ref = 43;          // modifies value

int* ptr = &value; // pointer: holds an address
*ptr = 44;         // dereference to modify value

int* maybe = nullptr; // null pointer
```

- Prefer a **reference** when a value must always exist, is never reassigned,
  and you don't need to store it. Use a **pointer** when null is possible or
  the target must be changed.

## Classes and Objects

```cpp
#include <string>

class User {
private:
    std::string name_;
    int age_;

public:
    User(std::string name, int age) : name_(name), age_(age) {}

    const std::string& name() const { return name_; }
};
```

- `const` member functions promise not to modify the object.
- Use member initializer lists (`: name_(name)`) to initialize members.
- Prefer `struct` for plain data aggregates, `class` when invariants apply.

## Function Overloading

```cpp
#include <string>

int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }

// Default arguments
void log(const std::string& msg, bool verbose = false);
```

## Standard Containers

```cpp
#include <vector>
#include <unordered_map>

std::vector<int> nums = {1, 2, 3};
nums.push_back(4); // append

std::unordered_map<std::string, int> ages;
ages["Ada"] = 36;

for (const auto& item : nums) { // range-based for loop
    // item is a const reference
}
```

## Dynamic Memory (RAII)

```cpp
#include <memory>

auto ptr = std::make_unique<int>(42); // owned, auto-freed
auto shared = std::make_shared<int>(42); // shared ownership

// No manual delete needed — lifetime tied to scope
```

- Prefer stack objects and smart pointers (`std::unique_ptr`,
  `std::shared_ptr`) over raw `new`/`delete`.
- RAII guarantees resources are released when the object goes out of scope.

## Common Modern Features

```cpp
#include <string>
#include <utility>

// Lambdas with capture (space keeps the capture list unambiguous)
auto times = [factor = 2] (int x) { return x * factor; };

// Structured bindings
std::pair<int, std::string> p = {1, "one"};
auto [num, label] = p;

// Move semantics: transfer ownership instead of copying
std::string a = "ada";
std::string b = std::move(a);
```

## References

- [cppreference.com](https://en.cppreference.com/w/cpp)
- [C++ Language Specification (ISO/IEC 14882)](https://isocpp.org/std/status)