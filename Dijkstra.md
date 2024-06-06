#CP
# What 
Shortest path from a vertex to all others.

# Time Complexity
For [[Sparse vs Dense graph|Sparse graphs]]:
$$
O(elog(v))
$$
For [[Sparse vs Dense graph|Dense graphs]]:
$$
O(v^2)
$$
# When 
Whenever we have non-negative weights.
# Algorithm

## Dense Graphs
We start at a vertex $s$.
- Create an array $dist$ where for each vertex we store the shortest path from $s$.
	- The distance from $s$ to $s$ is 0.
	- All others are initially $\infty$.
- Create a boolean array where we store for each vertex if they have been visited.
- We run this $n$ times:
	- Select the vertex $v$ with the minimum $dist$ value.
	- Mark it as visited.
	- For every edge ($v$,$to$), update $dist[to]=min(dist[to],dist[v]+edge)$.
Whenever we reach a vertex $v$, we know FOR SURE that we have found the shortest path from $s$.
# Template