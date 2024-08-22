#CP
# What 
Finding the max amount of flow that can be sent through a graph.
# Time Complexity
$$O(v*e^2)$$
# When
Whenever we have multiple paths from a start to an end.
# Logic
We have a directed graph, where each edge has a capacity.
We also have a source, that has to send a flow to a sink (the flow can never exceed the capacity).
>We can think of it as how much water can we send through pipes.

The value of the flow in a graph is the flow in the source/sink.


The remaining capacity of an edge is $remaining\_capacity=capacity-flow$.

## Reverse Edges
In the graph we also need to add reverse edges, with a capacity of 0.
If the normal edge is $(a,b)$ and Nhas a flow of $f(a,b)$, the reversed edge is $(b,a)$ and has a flow of $-f(a,b)$. 

Whenever we pass some flow into a reverse edge, we need to decrease the normal one. (This is like walking back the normal flow decisions)
 
Reverse edges still have a positive remaining capacity.
>For example if an edge has a flow of $-3$, $remaining\_capacity=0-(-3)$

So the remaining capacity of a reverse edge increases with flow.
# Algorithm
- At the start each edge has a flow of 0.
- We keep searching for paths that increase the flow (paths where every edge has a positive remaining capacity).
	- We find the shortest path with a BFS.
	- The flow along each path is equal to its bottleneck (minimum remaining capacity)
	- Update the normal and reverse edges according to the bottleneck.
- Once there are no more paths we stop.

# Example
This is the start 
![[Pasted image 20240607195850.png|400]]
We find the shortest path, with a bottleneck of 5:
![[Pasted image 20240607195910.png|400]]
![[Pasted image 20240607195917.png|400]]
After some updates:
![[Pasted image 20240607200023.png|400]]
We now find a reverse edge, it decreases the flow from A to D by one:
![[Pasted image 20240607200040.png|400]]
![[Pasted image 20240607200059.png|400]]
# Template
```C++
//the matrix holds the remaining capacity for every pair of vertices (capacity[u][v])
vector<vector<int>> capacity;
//adjacency list
vector<vector<int>> adj;

//s is the source, d is the drain, parent is used to store the path.
//We run this (and reset the parents) until we cant improve flow.
//This returns the improvment
int bfs(int s, int d, vector<int> &parent){
	fill(parent.begin(),parent.end(),-1);
	//this is used to avoid the sources parent being updated.
	parent[s]=-2;
	//queue for bfs
	queue<pair<int,int>> q;

	while (!q.empty()){
		//gets the first element in the queue
		int cur=q.front().first;
		int flow=q.front().second;
		q.pop();

		//for every node adjacent to the current one
		for (int next: adj[cur]){
			//if the node hasnt already been visited and theres capacity
			if(parent[next]==-1&&capacity[cur][next]){
				//sets the parent
				parent[next]=cur; 
				//The bottleneck is the minimum of the current flow and the capacity of the edge.
				int bottleneck=min(flow,capacity[cur][next]);
				//if we reach the drain
				if (next==d)
					return bottleneck;
				//we have multiple bottlenecks because the code runs differnt paths with different bottlenecks until one hits the drain.
				q.push({next,bottleneck});
			}
		}
	}
	//if it cant update anymore
	return 0;
}

int tot_flow=0;
vector<int> parent(n);
//newflow is the improvment
int new_flow;
//while newflow isnt 0.
while (newflow=bfs(s,t,parent)){
	tot_flow+=newflow;
	//starting from the drain computes the path, in order to update the capacity.
	int cur=d;
	while (cur!=s){
		int prev=parent[cur];
		//updates the normal direction
		capacity[prev][cur]-=new_flow;
		//updates the backwards one. We dont have negative numbers and just increase it, because we are storing the REMAINING CAPACITY.
		capacity[cur][prev]+=new_flow;
		cur=prev;
	}

}
```

# Applications
## Min cut
Remove a set of edges so that theres no path from the source to the sink and the weight of the edges is minimized.

The capacity of the max flow is equal to the capacity of the min cut.

To find the min cut edges we have to:
- Start from $s$.
- Run Edmonds-Karp.
- Run a dfs to find:
	- All edges from a reachable vertex (reachable from $s$) connected to a non reachable vertex.

A vertex is reachable if it has a remaining capacity larger then 0.

