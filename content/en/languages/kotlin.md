---
title: Kotlin
description: Core Kotlin syntax for null safety, functions, classes, collections, and coroutines.
tags:
  - jvm
  - android
  - backend
---

## Variables and Types

```kotlin
val name = "Ada"          // read-only reference (final)
var count = 0             // mutable variable
count += 1

// Basic types: Byte, Short, Int, Long, Float, Double, Boolean, Char, String
val n: Int = 42
val f: Double = 3.14
val flag: Boolean = true

// Type inference
val message = "Hello"     // String

// String templates
val greeting = "Hello, $name!"
val length = "abc".length
```

- `val` is a read-only reference; it does not make the object immutable.
- `var` allows reassignment.

## Null Safety

```kotlin
var nullable: String? = null   // nullable type
val length: Int? = nullable?.length   // safe call -> null if nullable is null
val fallback: Int = nullable?.length ?: 0   // Elvis operator

// Non-null assertion (only after you have verified the value)
val sure: String = nullable!!
```

- `?` marks a type as nullable.
- `?.` returns null if the receiver is null.
- `?:` provides a default when the left side is null.
- `!!` throws `NullPointerException` if the value is null — avoid unless truly required.

## Control Flow

```kotlin
val n = 10

if (n > 0) {
    // positive
} else {
    // non-positive
}

// if is an expression
val label = if (n > 0) "positive" else "non-positive"

// when expression
val kind = when (n) {
    1 -> "one"
    2, 3 -> "two or three"
    in 4..10 -> "four to ten"
    else -> "other"
}

for (i in 1..5) {
    // 1, 2, 3, 4, 5
}

for (i in 5 downTo 1) {
    // 5, 4, 3, 2, 1
}

var i = 0
while (i < 5) {
    i++
}
```

## Functions

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

// Expression body
fun multiply(a: Int, b: Int): Int = a * b

// Default and named arguments
fun greet(name: String, punctuation: String = "!") = "Hello, $name$punctuation"
println(greet("Ada"))               // "Hello, Ada!"
println(greet(name = "Ada", punctuation = "?"))  // "Hello, Ada?"

// Lambda
val square: (Int) -> Int = { x -> x * x }
val doubled = listOf(1, 2, 3).map { it * 2 }  // [2, 4, 6]
```

## Classes and Inheritance

```kotlin
class User(val name: String, var age: Int) {
    fun greet(): String = "Hi, I'm $name"
}

// Data classes provide equals, hashCode, toString, copy, componentN
data class Point(val x: Int, val y: Int)

// Sealed class — restricted hierarchy
sealed class LoadState
data class Success(val value: Int) : LoadState()
data class Failure(val reason: String) : LoadState()

// Inheriting and overriding (mark the base open)
open class Animal
class Dog : Animal()
```

- Use `open` to allow a class or member to be overridden.
- A `data class` auto-generates `equals`, `hashCode`, `toString`, `copy`, and component functions.
- A `sealed class` restricts subclasses to the same file (or module).

## Extension Functions

```kotlin
fun String.isBlankOrNull(): Boolean = this.trim().isEmpty()

fun Char.isVowel(): Boolean = this in setOf('a', 'e', 'i', 'o', 'u')

println("   ".isBlankOrNull())  // true
println('e'.isVowel())          // true
```

- Extension functions add behavior to an existing type without modifying it.

## Collections

```kotlin
val nums = listOf(1, 2, 3)          // read-only list
val mutable = mutableListOf(1, 2)   // mutable list
mutable.add(3)

val map = mapOf("a" to 1, "b" to 2)   // read-only map
val set = setOf(1, 2, 3)              // read-only set

nums.map { it * 2 }          // [2, 4, 6]
nums.filter { it > 1 }       // [2, 3]
nums.sum()                   // 6

for (n in nums) {
    println(n)
}

// Destructuring a list
val (first, second) = listOf(1, 2)
```

- `listOf`/`mapOf`/`setOf` return read-only views; use the `mutable*` variants to modify.

## Error Handling

```kotlin
import java.io.IOException

fun readFile(path: String): String {
    try {
        // attempt to read the file
        return java.io.File(path).readText()
    } catch (e: IOException) {
        throw IllegalArgumentException("Could not read $path", e)
    }
}

// try is an expression
val parsed = try {
    "42".toInt()
} catch (e: NumberFormatException) {
    -1
}
```

- `throw` raises an exception; catch the most specific exception first.
- `try`/`catch` can be used as an expression to yield a value.

## Coroutines (async)

```kotlin
import kotlinx.coroutines.*

suspend fun fetchUser(id: Int): String {
    delay(300)   // simulate an async operation
    return "user-$id"
}

// Run in a coroutine scope
fun load() = runBlocking {
    val user = async { fetchUser(1) }
    val profile = async { fetchUser(2) }
    println("${user.await()} ${profile.await()}")
}
```

- A `suspend` function can only be called from a coroutine (or another suspend function).
- `async` launches a coroutine that returns a result; `await` waits for it.
- `runBlocking` bridges blocking code with a coroutine scope.

## Idiomatic Modern Patterns

```kotlin
fun <T> first(items: List<T>): T? = items.firstOrNull()

// Wrap fallible code in a Result instead of throwing
val parsed: Result<Int> = runCatching { "42".toInt() }
val value = parsed.getOrNull()   // 42 on success, null on failure

data class User(val id: Int, val name: String)
val users = listOf(User(1, "Ada"))
val names = users.map { it.name }   // ["Ada"]

fun describe(state: LoadState): String = when (state) {
    is Success -> "success: ${state.value}"
    is Failure -> "failure: ${state.reason}"
}
```

- `runCatching` wraps a block and returns a `kotlin.Result<T>`.
- Use `when` with `is` to smart-cast sealed subclasses.
- Prefer `firstOrNull()` over a manual null check.

## References

- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)
- [Kotlin Language Specification](https://kotlinlang.org/spec/)
- [Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)