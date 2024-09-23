#CP
# What 
Mapping each value to its relative size in a list (the index the value would have if the list was sorted).
$$
[7,3,4,1]->[3,1,2,0]
$$
# Time Complexity

$$
O(nlogn)
$$
# When 
When we only care about the order of values.

# Template
```C++
//pair made up of value-index
vector<pair<int,int>> pairs(n);
for (int i=0; i<n; i++){
	pairs[i]={a[i],i};
}
sort(pairs.begin(),pairs.end());
int next=0;
for (int i=0;i<n;i++){
	//if the current element is different from the previous one update next. We include this to maintain equal values.
	if (i>0&&pairs[i-1].first!=pairs[i].first)

		next++;
	a[pairs[i].second]=next;
}
```