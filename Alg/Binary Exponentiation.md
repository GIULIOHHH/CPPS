#CP 

# What 
Computes $a^n$ quickly.
# Time Complexity
$$
O(logn)
$$

# When 
Whenever we have an [[Associative operation]].
# Logic 
$$
a^{2n}=a^{n}*a^{n}
$$
$$
a^{b+c}=a^b+a^c
$$
We can split the work with the binary representation of a number, multiplying only when the bits are 1.
$$
3^{13}=3^{1101}=3^8*3^4*3^2
$$
Every element in this sequence is the square of the previous.
$3^1=3$
$3^2=(3^1)^2=9$
$3^4=(3^2)^4=81$.
# Algorithm
## Slower Recursive
We can use this recursive formula. 
$a^n =$
$(a^{n/2})^2$ if $n$ is even.
$(a^{n-1/2})^2*a$ if $n$ is odd.
## Faster Iterative
Maintain 2 variables, the result and the current square in the sequence $s$.
Get the binary representation of the exponent.
If the bit at the current index is $1$, multiply the result by the square $s$.
Move up in the sequence by squaring $s$.
left shift to get the next bit.
# Example
## Recursive
The target is $3^{13}$. 
$13$ is odd, so $3^{13}=(3^{6})^2*2$
$6$ is even so $3^6=(3^3)^2$
$3$ is odd so $3^3=(3^1)^2*3$
## Iterative
The target is $3^{13}$.
The binary representation is $13=1101$.

$bit_i=1$, so we multiply the result by $3^1$.
We now need to move up in the sequence, so we do $(3^1)^2=3^2$
Now we can left shift $1101>>=110$.

$bit_i=0$, so we dont do anything.
We now need to move up in the sequence, so we do $(3^2)^2=3^4$
Now we can left shift $110>>=11$.
# Iterative Template
```C++
long long binpow(long long a, long long b){
	//Maintain 2 variables, the result and the current square in the sequence s.
	long long res=1;
	long long s=a;
	//while the binary representation has ones
	while b(>0){
		//we take the AND of the first bit and 1 (will only be true if both of them are 1)
		if (b&1){
		//If the bit at the current index is 1, multiply the result by the square s.
			res*=s;
		}
		//Move up in the sequence by squaring s.
		s*=s;
		//left shift to get the next bit
		b>>=1;
	}
	return res;
}
```
# Problem example 
### LIMAK
Limak can be either happy or sad. He switches with probability $p$ every minute. If hes happy now, whats the probability that hes happy after $n$ minutes?

We have the probability that his mood flips for 1 minute: $p_1$. 
We can compute $p_2$. There are 2 possibilities of length 1 minute.
- He flips to sad and then stays sad $p_1*(1-p_1)$
- He stays happy 1 minute and then flips to sad $(1-p_1)*p_1$
We join them to get $p_2$: $p_2=p_1*(1-p_1)+(1-p_1)*p_1$.

We can also compute $p_4$, by splitting it into 2 possibilities of length 2 minutes.
- He flips to sad in the first 2 minutes and then stays sad for the next 2 minutes $p_2*(1-p_2)$
- He stays sad the first 2 minutes and then flips to happy in the next 2 $(1-p_2)*p_2$
We join to get $p_4$: $p_4=p_2*(1-p_2)+(1-p_2)*p_2$

We can do the same exact thing for $p_8$ and so on.
