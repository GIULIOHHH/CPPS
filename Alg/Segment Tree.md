#CP
# What 
Data structure that can:
- modify a value.
- Answer range queries.

# Time Complexity
$$
O(log(n))
$$
# When 
Whenever we have multiple range queries of an [[Associative operation]].
# Logic
A segment tree has a max size of $4n$.

We store it as a $1$-indexed array where every vertex has 2 children. 
Each vertex can store a value or an entire array.

Given a vertex $i$:
- The parent is $i/2$.
- The left child is $2i$
- The right child is $2i+1$

>We can switch between the left and right child with XOR.

Given a range $[l,r]$ and a mid point $m=(l+r)/2$
- The left child is $[l,m]$
- The right child is $[m+1,r]$

# Algorithm
### Building
We use a recursive function:
- Update the children of the current vertex.
- When we reach a leaf copy the value from the original array.
- Use the children to compute the original vertex.
### Queries
We maintain 2 variables $[lq,rq]$ representing the subset of the current range we care about.
We use a recursive function:
- If our query is invalid (there is no part of the current range we care about) return $0$.
- If our query matches the current range return the tree value.
- Otherwise query both the left and right children.
### Value Updates
We use a recursive function:
- Check if the position we want to update is in the left or right child.
- Once we get to the leaf update it.
- Update the original node based on the children.

### Range Updates (Lazy Propagation)
We find the vertices responsible for the range with the same recursive function for queries then:
- We update the value of the node accordingly.
- We store in another variable that we need to push the same update to the children.

Whenever going down the tree for any reason we push the update to the 2 children.
# Template
### Simple Sum
```C++
int n, t[4*MAXN];

//a is the starting array v the current vertex, lb the current left boundary and rb the right one.
void build(int a[], int v, lb, int rb){
	//if the boundaries match we have a leaf.
	if (lb==rb){
		//sets the value of the tree based on the boundary
		t[v]=a[lb];
	}
	else{
		int mid=(lb+rb)/2;
		//builds the left child
		build(a,v*2,lb,mid);
		//builds the right child
		build(a,v*2+1, mid+1,rb);
		//builds the current vertex based on the children.
		t[v]=t[v*2]+t[v*2+1];
	}

}
//lq is the left query of THE CURRENT NODE. ([lq,rq] is the part we care about of [lb,rb])
int query(int v, int lb, int rb, int lq, int rq){
	//if the query of the current node is invalid return 0.
	if (lq>rq)
		return 0;
	//if they match return them
	if (lq==lb&&rq==rb)
		return t[v];
	int mid = (lb+rb)/2;
	//we take the min to avoid rq becoming bigger then rb.
	return query(v*2,lb,mid,lq,min(rq,mid))+query(v*2+1,mid+1,rb,max(lq,mid+1),rq);

}

void val_update(int v, int lb, int rb, int pos, int new_val){
	//when we reach the leaf update it
	if (tl==tr){
		t[v]=new_val;
	}
	else{
		int mid=(lb+rb)/2;
		//if the val we want to update is in the left child go there
		if (pos<=tm)
			val_update(v*2,lb,mid,pos,new_val);
		else
			val_update(v*2+1,mid+1,rb,pos,new_val);
		//updating the current val
		t[v]=t[v*2]+t[v*2+1];
	}

}
```
### Max with sum Lazy propagation
```C++
//nothing changes in the build
void build (int v, int lb, int rb){
	if (lb==rb){
		t[v]=a[lb];
	}
	else{
		int mid=(lb+rb)/2
		build(v*2, lb,mid);
		build(v*2+1,mid+1,rb);
		t[v]=max(t[v*2],t[v*2+1]);
	}
}
//pushes a lazy update to the children
void push(int v){
	//adds the val to the left child
	t[v*2]+=lazy[v];
	//stores the lazy val
	lazy[v*2]+=lazy[v]
	t[v*2+1]+=lazy[v];
	lazy[v*2+1]+=lazy[v];
	//removes the lazy val from the main node
	lazy[v]=0;
}


//adds a val to each element in a range
void update (int v, int lb, int rb, int lq, int rq, int add_val){
	if (lq>rq)
		return;
	//if they match update
	if (lq==lb&&rq==rb){
		//THIS DEPENDS ON THE TYPE OF UPDATE.
		t[v]+=add_val;
		lazy[v]+=add_val;
	}
	else{
		//push to the children
		push(v);
		mid=(lb+rb)/2;
		//call update for each of the children.
		update(v*2,lb,mid,lq,min(rq,mid),add_val);
		update(v*2+1,mid+1,rq,max(lq,mid+1),rq,add_val);
		t[v]=max(t[v*2],t[v*2+1]);
		
	}

}
int query(int v, int lb, int rb, int lq, int rq){
	if (lq>rq)
		return -INF;
	if(lq==lb&&rq==rb)
		return t[v]
	//we always propagate when going down
	push(v);
	int mid=(lq+rq)/2;
	return max(query(v*2, lb,mid,lq,min(mid,rq))), query(v*2+1,mid+1,rb,max(lq,mid+1),rq))
}
```

# [[Persistent Data Structure|Persistent]] Segment Tree
We can turn a segment tree persistent by using pointers to children and creating new nodes for every update.

