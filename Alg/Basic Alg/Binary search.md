# What
The fastest way to find elements in sorted lists.

# Time complexity
$$
O(logn)
$$

# When
Whenever we need to find elements in a sorted list.

# Logic
## Example problem
We have $16$ boxes, each of which contains an _unknown_ number from $1$ to $100$. We know that the numbers are in order. 
We want to find the number $n=34$ while checking **at most** $4$ boxes.


At the start we know that the number $n$ could be in any of the boxes. We call the leftmost position where it could be $l$ and the rightmost $r$.
![[Pasted image 20241102121321.png]]
We check the middle box: $middle=(l+r)/2$
![[Pasted image 20241102121549.png]]
We get the number $45$, which is bigger then $34$. 
We now **Know for certain** that $34$ cannot be to the right of $mid$.
>since its smaller then $45$ and all the numbers right of $mid$ are bigger then $45$.

Thus $l$ stays the same, but $r$ becomes $mid-1$.
We can now repeat:
![[Pasted image 20241102121808.png]]
The middle box is $13$, which is less then $34$.
We now **Know for certain** that $34$ cannot be to the left of $mid$.
$r$ stays the same, but $l$ becomes $mid+1$.

We repeat:
![[Pasted image 20241102121943.png]]
The number will be found when $l=r$.

_What if the number isnt in the boxes?_
The previous number now is $25$ instead of $34$, so we restrict the range to only one box.
![[Pasted image 20241102121808.png]]
![[Pasted image 20241102122102.png]]
The number is not $34$.
![[Pasted image 20241102122456.png]]
Following the previous logic, since $44$ is bigger then $34$, $r$ becomes $mid-1$, but since in the previous state $r=l$, $r$ now **becomes smaller then $l$**.
This is when we know we can stop.
>The same thing would have happened if the number was smaller then $34$, because $l$ would have become bigger then $r$.

# Algorithm
While checking a range  $[Left\_bound,Right\_bound]$
1. We initialize $l=Left\_bound$
2. We initialize $r=Right\_bound$
3. while $l<=r$
	1. We get the mid point $mid=(l+r)/2$
	2. We check the element at the mid point:
		1. If its equal to our target we stop.
		2. If its smaller $l=mid+1$
		3. If its bigger $r=mid-1$
# Template
```C++
int target;
int n;
//the array where we are binary searching
int arr[n];
//in this case the range is [0,n]
int l=0;
int r=n;
while (l<r){

	//we get the mid point
	mid=(l+r)/2;

	//we check the value in the mid point
	if (arr[mid]==target) break;
	else if (arr[mid]<target) l=mid+1;
	else r=mid-1;
}

if (arr[mid]!=target) cout<<"the element isnt there";
else cout<<"the element is at position"<<mid;

```

# Binary search the answer

## Time complexity
$$O(n*logn)$$
