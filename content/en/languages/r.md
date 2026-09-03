---
title: R
description: Core R syntax for vectors, lists, data frames, indexing, functions, and control flow.
tags:
  - data-science
  - statistics
  - scripting
---

## Vectors

```r
x <- c(1, 2, 3, 4)        # numeric vector (c = combine)
y <- c("a", "b", "c")     # character vector

# Sequences
1:5                       # 1 2 3 4 5
seq(1, 10, by = 2)        # 1 3 5 7 9
rep(0, 4)                 # 0 0 0 0

# Vectorized operations
x * 2                     # 2 4 6 8
x + 1                     # 2 3 4 5
x[x > 2]                  # 3 4  (logical subsetting)
sum(x)                    # 10
mean(x)                   # 2.5
length(x)                 # 4
```

- `c()` builds vectors; operations are element-wise and recycled.
- Indexing uses `[ ]`; `x[x > 2]` filters by a logical condition.

## Lists

```r
lst <- list(name = "Ada", age = 36, scores = c(1, 2, 3))
lst$name            # "Ada"
lst[["age"]]        # 36
lst[[3]]            # c(1, 2, 3)  (single element, contents)
lst["age"]          # list(age = 36) (a sub-list)

# Lapply returns a list; unlist collapses it
unlist(lapply(list(1, 2, 3), function(v) v * 10))  # 10 20 30
```

- `[[ ]]` extracts the element itself; `[ ]` returns a sub-list.
- `$name` is shorthand for `lst[["name"]]`.

## Data Frames

```r
df <- data.frame(
  name = c("Ada", "Grace", "Linus"),
  age  = c(36, 85, 53)
)

df$name            # column as a vector
df[["age"]]        # column
df[1, ]            # first row (a data frame)
df[, "age"]        # age column
df[df$age > 50, ]  # rows where age > 50

nrow(df)           # 3
ncol(df)           # 2
str(df)            # structure overview
```

- A data frame is a list of equal-length columns.
- Subset rows with a logical condition and a trailing comma: `df[condition, ]`.

## Indexing

```r
v <- c(10, 20, 30, 40, 50)

v[1]               # 10
v[c(1, 3)]         # 10 30
v[-1]              # drop first -> 20 30 40 50
v[v > 25]          # 30 40 50
names(v) <- letters[1:5]
v["a"]             # 10
```

- Negative indices drop elements.
- Logical vectors select elements where the condition is true.

## Functions

```r
add <- function(a, b) {
  a + b
}

# Default arguments; last expression is returned
greet <- function(name, punct = "!") {
  paste0("Hello, ", name, punct)
}

add(2, 3)              # 5
greet("Ada")           # "Hello, Ada!"

result <- add(5, 7)

# Anonymous functions
sapply(1:3, function(v) v + 100)   # 101 102 103
```

- Functions are objects assigned like variables.
- The last evaluated expression is the return value; use `return()` for early exits.

## Control Flow

```r
x <- 10

if (x > 0) {
  "positive"
} else {
  "non-positive"
}

# Vectorized ifelse
score <- c(10, -5, 7)
ifelse(score > 0, "pos", "neg")    # "pos" "neg" "pos"

for (i in 1:3) {
  print(i)
}

i <- 0
while (i < 3) {
  i <- i + 1
}
```

- `if` is scalar; use `ifelse` for vectorized conditional selection.
- Functions inside `*apply` and `for` loops are common idioms.

## Factor and Data Transformation

```r
colors <- factor(c("red", "green", "red", "blue"))
levels(colors)      # "blue" "green" "red"
table(colors)       # counts per level

# Recoding / creating a categorical column
df$bucket <- ifelse(df$age >= 50, "senior", "junior")

# Sorting
sort(c(3, 1, 2))            # 1 2 3
order(c(3, 1, 2))           # 2 3 1  (indices that sort the vector)
df[order(df$age), ]         # rows sorted by age
```

- A factor stores categorical data as levels with a mapping to integers.
- `table()` tabulates counts; `order()` returns sort indices for reordering rows.

## Environment and Packages

```r
# Install and load a package
install.packages("dplyr")     # once
library(dplyr)                # each session

# Help
?mean
str(mean)
```

- Packages are installed once and loaded per session with `library()`.
- `install.packages` requires an active internet connection.

## Idiomatic Modern Patterns

```r
# Pipe operator (base R 4.1+) and magrittr
df |>
  subset(age > 30) |>
  nrow()                       # number of rows with age > 30

# Anonymous function with a pipe in R 4.1+: use \(x)
sapply(1:3, \(v) v * 2)        # 2 4 6

# Extract and combine
vals <- c(TRUE, FALSE, TRUE)
any(vals)          # TRUE
all(vals)          # FALSE
```

- The base pipe `|>` (R 4.1+) forwards a value as the first argument.
- `\(x)` is the base shorthand for anonymous functions (R 4.1+).

## References

- [R Documentation](https://www.r-project.org/)
- [R for Data Science](https://r4ds.hadley.nz/)
- [R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf)