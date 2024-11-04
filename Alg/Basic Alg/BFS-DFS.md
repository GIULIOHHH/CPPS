# What 
2 ways to traverse a graph.

# Time Complexity
$$
O(v+e)
$$
>where $v$ is the number of vertices and $e$ the number of edges.
# When 
They are used **whenever** a problem can be represented with multiple states that need to be visited.

>[!Note] 
>**Almost all problems** can be represented as a series of steps from the start to the end, and thus as a graph (that can have one or more dimensions).
>
# BFS
## Description
Starting from a node $s_0$, it first visits all children of $s$, $s_1$. 
It then visits all children of $s_1$ and so on.

## Algorithm
BFS works with a [[Queue]].
1. Append the starting node to the queue.
2. While the queue is not empty:
	1. Pop the first element.
	2. Mark it as visited
	3. Add to the queue all children of that element (if they are not visited).

>[!info]
>When all edge weights are equal, BFS can be used as an algorithm for finding distances from an origin. 
>
>This is equivalent to a simplified [[Dijkstra]], as the queue stores the nodes in order to the distance from the source, and node distances do not need to be updated because every edge is the same length.

![[Breadth-First-Search-Algorithm.gif|400]]
# DFS
## Description

[[Stack]]
![[0_r5blxPoPZaX1OkGr.gif]]



# On trees
Whenever the graph we are dealing with is a [[Tree]], we do not need to check if a node has been visited.
