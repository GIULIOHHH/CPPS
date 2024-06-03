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

# Template
### Sum
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
### Lazy propagation
# Applications
# Example
