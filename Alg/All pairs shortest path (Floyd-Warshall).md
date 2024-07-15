#CP 
# What 
Find the length of the shortest path for each pair $i$,$j$.

# Time Complexity
$$O(n^3)$$
# Algorithm
We store a matrix $d[\;][\;]$ for distances.

- We run the first for loop $n$ times, with a variable called $k$. The third and second for loops iterate over each pair $(i,j)$.
	- On the $k$th iteration, $d[i][j]$ is the min distance if the path can enter only vertices smaller then $k$ (except for $i$ and $j$)
- On $k=0$ $d[i][j]$ is $\infty$ if theres no edge. (otherwise we just put $e_{i-j}$)

If we are in the $k$th phase and need to calculate the $k+1$th for $i,j$ there are 2 cases:
1. The shortest path of vertices up to $k+1$ is the same as the one for vertices up to $k$. 
2. The shortest path passes through $k+1$. We can split the new paths into 2: The first between $i$ and $k+1$ and the second between $k+1$ and $j$. 
	1. Both paths only use vertices up to $k$.
So the formula is 
 $$d[i][j]=min(d[i][j],d[i][k+1]+d[k+1][j])$$

---
If we want the ancestors, we need another matrix $a[\;][\;]$ where we store the $k$ when $d[i][j]$ was last modified.
Now the shortest path is the union between the path from $i$ to $a[i][j]$ and from $a[i][j]$ to $j$.
# Template
>Its really quick to code :)
```C++
//outer k loop
for (int k=0;k<n;k++){
	//iterate over every pair
	for (int i=0;i<n;i++){
		for (int j=0;j<n;j++){
			//calculates d[i][j] for k=1.
			d[i][j]=min(d[i][j],d[i][k]+d[k][j]);
		}
	}
}
```
>If there are negative edges we just need to add the check
> `if (d[i][k]<INF&&d[k][j]<INF`.

