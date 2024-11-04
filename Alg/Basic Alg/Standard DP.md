# What


# Time complexity
Time complexity depends on the number of dimensions:
1. If its a one-dimensional dp of size $[n]$, its $O(n)$
2. If its a two-dimensional dp of size $[n][m]$ its $O(n*m)$
3. If its a three-dimensional dp of size $[n][m][j]$ its $O(n*m*j)$

>Mathematically its
>$$O(\prod^d_{i=1} n_i)$$

# When
- A problem **requires** a [[Bruteforce]] solution
- There are overlapping subproblems.
- A subproblem can be defined in 3 or less states.