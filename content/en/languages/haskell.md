---
title: Haskell
description: Core Haskell syntax for functions, types, lists, pattern matching, algebraic data types, and type classes.
tags:
  - functional
---

## Variables and Functions

```haskell
-- Bindings are immutable; this is a top-level definition
name :: String
name = "Ada"

-- Function: parameters then body
add :: Int -> Int -> Int
add a b = a + b

-- Function application is just juxtaposition with spaces
-- add 2 3   evaluates to 5

-- Single-argument functions
double :: Int -> Int
double x = x * 2
```

- Bindings and functions are declared with `name = value`.
- `::` declares a type; `a -> b` is a function from `a` to `b`.
- Function application uses spaces, not parentheses: `f x y`.

## Currying and Composition

```haskell
-- add 2 3 is (add 2) 3 — partial application
inc :: Int -> Int
inc = add 1

-- Composition
evenSquares :: [Int] -> [Int]
evenSquares = filter even . map (^ 2)
```

- Functions are curried: they take one argument and return a function.
- `.` composes functions right-to-left.

## Lists

```haskell
nums :: [Int]
nums = [1, 2, 3, 4]

-- Constructor (:) prepends; [] is empty
head nums          -- 1
tail nums          -- [2,3,4]
nums !! 0          -- 1
length nums        -- 4
sum nums           -- 10

-- Ranges
[1 .. 5]           -- [1,2,3,4,5]
[2, 4 .. 10]       -- [2,4,6,8,10]

-- Functions over lists
map (* 2) nums     -- [2,4,6,8]
filter even nums   -- [2,4]
take 2 nums        -- [1,2]
```

- `:` prepends, `++` concatenates.
- `map`, `filter`, `take`, and `sum` operate on lists.
- `head`, `tail`, and `(!!)` are partial: they raise an error on the empty list (and `(!!)` on an out-of-range index). Prefer `take`, `dropWhile`, total pattern matching, or `safeHead`-style total functions over them in production code.

## Pattern Matching

```haskell
describe :: Int -> String
describe 1 = "one"
describe n | n < 0 = "negative"
           | otherwise = "other"

-- Lists
sumFirstTwice :: [Int] -> Int
sumFirstTwice (x : y : _) = x + y   -- ignores the rest
sumFirstTwice _            = 0      -- fewer than two elements
```

- Function definitions can pattern-match on arguments.
- Guards (`| condition`) refine matching; `otherwise` is a fallback.

## Algebraic Data Types

```haskell
data Color = Red | Green | Blue

-- `Maybe` already exists in Prelude; shown here only to illustrate the shape
data Maybe a = Nothing | Just a

data Shape
  = Circle Double
  | Rectangle Double Double

draw :: Color -> String
draw Red   = "red"
draw Green = "green"
draw Blue  = "blue"
```

- `data` declares a new type; constructors begin with uppercase.
- `Maybe a` models optional values (`Nothing` or `Just a`).
- Fields in constructors can carry data (`Circle Double`).

## Type Classes

```haskell
-- A class constrains a type to support certain functions
class Pair a where
  multiply :: a -> a -> a

-- Instance for Int
instance Pair Int where
  multiply a b = a * b

-- Built-in common classes: Eq, Ord, Show, Read, Num
(==) :: Eq a => a -> a -> Bool
(<)  :: Ord a => a -> a -> Bool
show :: Show a => a -> String
```

- A type class is an interface for functions that must exist for a type.
- `instance` provides the implementation for a concrete type.
- `Eq`, `Ord`, `Show`, and `Read` are standard classes.

## Maybe and Either

```haskell
-- Maybe-safe division
safeDiv :: Double -> Double -> Maybe Double
safeDiv _ 0 = Nothing
safeDiv a b = Just (a / b)

result :: Maybe Double
result = safeDiv 10 2     -- Just 5.0

divisible :: Double -> Double -> Either String Double
divisible _ 0 = Left "division by zero"
divisible a b = Right (a / b)

-- Pattern matching on the result
case safeDiv 10 0 of
  Nothing -> putStrLn "failed"
  Just v  -> print v
```

- `Maybe` encodes optional values; `Either String a` can carry an error message.
- Use pattern matching or `case` to consume them.

## Standard Functions

```haskell
-- Higher-order functions
foldr (+) 0 [1, 2, 3]     -- 6
foldl (+) 0 [1, 2, 3]     -- 6
zip [1, 2] ['a', 'b']     -- [(1,'a'),(2,'b')]

-- Partial application
plusTen :: Int -> Int
plusTen = (+) 10
plusTen 5                 -- 15

-- Composition and pipelines
result :: Int
result = (+ 1) . (* 2) $ 10   -- 21
```

- `foldr`/`foldl` reduce a list using a binary function.
- For processing long lists, prefer `foldl'` (strict) over `foldl` (lazy), which can build up deferred thunks and overflow the stack.
- `$` applies a function to its right argument with low precedence.

## Monadic I/O

```haskell
main :: IO ()
main = do
  putStrLn "Name?"
  name <- getLine
  putStrLn ("Hello, " ++ name ++ "!")
```

- `do` blocks sequence actions in the `IO` monad.
- `<-` binds the result of an action.
- `putStrLn` and `getLine` are I/O actions.

## References

- [Haskell Documentation](https://www.haskell.org/documentation/)
- [Learn You a Haskell](https://learnyouahaskell.com/)
- [Haskell Language Report](https://www.haskell.org/onlinereport/)