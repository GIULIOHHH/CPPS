# What
Finding the lowest common ancestor between 2 vertices.

# Preprocessing
## Time complexity
Construction in
$$O(n)$$
Queries in 
$$O(logn)$$

## Algorithm
Starting from the root we need to make a dfs traversal of the tree.
We create a list were we store each vertex when we first visit it and after the return of the dfs to each one its children.

We also store in $first$ the first time each node appears in the list.

We also store the height of each node in $height$.

![[Pasted image 20240625111718.png]]

We now need to find the vertex with the smallest height in the range from $first[v_1]$ to $first[v_2]$. 
We can solve this with a segment tree.

