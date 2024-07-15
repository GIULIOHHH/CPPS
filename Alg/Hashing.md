 #CP
# What 
Comparing strings in $O(1)$.
# Time Complexity
Building in:
$$
O(n)
$$
Comparing in:
$$
O(1)
$$
# Algorithm
$$
hash(s)=s[0]*p^0+s[1]*p^1+s[2]*p^2+...\;\;\;mod\;\;m
$$
$p$ is a prime number roughly equal to the characters in the alphabet. 
We use $p=31$ for only lowercase and $p=51$ for uppercase as well.

Usually $m=10^9+9$.

To make the algorithm faster we should precompute the values of $p^i$.
# Computing a Hash
```C++
long long compute(string const &s){

	const int p=31;
	const int m= 1e9+9;
	long long hash=0;
	long long power=1;
	
	for (char c:s){
	
		hash+=(c-'a' +1)*power;
		power*=p;
		
		hash%=m;
		power%=m;
	}
	return hash;
}
```

# Substring hashing
We can easily compute substrings with hash prefixes.
$$
hash(s[i,j])*{p^{i}}={(hash(s[0,j])-hash(s[0,i-1]))}\;\;\;mod\;\;m
$$
To get rid of the $p^i$ we can either do [[#Division]] or [[#Multiplication]].
#### Example
$$hash(coding)=c*p^0+o*p^1+d*p^2+i*p^3+n*p^4+g*p^5$$
We want:
$$
hash(din)=d*p^0+i*p^1+n*p^2
$$
To get it we use the formula.
$$
hash(s[0,j])/hash(codin)=c*p^0+o*p^1+d*p^2+i*p^3+n*p^4
$$
$$
hash(s[0,i-1])/hash(co)=c*p^0+o*p^1
$$
If we subtract these 2 we get
$$
d*p^2+i*p^3+n*p^4
$$
We now have to get rid of the $p^2$.
## Division
We want to divide by $p^i$, but since MOD division isnt possible, we have to multiply everything by the [[Division with MOD|Modular Multiplicative Inverse]] $mp^i$. 
We also need to store $mp^i$ to be more efficient.
$$
hash(s[i,j])=(hash(s[0,j])-hash(s[0,i-1]))*mp^i\;\;\;mod\;\;m
$$

# Multiplication
When comparing 2 substrings we can just have them both raised to the same power.
If we have a substring multiplied by $p^i$ and another by $p^j$:
- If $i$ is smaller we multiply the first by $p^{\;j-i}$, aka $p^j/{p^i}$
- if $i$ is bigger we multiply the second by $p^{\;i-j}$

>We are taking the smallest $p$ substring and pushing it up in order to have it match the bigger $p$.
# Applications

## Check for duplicate strings.
- We compute the hash for each string.
- We sort the strings.
- We check if a string is equal to the previous one.
## Pattern matching
Return all occurrences of $s$ in $t$.
- Calculate the hash of $s$.
- Calculate prefix hashes for $t$.
- Match $s$ with every substring.
```C++
const int p=31;
const int m=1e9+9;
int s_size=s.size(), t_size=t.size();
//stores the powers of v up to t.
vector<long long> p_pow(t_size);
p_pow[0]=1;
//computes the powers
for (int i=1;i<p_pow.size();i++){
	p_pow[i]=(p_pow[i-1]*p)%m;
}
//holds the prefixes of t. Is initialized with 0.
vector <long long> pref(t_size+1,0)
for (int i=0;i<t_size;i++){
	//the prefix is equal to the previous one + the digit times p_pow.
	h[i+1]=(h[i]+(t[i]-'a'+1)*p_pow[i])%m;
}

//calculates the hash
long long s_hash=0;
for (int i=0; i<s_size;i++){
	s_hash=(s_hash+(s[i]-'a'+1)*p_pow[i])%m;
}

vector<int> occurrences;
//checks all substrings.
for(int i=0;i+s_size-1<t_size;i++){
	//calculates the substring. We do +m to ensure the value is positive. It doesnt change the final value because we do %m afterwards. We can also use abs()
	long long current_sub=(pref[i+size_s]-pref[i]+m)%m;
	//we do s_hash times p_pow[i] in order to offset it. its like we did times p^{j-i} but i is 0, because s_hash starts from 0.
	if (current_sub==s_hash*p_pow[i]%m)
		occurrences.push_back(i);
```

# Shortest palindromic suffix
Given a string $s$ return the smallest palindrome that can be formed by adding zeroes or more characters at the end.
>For example abba-> abba
>zyabba->zyabbaYZ

>abba is the longest palindromic suffix. Then we have to append everything else. The actual task is finding the lowest palindromic suffix.

- Start from the end, compute a suffix and its reverse.
- To handle the reverse:
	- Start by hashing the current letter.
	- Left shift everything by multiplying by $p$.
	- Add to the end the last letter, multiplying it by $p^0$.

> For example
> a/a
> ba/ab
> bba/abb
> abba/abba
> yabba/abbay
> zyabba/abbayz

file:///C:/Users/admin/Downloads/String_Processing_Rolling_Hash.pdf


# Number of palindromic substrings
You can use hashing and binary search to get the number of palindromic substrings in $O(n*logn)$

