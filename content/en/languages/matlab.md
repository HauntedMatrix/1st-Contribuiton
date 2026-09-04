---
title: MATLAB
description: Core MATLAB syntax for matrices, indexing, functions, control flow, and plotting.
tags:
  - numerical
  - scientific
  - matrix
---

## Variables and Matrices

```matlab
% Comments start with %
x = 5;            % semicolon suppresses output
name = 'Ada';
flag = true;

% Matrices are the core data type
A = [1 2 3; 4 5 6];        % 2x3 matrix (rows separated by ;)
v = [1; 2; 3];             % column vector
row = [1 2 3];             % row vector

% Element-wise vs matrix operations
B = A .* 2;        % element-wise multiply
C = A * 2;         % scalar multiply (same here)
D = A .^ 2;        % element-wise power

size(A)            % 2 3  (rows, columns)
numel(A)           % 6
```

- `%` starts a comment; `;` at the end suppresses console output.
- The default data type is the double-precision matrix.
- `.^`, `.*`, `./` are element-wise; `*`, `^` are the matrix versions.

## Indexing

```matlab
A = [10 20 30; 40 50 60];

A(1, 2)        % 20   (first row, second column)
A(2, :)        % entire second row  -> 40 50 60
A(:, 3)        % entire third column -> [30; 60]
A(1, [1 3])    % first row, columns 1 and 3 -> 10 30

% Linear indexing counts column-major
A(2)           % 40  (column 1 of row 2)

% End keyword
A(end, end)    % 60  (last row, last column)
```

- MATLAB is 1-indexed; index with `(row, column)`.
- `:` selects all rows/columns; `end` refers to the last index.
- Index vectors can select arbitrary elements.

## Matrix Operations

```matlab
M1 = [1 2; 3 4];
M2 = [5 6; 7 8];

M1 + M2        % element-wise addition
M1 * M2        % matrix multiplication
M1'            % conjugate transpose (equal to .' for real inputs)
M1.'           % non-conjugate transpose
inv(M1)        % matrix inverse (prefer rref / \ for solving)
det(M1)        % determinant -> -2
sum(M1)        % column sums -> [4 6]
max(M1(:))     % overall max -> 4

% Solve linear systems M1 * x = b
b = [9; 12];
x = M1 \ b;    % preferred way to solve; avoid computing inv(M1) * b
```

- Matrix multiplication uses `*`; element-wise uses `.*`.
- `'` is the conjugate (Hermitian) transpose; `.'` is the plain transpose. For real matrices they are identical.
- `\` is the backslash operator that solves linear systems; prefer it over `inv(M1) * b`.
- `M1(:)` flattens into a column vector.

## Control Flow

```matlab
n = 10;

if n > 0
    disp('positive');
elseif n < 0
    disp('negative');
else
    disp('zero');
end

for i = 1:5
    disp(i);
end

i = 0;
while i < 5
    i = i + 1;
end

% Vectorized iteration over a vector
v = [2 4 6];
for val = v
    disp(val);
end
```

- `if`, `for`, and `while` end with `end`.
- Prefer vectorized operations over loops for performance.

## Functions

```matlab
% Save in file add.m
function result = add(a, b)
    result = a + b;
end

% Multiple outputs
function [s, p] = sumprod(v)
    s = sum(v);
    p = prod(v);
end

% Scripts vs functions
% Functions have local workspace; scripts share the base workspace
```

- A function file must start with `function` and its name must match the filename.
- Multiple outputs are returned in a bracket list.
- MATLAB uses pass-by-value semantics for function arguments.

## Anonymous and Built-in Functions

```matlab
% Anonymous function
square = @(x) x.^2;
square(4)          % 16

% Apply to arrays
v = [1 2 3];
arrayfun(@(x) x^2, v)      % 1 4 9
```

- `@(args) expr` creates an anonymous function.
- `arrayfun`/`cellfun` apply a function over an array.

## Vectors and Logical Indexing

```matlab
v = [1 5 3 8 2];

v > 3                  % logical mask -> [0 1 0 1 0]
v(v > 3)               % elements >3 -> [5 8]
find(v > 3)            % indices -> [2 4]
sum(v > 3)             % count of elements >3 -> 2

% Generate data
x = linspace(0, 10, 5);     % 5 points from 0 to 10
y = 0:2:10;                 % 0 2 4 6 8 10
```

- Logical masks select elements satisfying a condition.
- `find` returns the indices of true elements.
- `linspace` and `:` generate evenly spaced points.

## Plotting

```matlab
x = 0:0.1:2*pi;
y = sin(x);

figure;
plot(x, y, 'r-');
xlabel('x');
ylabel('sin(x)');
title('Sine wave');
grid on;

% Multiple lines and styling
hold on;
plot(x, cos(x), 'b--');
hold off;
```

- `plot`, `xlabel`, `ylabel`, and `title` drive basic line charts.
- `hold on` overlays subsequent plots on the current axes.
- `grid on` toggles gridlines.

## Strings and Text

```matlab
str1 = 'Hello';
str2 = 'World';
combo = [str1 ' ' str2];       % concatenation -> 'Hello World'
upper(combo)                   % 'HELLO WORLD'
length(combo)                  % 11
strsplit('a,b,c', ',')         % {'a','b','c'}
sprintf('%d apples', 3)        % '3 apples'
stringArray = ["Hello", "World"]   % string array (R2016b+)
```

- Use single quotes for character vectors; concatenation with `[]`.
- MATLAB also has string arrays (`"..."`, R2016b+). Prefer them for modern code.
- `upper`, `length`, and `sprintf` are text utilities.
- `%d` formats a number in `sprintf`.

## References

- [MATLAB Documentation](https://www.mathworks.com/help/matlab/)
- [MATLAB Language](https://www.mathworks.com/help/matlab/language-fundamentals.html)
- [MATLAB for Linear Algebra](https://www.mathworks.com/help/matlab/linear-algebra.html)