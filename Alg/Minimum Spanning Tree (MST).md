#CP
# What 
The graph connecting all vertices with the least weight.

The graph will always contain $V-1$ edges.
# Time Complexity
Kruskal
$$
O(e*log(v))
$$
Prim
$$
O((e+v*log(v))
$$
# When 
In most situations Kruskal is faster (and quicker to code), but we use Prims for very [[Sparse vs Dense graph|dense graphs]].

![[Pasted image 20240530195139.png]]
# Kruskal
- Start with each vertex in a [[Disjoint set union (DSU)|Disjoint Set]].
- Sort the edges.
- Repeat this:
	- Consider the next shortest edge:
	- Check if the vertices are in different sets:
	- If so join them.
```C++

int find_set(int v){
	if (parent[v]==v){
		return v;
	}
	return parent[v]=find_set(parent[v]);
}

void unite(int a, int b){
	a=find_set(a);
	b=find_set(b);
	if (a!=b){
		if (size[a]<size[b]){
			swap(a,b);
		}
		parent[b]=a;
		size[a]+=size[b];
	}
}

struct Edge{
int u,v,weight;
bool operator < (Edge const &other){
	return weight<other.weight;
}
};

//number of vertices
int n;
int parent[n];
int size[n];

int cost=0;
vector<Edge> edges;
vector<Edge> result;

for (int i=0;i<n;i++){
	parent[i]=i;
	size[i]=i;
}
sort(edges.begin(),edges.end());

//main algorithm
for (Edge e: edges){
	if (find_set(e.u)!=find_set(e.v)){
		cost+=e.weight;
		result.emplace_back(e);
		unite(e.u,e.v);
	}
}

```
# Prim
- Create an adjacency matrix.
- Choose a vertex.
- Repeat this:
	- Choose the shortest adjacent new vertex.

```C++

//weight INF means theres no edge
const int INF=1000000000;

struct Edge{
	int w=INF;
	int to=-1;
};

int n;
//this stores the weight of the edges.
int adj[n][n];


vector<bool> selected(n,False);
//for each non selected vertex stores the min edge to a selected vertex.
vector<Edge> min_edge(n);
vector<Edge> result;
//we initialize that the cost to get to the starting vertex is 0.
min_edge[0].w=0;

int cost=0;

//we run the algorithm n times.
for(int i=0;i<n;i++){
	//variable for the closest unadded vertex
	int v=-1;
	//for every unadded vertex
	for (int j=0;j<n;j++){
		//if its closer then the current update it.
		if (!selected[j]&&(v==-1||min_edge[j].w<min_edge[v].w))
			v=j;
		
	}
	//if the closest edge has a distance of INF it cant work.
	if (min_edge[v].w==INF){
		cout<<"NAHHHH";
	}
	selected[v]=true;
	cost+=min_edge[v].w;
	if (min_edge[v].to !=-1)
		result.emplace_back(min_edge[v]);

	//after we have added a new vertex we check for every vertex
	for (int to=0;to<n;to++){
		//if the edge between it and the new vertex is cheaper then the previous edge to a connected vertex.
		if (adj[v][to]<min_edge[to].w)
			//if so update
			min_edge[to]={adj[v][to],v};
	}
}

```
# Applications
## Maximum Spanning Tree
To get the maximum spanning tree we simply turn all the weights negative and run the MST algorithm.
# Fixed Spanning Tree
Some edges are already fixed as part of the solution.
We first add them to the solution then we run a normal algorithm.
# Second Best MST
This can be done quicker with: https://cp-algorithms.com/graph/second_best_mst.html.

- Get the MST with Kruskal. 
- For each edge remove it and retry to find the MST.
- The second best MST is the minimum of these.

# Number of Spanning Trees.
If the graph is complete its $n^{n-2}$.

The number of spanning trees is the determinant of the Laplacian matrix:
- $L[i,i]$ is the degree of $i$
- $L[i,j]$ is $-1$ if there is an edge.
- Otherwise $L[i,j]$ is $0$.
The number of spanning trees is the cofactor of any index.
The cofactor of $(i,j)$ is $(-1)^{i+j}$ times the determinant of a matrix obtained by removing the $i$th row and the $j$th column.




# redone kruskal template
```C++
//This is just a DSU  
int parent[n];  
int size[n];  
  
//Standard DSU  
int find_parent(int v){  
    if (parent[v]==v) return v;  
    else return parent[v]= find_parent(parent[v]);  
}  
//More standard DSU  
int join(int a,int b){  
    a= find_parent(a);  
    b= find_parent(b);  
    if (size[b]>size[a]) swap(a,b);  
    parent[b]=a;  
    size[a]+=size[b];  
}  
  
//This is a custom variable type called edge.  
struct Edge{  
    //Edge can hold multiple variables. An edge needs:  
    //an origin    
    int from=-1;  
    //a destination  
    int to=-1;  
    //a cost  
    int cost=-1;  
    /*this is a custom operator used to compare 2 edges.  
    Normally < returns true or false depending on if something is    smaller or not. (This is why we are using bool). The custom comparison     is between 2 edges, so we define another edge called "other"*/    
    bool operator < (Edge other){  
        //we just compare the 2 costs  
        return cost<other.cost;  
    }  
};  
  
int get_MST(){  
  
    //this a vector of edges.  
    vector<Edge> all_edges;  
    //we need to sort it.  
    sort(all_edges.begin(), all_edges.end());  
  
    //by definition a MST has n-1 edges, but we cannot run a for loop n-1 times  
    //because some edges might not connect anything new, so we use a variable    //to keep track of the edges.    
    int edges_added=0;  
  
    //we also need an index to iterate over all_edges.  
    int i=0;  
  
    //this keeps track of the MST cost  
    int cost=0;  
    while (edges_added<n-1){  
        //given the current edge with a from and a to, we are checking if the 2 vertices  
        //are in different connected components.        
        if (find_parent(all_edges[i].from!= find_parent(all_edges[i].to))){  
            //increase the cost  
            cost+=all_edges[i].cost;  
            edges_added++;  
            join(all_edges[i].from,all_edges[i].to);
        }  
    }
```