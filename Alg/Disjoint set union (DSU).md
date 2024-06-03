#CP 
# What 
Putting nodes into trees, that can be quickly merged, and allow checking quickly if 2 nodes are connected.
# Time Complexity
$$
O(1)
$$
# Algorithm
In each tree the root is the representative, that is used to check if 2 elements are part of the same set.

A DSU supports 2 operations:
- __Union__ joins 2  sets
- __Find__ returns the representative of an element.

Always connect the representative of the smaller set to the larger one. (you can do either shorter tree or smaller tree it doesnt matter)

We also use lazy propagation, so when uniting we only change the representative of the smaller node.
# Template

```C++
//every set has 1 node in the beginning (size 1)
int size[n];
fill(size.begin(),size.end(),1);
int parent[n];
//in the beginning every node is the parent of itself
for (int i=0;i<n;i++){
	parent[i]=i;
}
```

```C++

int find(int n){
	//if we reach a representative return it
	if (parent[n]==n){
		return n;
		}
	//otherwise get it recursively
	return parent[n]=find(parent[n]);
}

void union(int a,int b){
	a=find(a);
	b=find(b);
	//if theyre different
	if (a != b){
		//if a is smaller swap them
		if (size[a]<size[b])
			swap(a,b);
		//a is now the parent of b
		parent[b]=a;
		//increase the size of the a component
		size[a]+=size[b];
	}
}
```

# Example
We have 2 sets `[1,2,3]` and `[4,5]`.
The representatives are `[1]` and `[4]`.
We want to join the 2 sets.
The second is smaller, so we join it.
The representative of `[4]` is no longer itself, but is now `[1]`.

We now want to find the representative of `[5]`. 
The representative of `[5]` is still `[4]` (we only updated the representative of `[4]`).
We will now recursively get  the representative of `[4]` which is `[1]`.
`[1]` is the representative of itself so now were done. We set the representative of `[5]` to `[1]`.

# Applications
Check for connected components in an online graph.
# Painting subarrays
We have an array. We paint the subarray $[L,R]$ with color $c$ for each query. We want the final color of each cell.
We can solve the queries in reverse order, this way when we paint a cell it already has its final color. 

For each cell we also maintain the closest unpainted cell.
```C++
for (int i=0;i<n;i++){
	parent[i]=i;
}


int find(int n){
	//if we reach a representative return it
	if (parent[n]==n){
		return n;
		}
	//otherwise get it recursively
	return parent[n]=find(parent[n]);
}

for (int i=queries-1;i>=0;i--){
	int l = query[i].l; 
	int r = query[i].r; 
	int c = query[i].c;
	//we start by finding the cloeset unpainted cell to the left bound.
	//After we get it we move on to the next unpainted cell.
	for (int v=find(l);v<=r;v=find(v)){
	color[v]=c;
	//the closest unpainted cell to the current one is the adjacent one (lazy propagation)
	parent[v]=v+1;
	}
}
```
## Get distance to representative
We store for every node the distance to the representative using pairs.
## Check for online graph bipartiteness.
We start with an empty graph where we add edges. We also have to answer queries that ask if the connected component containing a vertex is [[Bipartite Graph|bipartite]].

For each vertex we can store the parity of the path from the representative. 

If an edge $(a,b)$ connects 2 components, we use a formula to get the parity of the leader of the smaller set.

$$
parity= len_a\oplus len_b\oplus 1
$$
Where $len_a$ is the length from $a$ to the representative, the same for $len_b$, and $1$ is the length from $a$ to $b$.

We can also store an array for the bipartiteness of the components.
```C++
//path compression
pair<int,int> find(int v){
	//if we dont have a representative
	if (v!=parent[v].first){
		//the parity of the parent
		int parity= parent[v].second;
		//sets the parent to the representative
		parent[v]=find(parent[v].first);
		//calculates the parity of the path.
		parent[v].second ^=parity;
		}
	return parent[v];
	}

void union(int a,int b){
	//gets the representative and the parity
	pair<int,int> pa =find_set(a);
	
	//a is the representative
	a=pa.first;
	//x is the parity of the original node
	int x=pa.second;
	
	pair<int, int> pb = find_set(b); 
	b = pb.first; 
	int y = pb.second;

	//if the representatives are the same
	if (a==b){
		//and the parities are the same
		if (x==y){
			//the graph isnt bipartite
			bipartite[a]=false;
		}
		//otherwise join 
		else{
		
			if(size[a]<size[b])
				swap(a,b);
			
			parent[b]=make_pair(a,x^y^1);
			//both need to be bipartite
			bipartite[a]&=bipartite[b];
			size[a]+=size[b];
		}
	}
}
```

## Undirected Cycle Detection
For each edge $(u,v)$, if $u$ and $v$ are in different subsets join them, otherwise if they are in the same one a cycle has been found.

# Alternative version
https://cp-algorithms.com/graph/bridge-searching-online.html
## Template
