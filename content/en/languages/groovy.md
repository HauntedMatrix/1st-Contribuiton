---
title: Groovy
description: Core Groovy syntax for coercion, GStrings, closures, lists, maps, and classes.
tags:
  - jvm
  - scripting
---

## Variables and GStrings

```groovy
def name = 'Ada'        // inferred type
int age = 36            // typed variable
String city = 'Paris'

// Single quotes: literal; double quotes: GString (interpolated)
def msg = "Hello, ${name}!"
def literal = 'Hello, ${name}!'   // not interpolated
```

- `def` infers the type from the value.
- Double-quoted strings interpolate `${expr}`; single-quoted ones do not.
- GStrings interpolate when `toString()` is called. A referenced variable's *current* value is used at that moment, so changing the variable later changes the rendered string; single-quoted strings are always literal.

## Lists

```groovy
def list = [1, 2, 3]
list[0]          // 1
list << 4        // append -> [1,2,3,4]
list - [3]       // remove value -> [1,2,4]
list.size()      // 4

// Iteration
list.each { println it }
list.each { v -> println v }

// Transformations
list.findAll { it > 1 }      // [2,3,4]
list.collect { it * 2 }      // [2,4,6,8]
list.sum()                   // 10
```

- Lists use literal syntax `[]` and support `<<` to append.
- `it` is the implicit closure parameter.
- `findAll`, `collect`, and `sum` are common list operations.

## Maps

```groovy
def profile = [name: 'Ada', age: 36]

profile.name           // 'Ada'
profile['age']         // 36
profile.role = 'dev'   // add a key
profile.name = 'Grace' // update

// Iteration
profile.each { k, v -> println "$k = $v" }
profile.entrySet()     // set of Map.Entry
```

- Maps use `[key: value]` literal syntax.
- Access fields with `.` (map-style property) or `['key']`.
- `each` with `{ k, v -> }` iterates key/value pairs.

## Control Flow

```groovy
def n = 10

if (n > 0) {
    println 'positive'
} else {
    println 'non-positive'
}

def label = n > 5 ? 'large' : 'small'

// switches are flexible
switch (n) {
    case 1: return 'one'
    case { it > 5 }: return 'large'
    default: return 'other'
}

for (def v in [1, 2, 3]) {
    println v
}

// while
def i = 0
while (i < 5) {
    i++
}
```

- Groovy's `switch` matches types, ranges, and closures.
- Closures in `case` receive `it` as the value.

## Closures

```groovy
def square = { x -> x * x }
square(4)          // 16

// Closures can reference outer variables and use `it`
def add = { a, b -> a + b }
add(2, 3)          // 5

// Passing closures to methods
def result = [1, 2, 3].inject(0) { acc, x -> acc + x }   // 6
```

- Closures are `{ params -> body }`; single value is `it`.
- `inject` folds a list using an accumulator and closure.

## Methods and Defaults

```groovy
def add(int a, int b) {
    a + b
}

// Last expression is the return value
def greet(String name, String punct = '!') {
    "Hello, $name$punct"
}

add(2, 3)               // 5
greet('Ada')            // 'Hello, Ada!'
greet('Ada', '?')       // 'Hello, Ada?'
```

- The last expression in a method is its return value.
- Default parameter values apply when an argument is omitted.
- Parentheses are optional when calling methods with arguments.

## Classes and Traits

```groovy
class Person {
    String name
    Integer age

    String greet() {
        "Hi, I'm $name"
    }
}

def ada = new Person(name: 'Ada', age: 36)
ada.greet()            // "Hi, I'm Ada"

// Getters/setters and property-style access
ada.name = 'Grace'
println ada.name
```

- Property-style access `name` compiles to getter/setter calls.
- Constructor kwargs (`new Person(name:...)`) map to properties.

## Spread and Collect

```groovy
def names = ['ada', 'grace', 'linus']
def upper = names.collect { it.toUpperCase() }   // ['ADA', 'GRACE', 'LINUS']

def uppercaseAll = names*.toUpperCase()          // same result (spread)
```

- `*.` is the spread operator calling a method on each element.
- `collect` transforms a list; the result is a new list.

## References

- [Groovy Documentation](https://groovy-lang.org/documentation.html)
- [Groovy JDK Enhancements](https://groovy-lang.org/groovy-dev-kit.html)
- [Apache Groovy](https://groovy-lang.org/)