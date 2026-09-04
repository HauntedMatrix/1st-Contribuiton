---
title: Erlang
description: Core Erlang syntax for modules, atoms, lists, pattern matching, case, and funs.
tags:
  - functional
  - concurrency
  - distributed
---

## Values and Matching

```erlang
%% Erlang uses single assignment: a variable binds once in its scope.
%% A pattern that repeats a bound name matches its existing value instead.
Name = "Ada",
Age = 36,
Height = 1.72,

%% Atom vs string
Role = developer,          %% atom
Str  = "hello",            %% string (list of bytes)

%% The match operator =
{X, Y} = {3, 4},            %% destructures
X.                          %% 3
```

- Every expression ends with a period (`.`) in the shell; commas separate.
- Variables start with an uppercase letter; atoms with a lowercase letter.
- `=` binds patterns; a failed match raises `badmatch`.
- Erlang is 1-indexed for lists.

## Lists and Tuples

```erlang
L = [1, 2, 3, 4],

hd(L),            %% 1
tl(L),            %% [2,3,4]
length(L),        %% 4
[0 | L],          %% [0,1,2,3,4]
[1, 2] ++ [3, 4], %% [1,2,3,4]

T = {erlang, otp},          %% tuple
element(2, T),              %% otp
setelement(2, T, beam),     %% {erlang, beam}
```

- Lists use `[ ]` and `|` (prepend); tuples use `{ }`.
- Lists are commonly built by prepending; `++` concatenates.
- Tuples are fixed-size; `element` and `setelement` access them.

## Pattern Matching in Functions

```erlang
-module(shape).
-export([area/1]).

area({square, Side}) -> Side * Side;
area({circle, Radius}) -> 3.14159 * Radius * Radius;
area(_) -> 0.
```

```erlang
%% usage
shape:area({square, 4}).    %% 16
shape:area({circle, 2}).    %% ~12.566
```

- Headless clauses in a module export a function name with arity (`area/1`).
- Pattern matching selects a clause based on the argument shape.

## Control Flow

```erlang
N = 10,

if
    N > 0 -> positive;
    N < 0 -> negative;
    true  -> zero
end,

%% case
case N of
    1 -> one;
    _ when N > 5 -> large;
    _ -> other
end,

%% guards
F = fun(N) when is_integer(N), N > 0 -> positive;
       (_) -> other
end.
```

- `if` clauses run in order until a true guard; include a `true` fallback.
- `case` matches a value to clauses; guards can refine matches.
- `fun` declares anonymous functions with optional guards.

## Guards

```erlang
is_atom(X),
is_integer(N),
is_list(L),
N > 5,
(X > 0; Y < 0)   %% any or — semicolon is an 'or' in guards

%% Using guards in a function head
-module(helpers).
-export([sign/1]).
sign(N) when N > 0 -> positive;
sign(N) when N < 0 -> negative;
sign(_) -> zero.
```

- Guard expressions include type checks (`is_integer`) and comparisons.
- A comma in a guard is a logical `and`; a semicolon is a logical `or`.
- Guards may appear in function heads and `case` clauses.

## Funs (Anonymous Functions)

```erlang
Double = fun(X) -> X * 2 end,
Double(4).                        %% 8

Add = fun(A, B) -> A + B end,
Add(2, 3).                        %% 5

%% Capture a module function
MapDouble = fun lists:map/2,
MapDouble(fun(X) -> X * 2 end, [1, 2, 3]).   %% [2,4,6]
```

- `fun` creates a closure; call it as a normal function.
- Capture module functions with `fun Module:Name/Arity`.
- Anonymous functions can be passed to higher-order functions like `lists:map`.

## Recursion and Higher-Order Functions

```erlang
factorial(0) -> 1;
factorial(N) -> N * factorial(N - 1).

%% Built-in list combinators
lists:map(fun(X) -> X * 2 end, [1, 2, 3]),      %% [2,4,6]
lists:filter(fun(X) -> X > 1 end, [1, 2, 3]),   %% [2,3]
lists:foldl(fun(X, Acc) -> X + Acc end, 0, [1, 2, 3]),   %% 6
```

- Recursion replaces iteration; a base clause terminates recursion.
- `lists:map`, `lists:filter`, and `lists:foldl` transform and reduce lists.

## Records

```erlang
-record(person, {name, age}).

%% Define and use
P = #person{name = "Ada", age = 36},
P#person.name,       %% "Ada"
P#person{age = 37}.  %% updated record
```

- Records define a named tuple with named fields.
- Defaults can be given in the record definition.
- `#record{field = value}` creates; `#rec.field` accesses.

## Error Handling

```erlang
case file:read_file("data.txt") of
    {ok, Bin} -> io:format("read ~p bytes~n", [byte_size(Bin)]);
    {error, Reason} -> io:format("error: ~p~n", [Reason])
end,

%% Pattern matching an expected shape
{ok, Bin} = file:read_file("data.txt").
```

- Many BIFs return `{ok, Value}` or `{error, Reason}`.
- Use `case` to branch on success/failure rather than relying on exceptions.
- Pattern matching with `{ok, Bin} = ...` fails loudly on error.

## Processes

```erlang
Pid = spawn(fun() -> io:format("hi") end),

Parent = self(),
spawn(fun() -> Parent ! {hello, self()} end),

receive
    {hello, From} -> io:format("~p", [From])
after 1000 -> io:format("timeout")
end.
```

- `spawn` creates a lightweight isolated process; `self()` is the current pid.
- `!` sends a message; `receive` waits for a matching message.
- `after` sets a receive timeout.

## References

- [Erlang Documentation](https://www.erlang.org/docs)
- [Erlang/OTP System Documentation](https://www.erlang.org/doc/)
- [Learn You Some Erlang](https://learnyousomeerlang.com/)