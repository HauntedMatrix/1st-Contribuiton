---
title: Bash
description: Core Bash syntax for variables, quoting, conditionals, loops, functions, and shell safety.
tags:
  - shell
  - scripting
  - devops
---

## Variables

```bash
name="Ada"                       # no spaces around =
echo "$name"                     # Ada
readonly pi=3.14159              # cannot be reassigned

# Command substitution
current_dir=$(pwd)
files=$(ls "$HOME")

# Arithmetic
count=10
count=$((count + 1))             # 11
echo $((5 * 3))                  # 15
```

- Quote variables: `"$name"` prevents word splitting and globbing.
- Use `$( )` for command substitution; avoid backticks.
- No spaces around `=` when assigning.

## Quoting

```bash
single='literal $HOME'       # no expansion
double="expanded $HOME"      # expands variables and commands
```

- Single quotes: everything literal.
- Double quotes: variable and command substitution happen; globbing is disabled.
- Always quote variable expansions in most contexts.

## Conditionals

```bash
if [ "$name" = "Ada" ]; then
    echo "hi Ada"
elif [ -f /tmp/file ]; then
    echo "file exists"
else
    echo "other"
fi

# Test operators
#   [ -f file ]  file exists and is a regular file
#   [ -d dir ]   directory exists
#   [ -n "$s" ]  string non-empty
#   [ -z "$s" ]  string empty
#   [ "$a" = "$b" ]   string equality
#   [ "$a" -eq "$b" ] integer equality
#   [ "$a" -gt "$b" ] integer greater-than

# Numeric test with [[ ]]
if [[ $count -gt 5 ]]; then
    echo "big"
fi
```

- `[ ]` is the POSIX test command; `[[ ]]` is a Bash extension (pattern matching, no word-splitting).
- Terminate `if` with `fi`.

## Loops

```bash
for i in 1 2 3; do
    echo "$i"
done

for f in *.txt; do
    echo "processing $f"
done

i=0
while [ "$i" -lt 5 ]; do
    echo "$i"
    i=$((i + 1))
done

i=0
until [ "$i" -ge 5 ]; do
    echo "$i"
    i=$((i + 1))
done
```

- `for` iterates over a list (expansions happen first).
- `while` repeats while the test is true.
- Use `break` to exit a loop, `continue` to skip to the next iteration.

## Functions

```bash
greet() {
    local name="$1"
    echo "Hello, $name"
}

add() {
    local a=$1 b=$2
    echo $((a + b))
}

greet "Ada"          # "Hello, Ada"
result=$(add 2 3)    # result = 5

# Return an exit status, not a value
fail() {
    return 1
}
```

- `$1`, `$2`, ... are positional parameters; `$@` is all of them.
- `local` scopes a variable to the function.
- Functions communicate results via stdout; `return` sets the exit status (0 = success).

## Positional Parameters and Special Variables

```bash
script_name=$0
first_arg=$1
all_args=$@        # each argument separately quoted
arg_count=$#
last_status=$?     # exit status of the last command
pid=$$
```

- `$?` holds the exit status of the last executed command.
- `$#` is the number of arguments.
- `$@` vs `$*`: `$@` keeps each argument separate when quoted.

## Exit Codes

```bash
# A command's exit status: 0 = success, non-zero = failure
if grep -q "needle" file.txt; then
    echo "found"
else
    echo "not found (exit $?)"
fi

# Explicitly exit with a status
exit 1   # signal an error

# Chain only on success/failure
cmd1 && cmd2      # run cmd2 only if cmd1 succeeds
cmd1 || cmd2      # run cmd2 only if cmd1 fails
```

## Arrays

```bash
fruits=("apple" "banana" "cherry")
echo "$fruits"        # first element: apple
echo "${fruits[1]}"   # banana
fruits+=("date")      # append
echo "${#fruits[@]}"  # length 4

for fruit in "${fruits[@]}"; do
    echo "$fruit"
done
```

- Declare with parentheses; index with `[n]`.
- `"${arr[@]}"` expands to each element as a separate word.
- `${#arr[@]}` is the element count.

## Parameter Expansion

```bash
name="Ada Lovelace"

echo "${name#Ada}"      # remove prefix "Ada"   -> " Lovelace"
echo "${name%Lovelace}" # remove suffix "Lovelace" -> "Ada "
echo "${name// /_}"     # replace all spaces with "_"
echo "${name:-default}" # use "default" if name is empty/unset
echo "${name:0:3}"      # first 3 characters -> "Ada"
len=${#name}            # string length
```

- `${var:-def}` provides a default for empty/unset variables.
- `${#var}` is the string length.
- Pattern-based prefix/suffix removal and global replacement are available.

## Shell Safety

```bash
# Fail fast on errors
set -e          # exit on any command that returns non-zero
set -u          # exit on use of an unset variable
set -o pipefail # a pipeline fails if any command in it fails

# Common combination at the top of a script
set -euo pipefail
```

- `set -e` can cause surprising early exits; test carefully.
- Prefer `local` in functions and quote expansions.

## Conditionals on Commands

```bash
if command -v curl >/dev/null 2>&1; then
    echo "curl is installed"
fi

# Run a command only if its prerequisite exists
mkdir -p build && cmake --install build
```

## References

- [Bash Reference Manual](https://www.gnu.org/software/bash/manual/)
- [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- [explainshell.com](https://explainshell.com/)