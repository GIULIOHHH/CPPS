#CP
# What 
Shortest path from a vertex to all others.
Works with negative edges and negative cycles.

# Time Complexity
$$
O(v*e)
$$
# Algorithm
We start at a vertex $s$.
- Create an array $dist$ where for each vertex we store the shortest path from $s$.
	- The distance from $s$ to $s$ is 0.
	- All others are initially $\infty$.
- Create a queue and add $s$.
	- Create a boolean array where we store for each vertex if they are in the queue.
- We run this until the queue is empty:
	- Pop the first element of the queue.
	- For every edge ($v$,$to$), update $dist[to]=min(dist[to],dist[v]+edge)$.
	- If we update $dist$ we put $to$ in the queue.
	- If a vertex is updated more then $n$ times there is a negative cycle.

# Template

```C++
const int INF = 1000000000;
//for every vertex holds a vector containing pairs. The first value of the pair is the destination, the second is the cost.
vector<vector<pair<int,int>>> adj;
vector<int> dist(n,INF);
//the times each vertex has been updated.
vector<int> count(n,0);
//if a vertex is the queue.
vector<bool> inqueue(n,false);
queue<int> q;

dist[s]=0;
//adds the start to the queue
q.push(s);
inqueue[s]=true;
while(!q.empty()){
	//gets the first element
	int v=q.front();
	//removes the first element
	q.pop();
	//the element isnt in the queue anymore
	inqueue[v]=false;
	for (auto edge: adj[v]){
		int to=edge.first;
		int cost=edge.second;
		//if we can improve the distance
		if (dist[v]+cost<dist[to]){
			dist[to]=dist[v]+cost;
			//if to isnt in queue
			if(!inqueue[to]){
				q.push(to);
				inqueue[to]=true;
				//to has been updated once.
				count[to]++;
				//if it has been updated more then n times
				if (count[to]>n)
					return false; //NEGATIVE CYCLE!
			}
		}
	}
}
```

# Finding the negative weight path
- Store the penultimate vertex on the shortest path for each vertex.
- Starting from a vertex $v$ that has been updated $n$ times:
	- Pass through the predecessors $n$ times.
	- We will get to a vertex $y$, which is the start of the cycle.
	- Pass through the predecessors once you get to $y$ again.
```C++
vector<int> find_path(int x){
	int y=x;
	for (int i=0; i<n; i++)
		y=prev[y];
	
	vector<int> path;
	for (int cur=y;;cur=prev[cur]){
		path.emplace_back(cur);
		if (cur==y&&path.size()>1)
			break;
	reverse(path.begin(),path.end());
}
```