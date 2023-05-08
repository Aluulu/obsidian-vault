# Finding a function

You can find the function by simply replacing `x` with the given value of `x` and then working out/simplifying the new function using BODMAS.

## Example

```Math
f(x) = 2x^3 + 4
Let f(x) = f(7)

Replace x with the new expression
f(7) = 2(7)^3 + 4

Using BODMAS, work out the expression
f(7) = 2(343) + 4    // 7 times 7 times 7
f(7) = 686 + 4    // 2 times 343
f(7) = 690    // Added the 4

Since no more calculations or simplifcations can be done. That's it.
```

```Math
f(x) = 2x^3 + 4
let f(x) = (-0.5)

Replace x with the new expression
f(-0.5) = 2(-0.5)^3 + 4

Using BODMAS, work out the expression
f(-0.5) = 2(-0.125) + 4    // -0.5 times -0.5 times -0.5
f(0.5) = -0.25 + 4    // -0.125 times 2
f(-0.5) = 3.75    // -0.25 add 4

Since no more calculations or simplifcations can be done. That's it.
```

# Finding funtion of function (g of f)

To find a function of another function, you have to replace `x` with the other function of the expression. For example `g` of `f`; in this situation `f` goes where the `x` would be in the function of `g`.

## Examples

```Math
f(x) = 4x + 2
g(x) = x + 3

In this case, multiplying g with f is easier
g(x) = x + 3
Would be
g(4x+2) = (4x + 2) + 3

Using BODMAS nothing can be done in the brackets so they are removed
4x + 2 + 3

Simply the equation by adding together items
4x + 5

Since nothing else can be done, that's it.
```

# Corresponding expression

This is working out and simplifying the expression down. To do this we replace `x` with the expression given and then work out the new expression.

## Example

```Math
f(4x + 2) − f(x) when f(x) = x^2

Replace the x with our newly given expression
f(4x + 2)^2 - f
(x)^2

Now we work out the equation using BODMAS
(4x + 2)^2 = (4x + 2) times (4x + 2) = 16x^2 + 8x + 8x + 4
	4x times 4x = 16x^2
	4x times 2 = 8x
	4x times 2 = 8x
	2 times 2 = 4

x^2 = x times x = x^2

Putting these together using the original equation
16x^2 + 8x + 8x - x^2

Simply it
	16x^2 - x^2 = 15x^2
15x^2 + 16x + 4
```

# Inverse

To do an inverse, you have to make x the sole remaining part of the right-side of the equation.

You do this by replacing the function `f(x)` with `Y`.

Then you remove numbers from the side with `X`. By doing this you must also do the opposite to the other side.

Then you put the equation of `f^-1` in by replacing `Y` with `X`

## Examples

```Math
f(x) = 7 + 5x

Replace f(x) with Y
Y = 7 + 5x

Using BODMAS, remove numbers from the right side to the left side
Y-7 = 5x    // Minus 7 from both sides
(Y-7)/5 = x    // Divide by 5

Now that x is alone we use f-1
f^-1(x) = (x-7)/5
```

```Math
f(x) = 3x

Replace f(x) with Y
Y = 3x

Using BODMAS, remove numbers from the right side to the left side
Y/3 = x    // Divide by 3

Now that x is alone we use f-1
f^-1(x) = x/3
```

# Matrices

>[!Important]
>The following tables may not work on another markdown reader such as GitHub's own markdown viewer. View it in Obsidian for the best viewing experience

## Transpose of a matrix

To find a transpose of a matrix, you must simply swap the rows and the colomns so that the rows become colomns and the colomns become rows.

### Example

```Math
A = [1 3 4]
    [2 3 2]

Finding the transpose is simply swapping the rows and colomns
     [1 2]
At = [3 3]
     [4 2]
```

```Math
    [1 5 -1]
B = [3 0  4]
    [1 1 -2]

Swap the rows and colomns
     [1  3  1]
Bt = [5  0  1]
     [-1 4 -2]

The first row of [1 3 1] now became the first colomn.
The second row of [3 0 4] now became the second colomn
The third row of [-1 4 -2] now became the third colomn
```

## Product of Matrices A and B (AB)

To find the product of two matrices (or AB), you will need to do matrix multiplcation.

The two matrices will have two sides. 
- Matrix A in this case will have `m` and `n`.
- Matrix B will have `n` and `p`
The product of A and B will be `m` times `p`

### Example

```Math

    [1 5 −1]          [1 -1 3]
A = [3 0 4]       B = [1 -2 1]
    [1 1 −2]          [2 3 -2]

Calculate the first Row of A with the first Colomn of B
AB[1,1] = A(R1C1) times B(R1C1) = 1 times 1
AB[1,1] = A(R1C2) times B(R2C1) = 5 times 1
AB[1,1] = A(R1C3) times B(R3C1) = -1 times 2
= (1 x 1) + (5 x 1) + (-1 x 2)
     ^         ^          ^
     |         |          |
     1    +    5    +    -2   = 4
So AB[1,1] = 4

Calculate the first Row of A with the second Colomn of B
AB[1,2] = A(R1C1) times B(R1C2) = 1 times -1
AB[1,2] = A(R1C2) times B(R2C2) = 5 times -2
AB[1,2] = A(R1C3) times B(R3C2) = -1 times 3
= (1 x -1) + (5 x -2) + (-1 x 3)
     ^         ^           ^
     |         |           |
     -1    +  -10     +    -3  = -14
So AB[1.2] = -14

This continues until every point in the matrices are done.
AB[1,1] is A first row times B first colomn
AB[1,2] is A first row times B second colomn
AB[1,3] is A first row times B third colomn
AB[2,1] is A second row times B first colomn
AB[2.2] is A second row times B second colomn
AB[2,3] is A second row times B third colomn
AB[3,1] is A third row times B first colomn
AB[3,2] is A third row times B second colomn
AB[3,3] is A third row times B third colomn
```