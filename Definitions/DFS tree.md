By using a DFS from node $n$ we can turn any graph into a tree rooted at node $n$.
![[8dbe5d89e58b67f3d8e4d8e0e8eb3358ba921b28.gif]]
Whenever we visit a node we set it as a child of the vertex we just came from.

This is the resulting tree:
![[Pasted image 20240623214611.png]]
The bold edges form a spanning tree (They are called span edges).
Each other edge connects a vertex with an descendant(They are called back edges).

# Finding bridges
A span edge $e$ is a bridge if theres no back edge that connects a descendant of $e$ with an ancestor of $e$.
>If theres no back edge that passes over $e$. 
>Because then we can remove the edge and split the graph in 2.

So given a graph:
- Find the DFS tree. 
- For each span edge check if theres a back edge.
	- given a vertex $u$, $dp[u]$ is the number of edges that cross the edge between $u$ and its parent.

We might need to fix this

$dp[u]$ is equal to:

The edges that go up from $u$
![[Pasted image 20240625003406.png|400]]
The edges that cross over a span edge between $u$ and one of its children,
![[Pasted image 20240625003511.png|400]]
EXCEPT FOR THE ONES ENDING AT $u$.
![[Pasted image 20240625003643.png|400]]
$dp[u]=(edges\;up\;from\;u)+\sum_{over\;all\;children\;v}(dp[v])-(edges\;down\;from\;u)$

# Finding articulation points
![[Pasted image 20240625001702.png]]
