---
title: Objective-C
description: Core Objective-C syntax for classes, message passing, collections, and blocks.
tags:
  - apple
  - object-oriented
---

## Message Passing

```objc
// Messages are sent with square brackets
NSString *name = @"Ada";
NSUInteger len = [name length];

// Nested messages
NSString *upper = [[name uppercaseString] lowercaseString];

// Arguments use colons
NSString *combo = [name stringByAppendingString:@"!"];
```

- Objective-C extends C; most calls are sent as messages: `[receiver selector:arg]`.
- `@"..."` is the constant NSString literal.
- Methods are invoked with `[object method]`.

## Message Sending and Selectors

```objc
// Method declarations in the interface
@interface Greeter : NSObject
- (void)sayHelloTo:(NSString *)person;
- (int)add:(int)a to:(int)b;
@end

@implementation Greeter
- (void)sayHelloTo:(NSString *)person {
    NSLog(@"Hello, %@", person);
}

- (int)add:(int)a to:(int)b {
    return a + b;
}
@end

// Usage
Greeter *greeter = [[Greeter alloc] init];
[greeter sayHelloTo:@"Ada"];      // the exact selector is sayHelloTo:
int total = [greeter add:2 to:3]; // 5
```

- Selectors include the colons (e.g. `add:to:`).
- Messages may be nested and chained.

## NSString

```objc
NSString *name = @"Ada";
NSUInteger len = [name length];               // 3
NSString *sub = [name substringToIndex:2];    // "Ad"
NSString *upper = [name uppercaseString];     // "ADA"
BOOL equal = [name isEqualToString:@"Ada"];   // YES

// Immutable; mutable via NSMutableString
NSMutableString *m = [NSMutableString stringWithString:@"Go"];
[m appendString:@"!"];
```

- NSString is immutable; `NSMutableString` is the mutable variant.
- `length`, `substringToIndex`, and `uppercaseString` are common selectors.
- `isEqualToString:` compares string contents.

## Arrays and Dictionaries

```objc
// NSArray is immutable
NSArray *fruits = @[@"apple", @"banana", @"cherry"];
fruits[1];                     // "banana"
fruits.count;                  // 3

// NSDictionary
NSDictionary *profile = @{ @"name": @"Ada", @"age": @36 };
profile[@"name"];              // "Ada"

// Mutable variants
NSMutableArray *list = [NSMutableArray array];
[list addObject:@"x"];

NSMutableDictionary *dict = [NSMutableDictionary dictionary];
dict[@"k"] = @"v";
```

- Class-cluster literals `@[ ]` (array) and `@{ }` (dictionary) create objects.
- Boxed numbers use `@` (e.g. `@36`).
- Use `NSMutableArray`/`NSMutableDictionary` for mutation.

## Blocks and Closures

```objc
int (^square)(int) = ^(int x) {
    return x * x;
};
square(4);                 // 16

// Blocks capture local variables
int base = 10;
int (^addBase)(int) = ^(int x) {
    return base + x;
};
addBase(5);                // 15

// Passing a block to a collection method (realized, not a "map"):
NSArray *nums = @[@1, @2, @3];
[nums enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
    NSNumber *n = (NSNumber *)obj;
    NSLog(@"%ld", (long)n.intValue * 2);
}];
```

- Blocks are closures introduced with `^`.
- Literal syntax creates array/dictionary literals; blocks capture surrounding scope.
- Plain local variables are captured as *const copies*: a block can read but not reassign them. Declare a captured variable `__block` to let the block modify it.
- `^` in a method signature marks a block return type/arguments.

## Properties

```objc
@interface Person : NSObject
@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;
@end

@implementation Person
@end

Person *ada = [[Person alloc] init];
ada.name = @"Ada";
NSLog(@"%@", ada.name);      // Ada
```

- `@property` declares accessors; LLVM auto-synthesizes getters/setters (explicit `@synthesize` is optional).
- Dot syntax `ada.name` calls the getter/setter.

## Loops and Conditionals

```objc
NSInteger n = 10;

if (n > 0) {
    NSLog(@"positive");
} else {
    NSLog(@"non-positive");
}

// Fast enumeration over a collection
NSArray *fruits = @[@"apple", @"banana", @"cherry"];
for (NSString *fruit in fruits) {
    NSLog(@"%@", fruit);
}

// C-style loop
for (NSInteger i = 0; i < 5; i++) {
    NSLog(@"%ld", (long)i);
}
```

- Fast enumeration `for (Type *x in collection)` iterates collections.
- `%@` formats an object description in `NSLog`.

## References

- [Objective-C Programming Language](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
- [Apple Developer Documentation](https://developer.apple.com/documentation/)
- [Objective-C Runtime](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Introduction/Introduction.html)