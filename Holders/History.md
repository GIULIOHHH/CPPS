# DP

# Matrix

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
### Main Takeaway


### Secondary Takeaway


### Solution

#### Logic


#### Implementation