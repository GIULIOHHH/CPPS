Whenever i have a [[Directed Acyclic Graph]], i can find a topological ordering, aka every node is visited before its ancestors.

>Graphs can have multiple topological orderings.

![[Pasted image 20240609135511.png|400]]
# Algorithm
- For each vertex, if its not already visited do a DFS, where we visit all reachable nodes from it.
- At the end of the DFS mark the node as visited and add it to the topological order.
- Reverse the list.
>This will ensure topological ordering because a nodes children will appear in the list before it.