We will then store the roots in an array to jump between segment tree versions.
Example with Sum queries:
```C++
struct Vertex{
	//left and right children pointers
	Vertex *l, *r;
	int sum;
	//this is a leaf constructor. It is called when i create a new vertex and pass in a value as an argument. It creates null pointers for the children and assigns the value.
	Vertex(int val): l(nullptr), r(nullptr), sum(val){}
	//this is a normal vertex constructor. It runs when i create a vertex and pass in pointers to 2 children. I am assigning the pointers to the values l and r.
	Vertex(Vertex *l, Vertex *r): l(l), r(r), sum(0){
		//if the left child isnt null adds it sum to the main one.
		if (l) sum+=l->sum;
		if (r) sum+=r->sum;
	}
}

Vertex* build(int a[], int lb, int rb){
	//when we reach a leaf create a new leaf vertex
	if (lb==rb)
		return new Vertex(a[lb]);
	int mid=(lb+rb)/2;
	//otherwise create a main vertex.
	return new Vertex(build(a,lb,mid),build(a,mid+1,rb));
}

int query(Vertex *v, int lb, int rb, int lq, int rq){
	if (lq>rq)
		return 0;
	if (lq==lb&&rq==rb)
		return v->sum;
	int mid=(lb+rb)/2;
	return query(v,lb,mid,lq,min(rq,mid))+query(v,mid+1,rb,max(lq,mid+1),rq);
}
//when updating we create a new vertex
Vertex* update(Vertex *v, int lb, int rb, int pos, int new_val){
	//when we reach the leaf we update it
	if (lb==rb)
		return new Vertex(new_val);
	//otherwise we create a new vertex.
	int mid=(lb+rb)/2;
	//If the leaf update is on the left we leave the right child alone and viceversa.
	if (pos<=mid)
		return new Vertex(update(v->l,lb,mid,pos,new_val),v->r);
	else
		return new Vertex(v->l,update(v->r,mid+1,rb,pos,new_val));

}
```
# Dynamic Segment Tree
# Applications
### Compute the GCD/LCM
They are [[Associative operation]]s, so we can store the GCD/LCM for each vertex and then combine them.
### Finding the $k$-th zero in an array.
- At each vertex we store the amount of zeroes.
- Then we go down the tree:
	- If the left child has enough zeroes then we want we go left
	- Otherwise we go right and remove the zeroes from the left child.
```C++
int find_kth(int v, int lb, int rb,int k){
	//if theres not enough zeroes
	if (k>t[v])
		return -1;
	//when we reach the leaf return it
	if (lb==rb)
		return tl
	//if the left node has enough zeroes
	int mid=(lb+rb)/2;
	if (t[v*2]>=k){
		return find_kth(v*2,lb,mid,k);
	}
	else{
		return find_kth(v*2+1,mid+1,rb, k-t[v*2]);
	}
}
```

We can use the same logic to find:
- The first element greater than a given amount
- The first array prefix to be bigger than a given amount.
### Finding the max length subsegment online


### Finding the smallest number greater then $k$ in a range.
- We store the list in sorted order at each vertex. 
- When the query range matches the vertex range we binary search for the smallest value.
- When we have multiple values we pick the smallest one.

```C++
vector<int> t[4*MAXN];

void build(int a[], int v, int lb,int rb){
	if (lb==rb){
		t[v]=vector<int>(1,a[tl]);
	}
	else{
	mid=(lb+rb)/2
	build(a,v*2,lb,mid);
	build(a,v*2+1,mid+1,rb);
	//t[v] is empty. This merges the 2 arrays in it
	merge(t[v*2].begin(),t[v*2].end(),t[v*2+1].begin(),t[v*2+1].end(),back_inserter(t[v]));
	}
}
```
- If we want to modify numbers we use [[Multiset]]s.
- This can be sped up by using [[Fractional Cascading]].

### Finding the $k$th smallest number in a range
We use a persistent segment tree.

```C++
//initializes the entire tree with 0s.
Vertex* build(int lb, int rb){
	if (lb==rb)
		return new Vertex(0);
	int mid=(lb+rb)/2;
	return new Vertex(build(lb,mid),build(mid+1,rb));
}

Vertex* update(Vertex *v,int lb,int rb, int pos){

	if (lb==rb)
		return new Vertex(v->sum+1);
	int mid=(lb+rb)/2;
	if (pos<=mid)
		return new Vertex(update(v->l,lb,mid,pos),v->r);
	else
		return new Vertex(v->l, update(v->r,mid+1,rb,pos));
}

int find_kth(int lb, int rb, Vertex *vl, Vertex *vr, int k){
	if (lb==rb)
		return lb;
	int mid=(lb+rb)/2;
	int left_count= vr->l->sum - vl->l->sum;
	if (left_count>=k)
		return find_kth(lb, mid, vl->l, vr->l, k);
	else
		return find_kth(mid+1, rb, vl->r, vr->r, k-left_count);
}

int lb=0, rb=MAX_INDEX+1;
vector<Vertex*> roots;
roots.emplace_back(build(lb,rb));
for (int i=0l i<a.size(),i++){
	roots.emplace_back(update(roots.back(),lb,rb,a[i]));
}
// find the 5th smallest number from the subarray [a[2], a[3], ..., a[19]] 
int result = find_kth(roots[2], roots[20], lb, rb, 5);
```