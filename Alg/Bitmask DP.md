
## When
Whenever we have to do dp with "already completed things".

# Bit operations
We can represent a set of numbers through bits turned on or off.
>{$1,3,4$} is $11010$ because the bit at index 1 is set, same for index 3 etc.

We can use this to perform bitwise operations on sets.

$(1<<n)-1$ returns a number with the first $n$ bits set at 1.

$i\;|\;(1<<n)$ sets the $n$th bit as $1$.

~(not) converts all the 1s to 0s and viceversa.

>If we represent the prime divisors of a number with bits, `&` is the GCD `|` is the LCM.

# Examples
#### Floorboards
We have a $10*10$ grid filled with walls. We have to fill the grid with the minimum amount of vertical or horizontal floorboards.
![[Pasted image 20240609131239.png|200]]
- I can go from top to bottom (row by row).
- I hold one boolean that tells me if in my row theres an horizontal floorboard that i might want to extend.
- I use a bitmask that tells me where the vertical floorboards are in my row.
My dp will be $dp[bool][y][x][mask]$
For every given position:
- Can start a horizontal board
- Can start a vertical board
- Can continue a horizontal board
- Con continue a vertical board
#### Stamping
What is the minimum amount of times i have to stamp a cross to make a given image?
![[Pasted image 20240609132753.png]]
I can start top to bottom (i stamp from the top point of the cross), and use a bitmask the size of twice the row.
![[Pasted image 20240609132853.png]]
>I do this to save space because i dont care about whats behind me in the current row (already processed) and dont care about whats in front of be 2 columns down (couldnt have stamped it).

if the grid is $n*m$ the bitmask positions will be:
`       0   1    2   
`m-1    m   m+1  m+2  
`2m-1   2m           

Whenever i move on to the next position i can just rightshift the bitmask to get rid of the useless position.

Whenever i stamp i just have to set to $1$: $2^0$,$2^{m-1}$,$2^{m}$,$2^{m+1}$,$2^{2m}$.

#### Travelling Salesman
Starting at a vertex, find the shortest route that visits all others and returns in a position.
- Represent the visited nodes with 1s. 
- At each step pick the city with the min cost.
- Once we have all 1s add the distance from the current node to the start (pos 0)
```C++
int solve(int bitmask, int pos){
	//if we have already solved the subproblem return
	if (dp[bitmask][pos]!=-1)
		return dp[bitmask][pos];

	//if we have visited every city return the distance to go home
	if (bitmask ==(1<<N)-1)
		return dist[pos][0];

	//keep track of the minimum distance so far. Initialized to a big number
	int minDist=1e9+9;
	//for each unvisited city, see the cost to visit it next
	for (int k=0;k<N;k++){
		//check if the kth bit isnt set
		if (bitmask &(1<<k)==0){
			//set it
			int newbitmask=bitmask |(1<<k);
			//the newdistance is the distance to get to k and the distance in solving the rest starting at k.
			int newdist=dist[pos][k]+solve(newbitmask,k);
			minDist=min(minDist,newdist);
		}
	}
	//set the dp val and return
	return dp[bitmask][pos]=minDist;
} 
```
#### Hamiltonian paths
A Hamiltonian path visits each vertex ONCE.
If we have a subset of a path, we only care about if its an Hamiltonian path and where it ends. This means we can use dp to construct a solution from smaller hamiltionian paths.

>If we know that theres a hamiltionian path in the subset {$1,2$} that ends at 2, and i know that theres an edge between 2 and 4, i know that theres a hamiltionian path in the subset {$1,2,4$} ending at 4.

- We can use the same logic to count different Hamiltonian paths starting from the first city and ending at the last.

```C++
//amount of flights of subset s, ending at city d. 
long long dp[1<<20][21];
vector<vector<int>> adj;

//theres one way from the first city to the first city
dp[1][1]=1;
//iterates through all possible bitmasks up to the amount of cities
for (int s=2; s<(1<<n);s++){
	//skips the paths that include the last city and arent full (we want to end at the last city)
	if (s&(1<<n-1)&&s!=(1<<n)-1)
		continue;
	//we start at 1 because city 0 is always visited
	for (int city=1;city<=n;city++){
		//if the city is not in the subset change city
		if (s&(1<<(city-1))==0)
			continue
		//for each adjacent city to the one in the subset
		for (int v:adj[city]){
			//if the adjacent city is in the subset
			(if s&(1<<(v-1))){
				//add to the paths visiting s and ending at city the path not visiting city and ending at v.
				dp[s][city]+=dp[s-(1<<(city-1))][v];
			}
		}
	}
}
cout<<dp[(1<<n)-1][n]
```

#### Feedback arc set
I have to remove the min amount of edges from a graph in order to remove cycles.

If i turn the graph into a [[Directed Acyclic Graph]] it could have a working topological order.

Given a working topological order we know that all edges from a node point right to its children, so if i try to construct a topological ordering from left to right the cost will be the nodes that point left.
![[Pasted image 20240609140816.png]]
If i have some nodes that i know are in order, i only care about their cost, so we can construct the solution using dp.

I represent the nodes in the current set with a bitmask.
I also use an adjacency matrix with bitmasks.
So whenever i add a new node i can just `&` the 2 in order to get the nodes pointing left.
>I can also store the and of all possible bitmasks with the adjacency matrix in order to speed this up

the dp will be $dp[bitmask]$ and it will be the minimum cost to solve the nodes in the bitmask.

We can now do this algorithm:
- Iterate through all possible bitmasks
	- Loop over each index
		- If its a 0, the cost to make it a 1 is the dp of this bitmask plus all of the backwards connections.
		- Update the dp.
#### Sum Over subsets
Given an array, print the sum over all subsets.
We define $dp[bitmask][x]$ as the sum of the subsets of the bitmask, so that they're different only in the first $x$ bits (0 indexed).
>For example $dp[10011][1]$ can be different only up until index $1$, so only $11$ can change and $100$ has to stay the same. Its equal to
>{$10011$+$10010$+$10001$+$10000$}

>The $x$th bit is the last bit that can be different.

We have a generic dp state.
If the $x$th bit is 0, this means that i cant turn it on, so this state is equal to the one where i dont have control over the $x$th bit.
$$
dp[bitmask][x]=dp[bitmask][x-1]
$$

If the $x$th bit is 1, this means i can turn it on or off, so this state is equal to:
- The times where i cant turn it on, aka i remove it from the bitmask and decrease $x$ by 1.
- The times where its fixed as 1, aka i leave it in the bitmask but decrease $x$ by one, so that all subsets will be forced to leave it on.
$$
dp[bitmask][x]=dp[bitmask-(2<<x)][x-1]+dp[bitmask][x-1]
$$
