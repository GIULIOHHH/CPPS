#CP
# What 
Fastest algorithm for shortest path from a vertex to all others.

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

We can stop the algorithm as soon as the processed vertex has a distance of $\infty$.

# Storing the path
We store an array of predecessors, where $p[v]$ is the penultimate vertex in the shortest path from $s$ to $v$. If we remove $v$ from the path we get the shortest path for $p[v]$.
We can keep doing this to get the path.

Whenever we update $dist[to]$, we update $p[to]=v$.
# Template
## Dense Graphs
We check the min $dist$ value by iterating through the array.
```C++
const int INF=1000000000;
//For each vertex, holds a vector containing pairs. The first value of the pair is the destination, the second is the cost.
vector<vector<pair<int,int>>> adj;

vector<int>dist(n,INF);
vector<int>prev(n,-1);
vector<bool>visited(n,false);

//s is the start.
dist[s]=0;
//the main loop that runs n times.
for (int i=0;i<n;i++){
	//selects the lowest cost vertex.
	int v=-1;
	for (int j=0;j<n;j++){
		//if the vertex isnt visited and its the smallest so far
		if (!visited[j]&&(v==-1||dist[j]<dist[v]))
			v=j;
	}
	//if the min dist is infinite break.
	if (d[v]==INF)
		break;

	visited[v]=true;
	for (auto edge:adj[v]){
		int to=edge.first;
		int cost=edge.second;

		if (dist[v]+cost<dist[to]){
			dist[to]=dist[v]+cost;
			prev[to]=v;
		}
	}
}
```
## Sparse Graphs
- We use a set to select the smallest distance.
- We dont need visited anymore.
	- The code doesnt create loops because it adds an element to the set only if it improves its distance.
```C++
const int INF=1000000000;
vector<vector<pair<int,int>>> adj;

vector<int>dist(n,INF);
vector<int>prev(n,-1);
//q stores the pair distance, vertex
set<pair<int,int>> q;
//distance 0, vertex s
q.insert({0,s});

while (!q.empty()){
	//q.begin is the smallest val. We use the arrow because q.begin is a pointer.
	int v=q.begin()->second;
	//removes the smallest val.
	q.erase(q.begin());
	for (auto edge: adj[v]){
		int to=edge.first;
		int cost=edge.second;
		if (dist[v]+cost<dist[to]){
			//if its in the queue removes the slower edge
			q.erase({d[to],to});
			dist[to]=dist[v]+cost;
			prev[to]=v;
			//after updating adds the faster one.
			q.insert({d[to],to});
		}
	}

}

```
