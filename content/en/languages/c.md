---
title: C
description: Core C syntax for data types, control flow, pointers, and memory management.
tags:
  - systems
  - embedded
---

## Data Types and Variables

```c
int age = 25;            // basic integer
double price = 19.99;    // double-precision float
char letter = 'A';       // single character
char name[] = "Ada";     // null-terminated string
const float PI = 3.14f;  // read-only value
```

## Control Flow

```c
int n = 10;

if (n > 0) {
    // positive
} else {
    // non-positive
}

for (int i = 0; i < n; i++) {
    // iterate i from 0 to 9
}

while (n-- > 0) {
    // loop while n is positive
}
```

## Functions

```c
#include <stdio.h>

int add(int a, int b) {
    return a + b;
}

void greet(const char* name) {
    printf("Hello, %s\n", name);
}

// Function prototype / declaration
int multiply(int a, int b);

// A function that returns the result of multiply
int multiply(int a, int b) {
    return a * b;
}
```

## Pointers

```c
int value = 42;
int* ptr = &value;   // address of value

*ptr = 43;           // dereference to modify value
int copy = *ptr;     // read the pointed-to value

int array[3] = {1, 2, 3};
int* first = array;  // array name decays to a pointer
```

## Dynamic Memory Allocation

```c
#include <stdlib.h>

int* numbers = malloc(10 * sizeof *numbers); // heap allocation

if (numbers != NULL) {
    numbers[0] = 1;
    free(numbers); // release allocated memory
}
```

## Common Standard Library Functions

```c
#include <stdio.h>
#include <string.h>

printf("Hello, %s\n", "world"); // format output
char buffer[64];
snprintf(buffer, sizeof(buffer), "%d", 42); // safe formatted write

char a[20];
strcpy(a, "Ada");        // copy string into a (must be large enough)
strcat(a, " Lovelace");  // append; a must have room for the result
int length = strlen(a);  // string length
```

## References

- [C Reference](https://en.cppreference.com/w/c)
- [GNU C Manual](https://www.gnu.org/software/gnu-c-manual/)
- [Learn C Archives | C Programming Language (ISO)](https://www.open-std.org/jtc1/sc22/wg14/)