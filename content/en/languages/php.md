---
title: PHP
description: Core PHP syntax for variables, arrays, functions, classes, namespaces, and modern typing.
tags:
  - web
  - backend
---

## Variables and Types

```php
<?php

declare(strict_types=1);   // must be the first statement in the file

$name = "Ada";          // string
$age = 36;              // int
$height = 1.72;         // float
$isActive = true;       // bool

$tags = ["php", "web"];                 // indexed array
$profile = ["role" => "dev", "remote" => true];  // associative array
```

- Variable names start with `$`; PHP is dynamically typed.
- `declare(strict_types=1)` must be the first statement after `<?php`.
- Single-quoted strings do not interpolate; double-quoted strings do.

## Arrays

```php
<?php

$numbers = [1, 2, 3, 4, 5];
$numbers[] = 6;                       // append

$map = ["one" => 1, "two" => 2];
$map["three"] = 3;                    // add a key

echo count($numbers);                 // 6
echo implode(",", $numbers);          // "1,2,3,4,5,6"

foreach ($numbers as $n) {
    echo $n;
}

foreach ($map as $key => $value) {
    echo "$key=$value;";
}
```

## Control Flow

```php
<?php

$n = 10;

if ($n > 0) {
    // positive
} elseif ($n < 0) {
    // negative
} else {
    // zero
}

switch ($n) {
    case 1:
        echo "one";
        break;
    case 2:
    case 3:
        echo "two or three";
        break;
    default:
        echo "other";
}

// match expression (PHP 8+)
$label = match ($n) {
    1 => "one",
    2, 3 => "two or three",
    default => "other",
};

for ($i = 0; $i < 5; $i++) {
    // iterate
}

while ($n > 0) {
    $n--;
}
```

- `match` is an expression and does not require `break`.
- `switch` requires `break` to avoid fallthrough.

## Functions

```php
<?php

function add(int $a, int $b): int {
    return $a + $b;
}

// Default values and nullable types
function greet(string $name, string $punct = "!"): string {
    return "Hello, $name$punct";
}

echo greet("Ada");          // "Hello, Ada!"
echo greet("Ada", "?");     // "Hello, Ada?"

// Arrow functions (PHP 7.4+)
$square = fn(int $x): int => $x * $x;
echo $square(4);            // 16

// Variadic
function sum(...$numbers): int {
    return array_sum($numbers);
}
```

- Scalar type hints (`int`, `string`, `float`, `bool`) enforce types.
- `fn` arrow functions capture variables from the enclosing scope automatically.
- A parameter type that may be null is written `?Type`.

## Classes and Objects

```php
<?php

class User
{
    public string $name;
    private int $age;

    public function __construct(string $name, int $age)
    {
        $this->name = $name;
        $this->age = $age;
    }

    public function greet(): string
    {
        return "Hi, I'm {$this->name}";
    }
}

// Typed properties (PHP 7.4+), constructor promotion (PHP 8+)
class Product
{
    public function __construct(
        public string $name,
        private float $price,
    ) {}

    public function price(): float
    {
        return $this->price;
    }
}
```

- Properties and methods may carry type declarations.
- Constructor property promotion (PHP 8+) declares and assigns in the parameter list.

## Namespaces and Imports

```php
<?php

namespace App\Models;

use App\Support\Helper;      // import a namespace or class

class User {}

// refer to an imported class directly
$helper = new Helper();
```

- A `namespace` scopes code; `use` imports classes/namespaces.
- A file should declare `declare(strict_types=1);` at the top when strict typing is desired.

## Error Handling

```php
<?php

class ValidationError extends \Exception {}

function parsePort(string $value): int
{
    $port = (int) $value;
    if ($port < 1 || $port > 65535) {
        throw new ValidationError("Port out of range");
    }
    return $port;
}

try {
    $port = parsePort("8080");
    echo "using $port";
} catch (ValidationError $e) {
    echo "validation: " . $e->getMessage();
} catch (\Exception $e) {
    // handle any other exception
    throw $e;   // rethrow if not handled here
}
```

- Throw an `\Exception` subclass or `\Throwable` implementation.
- `catch` blocks may be ordered from most specific to most general.
- `finally` (not shown) runs regardless of whether an exception was thrown.

## Common Modern Patterns

```php
<?php

// Null-safe operator and null coalescing (PHP 8+)
class Request {
    public ?string $header = null;
}
$request = null;                       // or an instance of Request
$token = $request?->header ?? "default";   // "default" (null-safe on null)

// Null coalescing assignment
$config ??= [];

// Spread operator in arrays
$a = [1, 2];
$b = [3, 4];
$parts = [...$a, ...$b];          // [1, 2, 3, 4]

// String functions
$trimmed = trim("  hi  ");          // "hi"
$parts = explode(",", "a,b,c");     // ["a", "b", "c"]
$upper = strtoupper("hi");          // "HI"

// array_map / array_filter
$map = array_map(fn($x) => $x * 2, [1, 2, 3]);       // [2, 4, 6]
$filter = array_filter([1, 2, 3], fn($x) => $x > 1); // [2, 3]
```

- `?->` returns null instead of erroring when the left side is null.
- `??` returns the right-hand side when the left is null.
- `array_map` and `array_filter` operate on arrays functionally.

## References

- [PHP Manual](https://www.php.net/manual/en/)
- [PHP Language Reference](https://www.php.net/manual/en/langref.php)
- [What's New in PHP](https://www.php.net/releases)