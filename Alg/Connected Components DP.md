# What
Counting the numbers of permutations with some characteristics.
# Time complexity
$$
O(n^2)
$$
# Explanation
We build permutations by sorting the values and adding them one by one.
## Counting permutations
$dp[i][j]$ is the number of ORDERED [[Set|sets]] made up of elements up until $i$, divided in $j$ components.

>For example if we have the elements up to $4$, $dp[3][2]$ could be $(1,2),(3)$/$(2,1),(3)$/$(1),(3,2)$/$...$

>We are imagining that we have infinite space, for example we could fill ??? with 3 __separated__ components.

When we insert the $i+1$th element 3 things can happen:
- We can create a new component with only $i+1$.
	- $(1,2),(3)$ __->__ $(1,2),(3),(4)$
	- $dp[i+1][j+1]=dp[i][j]*(j+1)$ Because we can place it in $j+1$ different positions (before a component or after the end).
- We can append $i+1$ to an existing component.
	- $(1,2),(3)$ __->__ $(1,2),(4,3)$
	- $dp[i+1][j]=dp[i][j]*(2*j)$, Because for each component we have $2$ possibilities (front or back).
- We merge 2 components by placing $i+1$ between them.
	- $(1,2),(3)$ __->__ $(1,2,4,3)$
	- $dp[i+1][j-1]=dp[i][j]*(j-1)$ Because there are $j-1$ spaces between components.

The final answer will be $dp[n][1]$, since we will add all of the elements and they will all be a part of a single component.

