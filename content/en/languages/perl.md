---
title: Perl
description: Core Perl syntax for scalars, arrays, hashes, references, regular expressions, and subroutines.
tags:
  - scripting
  - text-processing
---

## Variables and Types

```perl
#!/usr/bin/perl
use strict;
use warnings;

my $name = "Ada";        # scalar: string/number
my $count = 10;          # scalar: number
my $active = 1;          # truthy integer

my @tags = ("perl", "web");     # array
my %profile = (role => "dev");  # hash (key => value)
```

- `use strict;` and `use warnings;` are recommended for every script.
- `my` declares a lexical scope.
- Sigils: `$scalar`, `@array`, `%hash`.
- A scalar can hold either a string or a number; Perl converts between contexts on demand, but the conversion is implicit and can surprise (e.g. a non-numeric string numifies as `0` with a warning).

## Scalars and Strings

```perl
my $x = "42";
my $n = $x + 1;          # 43 (numeric coercion)
my $len = length($x);    # 2
my $upper = uc("hi");    # HI

# String interpolation vs single quotes
my $a = "hi $name";      # interpolates
my $b = 'hi $name';      # literal

# Get user input (strip the newline)
chomp(my $line = <STDIN>);

# Concatenation
my $msg = "Hello, " . $name . "!";
```

- `length`, `uc`, and `.` (concatenation) are core string tools.
- `chomp` removes a trailing newline.

## Arrays

```perl
my @nums = (1, 2, 3, 4);

$nums[0];            # first element
$nums[-1];           # last element
push @nums, 5;       # append
pop @nums;           # remove last -> 5
unshift @nums, 0;    # prepend
shift @nums;         # remove first -> 0

my $count = scalar @nums;   # number of elements

for my $v (@nums) {
    print "$v\n";
}

# Slice
my @first_two = @nums[0, 1];
```

- `$arr[0]` accesses a single element; `@arr[0, 1]` is a slice returning a list (the sigil before the name shows the *storage*, while the index form chooses `$` or `@`).
- `push`/`pop` operate on the end; `unshift`/`shift` on the front.

## Hashes

```perl
my %ages = (ada => 36, grace => 85);

$ages{ada};          # 36
$ages{linus} = 53;   # add a key

delete $ages{ada};   # remove a key
exists $ages{ada};   # 0 (false) after delete

# Iteration
for my $key (keys %ages) {
    print "$key => $ages{$key}\n";
}

# Slice of values
my @names = keys %ages;
```

- Hashes map strings to scalars; access a single value with `$hash{key}`.
- `keys`, `delete`, and `exists` are the primary hash operators.

## References and Dereferencing

```perl
my $hash_ref = { a => 1 };       # reference to a hash
my $array_ref = [1, 2, 3];       # reference to an array

my $first = $array_ref->[0];     # 1
my $value = $hash_ref->{a};      # 1

# Nested structures use the -> dereference
my $nested = { user => { name => "Ada" } };
my $name = $nested->{user}->{name};   # "Ada"
```

- References let you build complex, nested data structures.
- Use `->` to dereference array/hash references.

## Regular Expressions

```perl
my $email = "ada@example.com";

# Match
if ($email =~ /@/) {
    print "has @\n";
}

# Capture
$email =~ /(\w+)@(\w+\.\w+)/;
my ($user, $domain) = ($1, $2);

# Substitution
$email =~ s/example/test/;   # only first occurrence
$email =~ s/\./ /g;          # global replace (all dots)

# Split / join
my @parts = split(/,/, "a,b,c");
my $joined = join("-", @parts);   # "a-b-c"
```

- `=~` applies a regex to a string; capture groups are `$1`, `$2`.
- `s///` substitutes; the `/g` flag makes it global.
- `split` and `join` convert between strings and lists.

## Subroutines

```perl
sub add {
    my ($a, $b) = @_;    # arguments come via @_
    return $a + $b;
}

my $sum = add(2, 3);     # 5

sub greet {
    my ($name, $punct) = @_;
    $punct //= "!";      # default when undef
    return "Hello, $name$punct";
}

print greet("Ada");      # "Hello, Ada!"
print greet("Ada", "?"); # "Hello, Ada?"
```

- Arguments are passed in `@_`; destructure with `my ($a, $b) = @_;`.
- `return` returns a value; a bare `return` returns undef.
- Use `//=` to default an argument when it is undef.

## Control Flow

```perl
my $n = 10;

if ($n > 0) {
    print "positive\n";
} elsif ($n < 0) {
    print "negative\n";
} else {
    print "zero\n";
}

# Ternary
my $label = $n > 0 ? "pos" : "non-pos";

# Postfix forms
print "big\n" if $n > 5;

my $countdown = 3;
$countdown-- while $countdown > 0;   # 3 -> 2 -> 1 -> stops

for my $i (1 .. 5) {
    print "$i\n";
}

my $i = 0;
while ($i < 5) {
    $i++;
}
```

- Postfix `if`/`while` read in a single line.
- `1 .. 5` is a range; `for my $x (list)` is the idiomatic loop.

## Text Processing

```perl
my $text = "The quick brown fox";

# Line-by-line reading
while (my $line = <>) {     # <> reads from stdin or @ARGV files
    chomp $line;
    print "$line\n";
}

# Word count via match
my $count = () = $text =~ /\w+/g;   # 4 words
```

- `<>` reads from standard input or files listed in `@ARGV`.
- Assigning a list match to an empty list `()` yields the number of matches.

## References

- [Perl Documentation](https://perldoc.perl.org/)
- [Perl Language](https://www.perl.org/)
- [Perl Tutorial](https://perldoc.perl.org/perlretut)