CPH chapter 20
## Edge disjoint paths
Given a graph, find the number of possible paths from the source to the sink. E ach edge can appear in at most one path.
![[Pasted image 20240607213205.png|400]]
Its the max flow of the graph where the capacity of each edge is one.
## Node disjoint paths
The same thing as above, but each __node__ can appear in at most one path.
![[Pasted image 20240607213346.png]]
We can divide each node into 2 parts. One has the incoming edges, the other the outgoing ones. There is one edge between them, so that each node can be only visited once.
![[Pasted image 20240607213356.png]]
# Unweighted Max bipartite matching.
Given a [[Bipartite Graph]], we need to create matches of vertices of different parities, such that each pair is connected by an edge.

We add 2 new vertices, a source and a drain.
We add edges from the source to the vertices of a color and from the vertices of the other color to the drain.

>The capacity of the edges from the source to the left nodes is $1$, because each left node can be matched with $1$ right node.
>The capacity of the edges to the source is $1$, because each right node can be selected $1$ time.
>The capacity of the intermediate edges is $1$, because each left node can select its pair $1$ time.

![[Pasted image 20240608002946.png]]
>Whenever we have an exclusive matching problem we need to formulate it as a bipartite graph.
## Min node cover/ Max independent set

The min node cover is the minimum amount of nodes, such that each edge in a graph touches at least one of them. 
![[Pasted image 20240608124810.png]]
In a bipartite graph its equal to the max matching.

All the nodes that arent a part of the min node cover are a part of the max independent set. 
The max independent set is the maximum amount of nodes such that no 2 nodes are connected.
# Path cover
A path cover is a set of paths such that each node belongs to at least one path. 

#### Node disjoint
In a node disjoint path cover each node can belong to max 1 path. 
>This means that we need to match each node to the next one in the path.

We construct a matching graph:
- We add a source and a drain.
- We connect all original vertices to the source.
- We duplicate all original vertices and connect them to the drain.
- We add an edge between a left node and a right node only if they are connected.
The size of the path cover is $n-max\_matching$.
#### Normal
We do the same thing, but we connect 2 nodes if there is a path between them.

This is also the max amount of nodes such that there isnt any path between them.

# Min cost flow
Each edge now also has a cost per unit of flow.
Find the lowest cost to transport the max flow/flow of a given size.

We define the cost of the reverse edge as the inverse of the normal cost, and then use Edmonds-Karp, but selecting the path with the lowest cost (calculated with [[Shortest path with negative weights|Bellman Ford]]).

>This code kinda sucks

```C++
vector<vector<int>> capacity;
vector<vector<int>> cost;
vector<vector<int>> adj;

vector<int> dist(n,INF);
vector<int> parent(n,-1);
const int INF=1e9;

//n is the number of nodes, s is the start
void bellman_ford(){
	dist.assign(n, INF);
	parent.assign(n,-1);
	dist[s]=0;
	vector<bool> inqueue(n,false);
	queue<int> q;
	q.push(s);
	

	while(!q.empty()){
		int v=q.front();
		q.pop();
		inqueue[v]=false;

		for(int to:adj[v]){
			//if the node is traversable and theres an update
			if (capacity[v][to]>0&&dist[to]>dist[v]+cost[v][dist]){
				dist[to]=dist[v]+cost[v][to];
				parent[to]=v;
				if (!inqueue[v]){
					inqueue[v]=true;
					q.push(v);
				}
			}
		}
	}
}
int find_flow(){
	int cur_flow=0;
	int cur_cost=0;
	while(cur_flow<MAXFLOW){
		bellman_ford();
		//if you cant reach/improve the drain stop
		if (dist[d]==INF)
			break;
	
		int allowed_flow=MAXFLOW-cur_flow;
		//starts at the drain and reaches the source
		int cur=d;
		while (cur!=s){
			//capacity[parent[cur]][cur] is the edge from the parent one to the current one
			allowed_flow=min(allowed_flow,capacity[parent[cur]][cur]);
		}
	
		cur_flow+=allowed_flow;
		//dist[d] is the total cost per unit of flow along the shortest path.
		cur_cost+=allowed_flow*dist[d];
		//starts from the drain and decreases capacity
		cur=d;
		while (cur!=s){
			capacity[parent[cur]][cur]-=f;
			capacity[cur][parent[cur]]+=f;
			cur=parent[cur];
		}
	}
	return cur_cost;
}
```