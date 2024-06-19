#CP
# What 
Finding the highest line at a given point.
# Time Complexity
$$
O(n*logn)
$$
# When
Whenever we have:
$$
dp[i]=min(m+b*a[i])
$$
# Explanation
We have a set of linear functions $y=m_i*x+b_i$ and some queries, where given an $x$ we need to find the function that returns the highest value.
We also need to be able to insert new functions quickly.

We can represent them as lines, so the problem becomes finding the highest line for a given $x$.
![[Pasted image 20240611141949.png]]
The combination of the highest lines creates a [[Convex Hull]].

- Every line provides the max value for a contiguous range. 
- Every line not on the hull is useless.
	- We dont need to keep them.
- The lines are in sorted order based on their slope.

# Different Cases
### Queries in (decreasing) sorted order
Once we get the max for a position $x$, all lines after it are now useless, so we can:
- Compare the value of $x$ of the rightmost line with the line left of it.
- If the second value is higher remove the rightmost line.
![[38c8c16505aeb550f4c41a5826841aecf3f9e277.gif]]
### Unsorted queries
We keep a [[Set]], storing for each line the minimum $x$ coordinate where its the highest. 
- We then binary search for the line to use.

### Lines in (decreasing) sorted order
When we insert a new line its slope is less then all the lines in the hull, so it handles the range $[-\infty,i]$.
- If the intersection point of the line we are inserting and the leftmost line is to the right of the previous intersection point (between the leftmost line and the line next to it) we can remove the leftmost line.

>We also need to deal with parallel lines
![[5c5fbd97c9aa2ceebe804ac789e5e67837217bdf.gif]]
### Unsorted lines
We do not insert a line if it doesn't appear on the hull.
- We can use a [[Set]] sorted by slope, and binary search the location where the line should be inserted.
$i$ is the line we are inserting $l1$ is the line to the left of $i$.
$l2$ is the line to the left of $l1$.
same thing for $r1$ and $r2$.

If the intersection $i-l2$ is to the left of $l1-l2$, we delete $l1$
>We are substituting $l1$.
![[Pasted image 20240612134317.png]]

If the intersection $i-r2$ is to the right of $r1-r2$ we delete $r1$.
![[Pasted image 20240612134606.png]]
# Template
(Horrible code, use [[Li chao Trees]])
```C++

struct Line{
	// m is the slope, b the y itercept and p the intersection with the next line
	mutable long long m, b,p;
	//if you compare 2 lines it compares them based on the slope
	bool operator<(const Line &other) const {return m<other.m;}
	//if you compare a line and an x query it compares the intersection point
	bool operator <(long long x) const{return p<x;}
};
//Linecontainer inherits from multisets, sorted from smallest to biggest
struct LineContainer: multiset<Line,less<>>{
	const long long INF=1e9+9;
	//We want to take the floor of negative numbers (for example -7/3 normally is -2 even if the division is -2.33333333, we want -3, aka the biggest int not greater then the division)
	long long div(long long a,long long b){
		//if theres a remainder and one of the numbers is negative subtract 1
		return a/b-(a^b<0&&a%b);
	}
	//finds the intersection of x and y
	bool intersection(iterator x, iterator y){
		//if y is the end, it means that there arent any more lines after x, so that means theres no intersection
		if (y==end()){
			x->p=INF;
			return false;
		}
		//if the slopes are equal
		if (x->m==y->m){
			if (x->b>y->b){
				//x is above y
				x->p=INF;
			}
			else{
				//x is below y.
				x->p=-INF;
			}
		}
		else{
			//if the slopes are different calculate the interection point
			div(y->b - x->b, x->m - y->m);
		}
	//it returns true if the intersection point of x is bigger then the line beyond it. If it returns true y is redundant.
	return x->b>=y->b;
	}
	//adds a new line
	void add(long long m, long long b){
		//inserts the line with initial insertion point 0. z is the iterator
		auto z= insert({m,b,0});
		//y and x are the new line
		y=z;
		x=y;
		//z is the line to the right
		z++;
		//removes lines from the right
		while (intersection(y,z)){
			//z is the pointer to the next line
			z=erase(z);
		}
		if (x!=begin &&interection(x--,y)){
			//removes y and redoes the intersections
			y=erase(y)
			intersection(x,y)
		}
		//checks the intersection points of the previous lines
		while ((y=x)!=begin()&&(x--)->p>=y->p){
			intersect(x,erase(y))
		}
	}
	long long query(long long x){
		//checks that the container isnt empty
		assert(!empty());
		auto l= *lower_bound(x);
		return l.m *x+l.b;
	}
}
```
# Examples

## Covered Walkway
# Syuff
https://wcipeg.com/wiki/Convex_hull_trick
https://codeforces.com/blog/entry/63823
https://codeforces.com/blog/entry/51684
https://cp-algorithms.com/geometry/convex_hull_trick.html
https://www.youtube.com/watch?v=OrH2ah4ylv4