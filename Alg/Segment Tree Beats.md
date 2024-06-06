#CP
# What 
Solving complex range queries by storing extra information at each node and using them to break early.

# Time Complexity
$$
n*log^2(n)
$$

# When 
Lazy propagation where a constant time update of a node isn't possible.
# Logic
## Problem
Usually with normal lazy propagation, we update the given vertex in $O(1)$, pass off the lazy value to the children and break. 
>For example the vertex stores the sum and we set all of its nodes equal to $x$, we can quickly recalculate its value by doing $x$ multiplied by the number of values.

There are some operations where its not clear how the value of a node would change:
- MOD each element by $x$
- Divide each element by $x$
- Replace each element $i$ with $max(i,x)$.
## Segment Tree Logic
When updating nodes we check 2 conditions before going to the children:
- __BREAK__ `(lq>rq)`: 
	- The node lies outside the range we need to update.
	- We stop without changing anything.
- __CHANGE__ `(lq==lb&&rq==rb)`: 
	- The node perfectly matches the range we want.
	- We can update the node without looking at the children.

With segment tree beats we store extra information for each node. 
We then add new conditions, both for __BREAK__ and for __CHANGE__.

# Example
## MOD updates
We need to:
- Query the sum of a given range.
- MOD all elements in a given range by $x$.

If all elements are smaller then $x$ the MOD wont do anything:
- We store the max element for each node
- We update __BREAK__ to be `(lq>rq||max_element<x)`.

If all of the elements are equal we can calculate the result without going to the children:
- We store the min element for each node.
- We update __CHANGE__ to be `(lq==lb&&rq==rb&&max_element==min_element)`
- We update the value of the node.
# Min updates
We need to:
- Query the sum of a given range.
- Replace each element $i$ with $min(i,x)$

If all the numbers are smaller then $x$ nothing will change:
- We store the maximum for each node.
- We update __BREAK__.

We now need to update __CHANGE__:
- We store the number of elements equal to the max and the biggest value different then the max.
- If $x$ is between the second and the first max, only the elements equal to the first max will change.
- We can update the value by subtracting $max*frequency$ and adding $x*frequency$.

If we had to perform first $min=(x)$ and then $min=(y)$ for a range, we can just perform the smaller of the 2.