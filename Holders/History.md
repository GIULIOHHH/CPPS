# DP

# Matrix
## Counting digits
Given a number, for each operation replace each digit n with $n+1$.
>If we have $192$ we get $2103$.
### Text
__Difficulty: Easy__
__Time Complexity: $O(n*logn)$__

### Methods
- Matrix exponentiation
- Sorting
### Main Takeaway
If our matrix is raised to the $n$th power we can raise it to the $n+m$ power by multiplying it with one raised to the $m$th power.

>If we have a matrix to the $6$th power and we want to get it to $15$ we can just multiply it by one raised to the $9$th.

We can use this in conjunction with sorting queries to answer them efficiently.

>If our queries are  $5$ and $24$, once we have already computed $5$ we just have to multiply it by $19$ to get $24$.
### Solution
We have a $10*10$ matrix, where each index is the number of digits in the number.
>For example there are $3$ $0$s, $2$ $1$s etc.

When counting the answer, for each digit in the original number we sum up the others.

>Because starting at $n$ we are summing up all possibilites.
# Flow

# Ad-Hoc

# Segtree

# Traversal


# Divisibility
## Subarray divisibility
### Text
__Difficulty: Normal__
__Time Complexity: $O(n)$__

Given an array and a number $k$ ($2019$), find the number of subarrays divisible by $k$.
### Methods
- Suffixes, where each suffix stores the number up until $i$.
	- Each suffix is the new letter to add.
	- We do $prev+new*10^j$.
### Main Takeaways
NUMBER 1
If $a$ and $b$ are coprime and $a$ divides $c$, $a$ also divides $b*c$.
>Since $13$ and $10$ are coprime, $13$ divides both $26$ and $260$.
>$5$ and $10$ arent coprime, so $5$ doesnt divide $1$ but divides $10$.

So if we have $123$ and we get the suffix $120$, we can just treat it as $12$.

NUMBER 2
If $i$ has a remainder of $x$ and $j$ has a remainder of $y$, $i-j$ has a remainder of $x-y$.

If we have a prefix $pref[i]$, with a remainder $r$, we can search for all the previous prefixes $pref[j]$, so that they have the same remainder. (so that they cancel out and we get $0$)
### Secondary Takeaways
- Sometimes suffixes are easier to code then prefixes.
### Solution
#### Logic
1. We want the subarrays to equal $0$ when $MOD \;2019$.
2. To calculate the subarray $(i,j)$, we do $suf[i]-suf[j]$.
3. For every subarray we add to the solution the number of previous subarrays with the same modulo.
#### Implementation
- We store the number of subarrays with a given modulo in a vector.
## Ones and Zeroes multiple
Given a number, finds its smallest multiple made up only of $1$s and $0$s.
### Text
__Difficulty: Easy__
__Time Complexity: $O(n)$__

### Methods
- Digit DP
- BFS
### Main Takeaway


### Secondary Takeaway


### Solution

#### Logic
