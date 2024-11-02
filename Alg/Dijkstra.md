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
![[Pasted image 20241030173336.png]]
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





### Redone template
Dense
```C++
  
const int INF=1e9;  
//for each vertex stores a vector, containing pairs representing edges.  
//Each pair is (edge_cost, edge_end)  
vector<vector<pair<int,int>>> adj(n);  
  
// Distance from the start (0) to every other vertex.  
vector<int> dist(n,INF);  
  
//Boolean array to mark if a node has been visited  
vector<bool> visited(n,false);  
  
//the distance from 0 to itself is 0  
dist[0]=0;  
  
//since we want the distance from 0 to every other node we run an outer for loop |O(n)|  
for (int i=0;i<n;i++)
{  
  
    //out of all the reachable edges we need to pick the one with the lowest distance.  
    //we initialize it as -1 because we still have to pick.    
    int v=-1;  
  
    //we need to check the distance to all edges in order to pick the minimum |O(n^2)|  
    for (int j=0;j<n;j++)
    {  
        //we need to check that we havent already visted v and that its the smallest so far.  
        if (!visited[j]&&(v==-1||dist[j]<dist[v]))  
            v=j;  
    }  
  
    //now we visit v.  
    visited[v]=true;  
  
    //now we need to update the distance to the neighbours of v. |O(n^2)|  
    for (pair<int,int> neighbour:adj[v])
    {  
        //the edge cost  
        int cost=neighbour.first;  
        //the edge target  
        int to=neighbour.second;  
  
        //the distance to get to the neighbour is the minimum between: itself and  
        //the distance to get to v+the edge cost.        
        dist[to]=min(dist[to],dist[v]+cost);  
    }  
  
}
```

Sparse 
```C++

const int INF=1000000000;  
  
// for each vertex we keep an array with its edges.  
//Each pair is (edge_cost, edge_destination).  
vector<vector<pair<int,int>>> adj;  
  
//a vector storing the distances from the start to each vertex  
vector<int> dist(n,INF);  
  
//we do not need visited because we visit a vertex only when there is an improvemnt to its distance.  
  
//we use a set to store the distances in order to get the shortest one in |O(1)|.  
//we do n insertions in logn time, for a total of |O(nlogn)|  
set<pair<int,int>> priority;  
  
//we insert to the priority queue the first vertex (with cost 0).  
priority.insert({0,0});  

//the distance from 0 to itself is 0  
dist[0]=0;  
  
//loop to visit each vertex |O(n+nlogn)|  
for (int i=0;i<n;i++){  
    //we need to pick the lowest distance vertex. We get the second int (destination) of  
    // the first element of the priority queue    int v=priority.begin()->second;  
  
    //we then delete the first element.  
    priority.erase(priority.begin());  
  
    //we need to update the other distances.  
    for (pair<int,int> edge:adj[v]){  
        //for edge we get the cost and destination  
        int cost=edge.first;  
        int to=edge.second;  
        //if we lower the distance to the destination:  
        if (dist[to]>dist[v]+cost){  
            //first we want to remove the old distance from the priority queue |O(logn)|  
            //if its not there it does nothing            
            priority.erase({cost,to});  
  
            //we update the distance  
            dist[to]=dist[v]+cost;  
            //we add the new distance |O(logn)|  
            priority.insert({dist[to],to});  
  
        }  
    }  
}
```