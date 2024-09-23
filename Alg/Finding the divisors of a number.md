# Time Complexity
$$
O(\sqrt n)
$$

# Logic
All of the divisors are in pairs:
>The divisors of 100 are:
1/100
2/50
4/25
5/20
10/10

So if we find the divisors $f_1,f_2,...$ up to $\sqrt n$, the rest of the divisors are just $N/f_1,N/f_2,...$ 


We also need to check that $f_i$ and $n/f_i$ are different to avoid recounting $\sqrt n$.