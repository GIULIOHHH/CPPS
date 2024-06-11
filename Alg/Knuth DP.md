#CP
# What 
Solving range DP quickly.
>Usually used when we have to break up a range.
# Time Complexity 
$$
O(n^2)
$$
# When
Whenever the transition is:
$$
dp(i,j)=min[dp(i,k)+dp(k+1,j)+C(i,j)]
$$
And $C$ is [[Divide and conquer DP|Monotone and satisfies the quadrangle inequality]].

# Explanation
We need to find values of $k$ that minimize the dp.
>$k$ is the optimal splitting point

We create another array called $opt[i][j]$, that is the index $k$ that minimizes $dp[i][k]+dp[k+1][j]$.

To calculate $opt[i][j]$ we only have to test values of $k$ between $opt[i][j-1]$ and $opt[i+1][j]$
>This works because 
>$opt[i][j-1]\le opt[i][j] \le opt[i+1][j]$

So we want to process the states in a way that $dp[i][j-1]$ and $dp[i+1][j]$ are calculated before $dp[i][j]$.

We know that $i<j$, so we can process them like this:
- a for loop where $i$ goes from the max val to the min one.
	- a nested for loop where $j$ goes from $i+1$ to its max value.
```C++
for (int i=0;i<n;i++){
	//the optimal splitting point for a range of lenght 1 is i
	opt[i][i]=i;
}
//the for loops described above
for (int i=n-2;i>=0;i--){
	for (int j=i+1;j<n;j++){

		int mn=INF;
		int cost= C[i][j];
		//tries values of k between opt[i][j-1] and opt[i+1][j] and also ensures that k isnt bigger then j-1 because it should be in range [i,j]
		for (int k=opt[i][j-1];k<=min(j-1,opt[i+1][j]);k++){
			//updates the min
			if (mn>=dp[i][k]+dp[k+1][j]+cost){
				opt[i][j]=k;
				mn=dp[i][k]+dp[k+1][j]+cost;
			}
		}
		dp[i][j]=mn;
	}

}
```