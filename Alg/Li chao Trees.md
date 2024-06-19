# What
A dynamic [[Segment Tree]] that supports 2 operations:
- Insert a line $mx+b$ (or monotonic function)
- find the max line for a given $x$.
>Basically a faster and easier [[Convex Hull DP|Convex Hull]].


# Time Complexity
$$
O(logn)
$$
# Explanation

The candidates for a query are the nodes on the path from the root to $x$.
Given a node we always keep the line with the greater middle value.

We always insert the line with the greatest slope. If the new line has a lesser slope we swap them.
In the range $[L,R]$ there is the line $red$. When inserting $blue$ we have 2 cases:
1:$red(mid)<blue(mid)$:
We put $red$ in to the range $[L,mid]$.
We put $blue$ in $[L,R]$.
![[Pasted image 20240613232014.png]]
2:$red(mid)>blue(mid)$:
We put $red$ in the range $[L,R]$.
We put $blue$ in $[mid,R]$
![[Pasted image 20240613232811.png]]
# Template
```C++
struct Line{
	long long m,b;
	//operator to do Line(x)
	long long operator(long long x) {return m*x+b;}
}
//
void insert(l,r, Line seg, int index=1){
	//if were at a leaf
	if (l==r){
		//if the new line is bigger then the leaf one delete the leaf one.
		if (seg(l)>a[index](l)) a[index]=seg;
		return;
	}
	int mid=(l+r)/2;
	//if the new line has a lesser slope swap them
	if (a[index].m>seg.m) swap(a[index],seg);
	//if the new line is bigger swap it in [l,r]
	if (a[index](mid)<seg(mid)){
		swap(a[index],seg);
		//inserts the swapped segment left
		insert(l,mid,seg,index*2);
	}
	else insert(mid+1,r,seg,index*2+1);
}

long long query(int l, int r, int x, int index=1){
	//leaf base case
	if (l==r) return a[index](x);
	int mid =(l+r)/2;
	if (x<=mid) return max(a[index](x), query(l,mid,x,index*2)));
	else return max(a[index](x),query(mid+1,r,x,index*2+1))
}
```


BRUHHHHHHHHHH
https://codeforces.com/blog/entry/86731