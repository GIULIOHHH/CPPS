#CP
# What
Using divide and conquer to speed up certain types of dp.

# Time complexity
$$
O(n*logn)
$$
# Explanation
This trick can be used whenever our dp includes a cost function $C$ that we have to minimize.

>This is usually used in problems where we have to group together elements of an array

For example:
$$
dp[k][j]=min(dp[k-1][i]+C[i][j])
$$
>The actual form of $dp[k-1][i]$ doesnt really matter, it could be $dp[k-1][i-1]$ or another form.

$C(l,r)$ has to have 2 properties:
## Quadrangle inequality
The cost of smaller intervals needs to be better or equal then the contiguous one.
>This is needed because we construct our solution from smaller ones.
#### Explaination
Our cost needs to satisfy this property:
$$
C(a,c)+C(b,d)\le C(a,d)+C(b,c)
$$
![[Pasted image 20240609191521.png|400]]
>Going wider is equal or worse.

>For example dividing into subarrays and taking square roots satisfies this property.

## Monotone
The cost needs to be monotone, aka increasing the interval does not decrease the cost.
## Divide and Conquer
We need to fill a 2d grid, where each row is a value of $k$.
given a row, each element is the min cost subset ending at that index (plus the previous dp). 
![[Pasted image 20240610105516.png]]
So for each index ($j$) we have to find the best $i$ that minimizes the cost.
>The bruteforce solution would be to try each subset, in $O(n^2)$.

Because our $C$ is monotone, given an index $a$, after we find its $i_a$, we know that the $i$ of every element left of $a$ has an index lesser or equal then $i_a$.

The $i$ of every element right of $a$ has an index greater or equal then $i_a$.
![[Pasted image 20240610135556.png]]

# Algorithm
- Repeat this to fill the entire row:
	- Calculate the best $i$ ending at the midpoint
	- Calculate the best $i$ for the left subset, knowing that its less then the one we just calculated.
	- Calculate the best $i$ for the right subset.
# Generic Template
This is a generic template for this $dp[k][j]=min(dp[k-1][i]+C[i][j])$
```C++

//we hold the last 2 rows in the dp
vector<long long> dp_before, dp_cur;

//computes dp_cur[l]...dp_cur[r] inclusive
//[optl optr] is the range where i is for the midpoint.
void compute (int lb, int rb, int optl, int optr){
	if (l>r)
		return;
	//computes the midpoint
	int mid (l+r)/2;
	//the best value of i and its index
	pair<long long int> best={INF,-1};

	//computes the value from optl to the middle (or optr if its smaller)
	for (int i=optl;k<=min(mid,optr);j++){
		//to do dp_before[i-1] i needs to be bigger then 0. 
		//best is the minimum between itself and a tuple containing the new cost and the position of i
		if (i)
			best=min(best,{dp_before[i-1]+C(i,mid),i});
		else
			best=min(best,{C(i,mid),i});
	}
	//sets the dp
	dp_cur[mid]=best.first;
	//opt is the position of the best i
	int opt = best.second;
	//computes the left 
	compute(l,mid-1,optl,opt);
	//computes the right
	compute(mid+1,r,opt,optr);
}
long long solve(){

	for (int i=1;i<m;i++){
		compute(0,n-1,0,n-1);
		dp_before=dp_cur;
	}
	return dp_before[n-1];
}
```
# Examples
## 
https://robert1003.github.io/2020/02/25/dp-opt-divide-and-conquer.html



https://assets.hkoi.org/training2023/dp-iii.pdf

https://assets.hkoi.org/training2022/adc.pdf