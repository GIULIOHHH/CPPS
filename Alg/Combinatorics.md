#CP 
## Index
[[#Addition Principle]]
[[#Multiplication Principle]]
[[#Factorial]]
[[#Combinations]]
[[#Binomial Theorem]]
[[#Inclusion-Exclusion]]
[[#Stars and Bars]]
[[#Catalan Numbers]]
# General Strategies
- Break the problem into parts.
	- If the things we are counting have no overlap, we can count them independently and then add them up
	- We can count the number of each kind of thing and them multiply them.
- Sometimes its easier to count the things you dont want.

# Addition Principle
If there are $A$ ways of doing one thing and $B$ ways of doing another and we cant do both, there are $A+B$ ways of doing one of them.
# Multiplication Principle
If there are $A$ ways of doing a thing and $B$ ways of doing another, there are $A*B$ ways of doing both in a fixed order (starting by $A$ or starting by $B$).
>We can do each of them once.
# Factorial
$n!$ is $1*2*3*...*n$.
Or knowing that $0! = 1$: 
$$n! =(n-1)!*n$$
---
_Count the different number of permutations of numbers from $1$ to $n$._

For the first place we have $n$ possibilities.

For the second we have $n-1$ and so on. 

So the answer is $n!$

(We have to pick $n$ elements from a set of $n$ elements).

---
_Count the permutations where we select $k$ elements from a set of $n$ elements_.

The answer is:
$$
\frac{n!}{(n-k)!}
$$
>We write this as $P_k^n$.
# Combinations
_Count the number of ways to select $k$ elements from a set of $n$ elements if the order the elements are in doesnt matter_.

For every array of length $k$ there are $k!$ permutations, so we divide the answer by $k!$.

$$
\frac{n!}{(n-k)!\:*k!}
$$
We write this as
$$
\begin{pmatrix}n  \\ k\end{pmatrix}
$$

>We can also write it as $C_k^n$.

---
$$
\begin{pmatrix}n  \\ k\end{pmatrix}=\begin{pmatrix}n-1  \\ k-1\end{pmatrix}+\begin{pmatrix}n-1  \\ k\end{pmatrix}
$$
>We are choosing the $nth$ thing. We can either choose it (n-1 and k-1) or not choose it (n-1 and k).
We can use this formula to calculate $C^n_k$

We also know that:
$$
\begin{pmatrix}n  \\ k\end{pmatrix}=\begin{pmatrix}n  \\ n-k\end{pmatrix}
$$
By simplifying the original one we get: 
$$
\begin{pmatrix}n  \\ k\end{pmatrix}=\frac{n*(n-1)...*(n-k+1)}{k!}
$$

#### Implementation
```C++
int binomialCoefficient(int n,int k){
	int res=1;
	//since C(n,k)=C(n,n-k)
	if (k>n-k) 
		k=n-k;
	for (int i=0; i<k, i++){
		//calculates n*(n-1)*...*(n-k+1)
		res*=(n-i);
		//calculates k!
		res/=(i+1);
	}
	return res;
}
```
# Binomial Theorem
$$
\sum_{i=0}^n \begin{pmatrix}n  \\ i\end{pmatrix}=2^n
$$
Where $\sum_{i=0}^n \begin{pmatrix}n  \\ i\end{pmatrix}$ means  $\begin{pmatrix}n  \\ 0\end{pmatrix}+\begin{pmatrix}n  \\ 1\end{pmatrix}+...+\begin{pmatrix}n  \\ n\end{pmatrix}$.

---
_How many subsets of $S$ exist if the size of $S$ is $n$?_

For each element we can choose if we pick it, so there are $2^n$ choices.

For each subset of size $i$, the solution is $\begin{pmatrix}n  \\ i\end{pmatrix}$. Since $i$ can range from $1$ to $n$ we have to sum them. $\sum_{i=0}^n \begin{pmatrix}n  \\ i\end{pmatrix}$
>This is the proof of the formula.

>If we want to enumerate all the subsets, we can use a binary string, where if the $i$th bit is set we chose the element.
>The max size of the number is $2^n$.


# Inclusion-Exclusion

_$i$ people are part of $A$, $j$ people are part of $B$ and $k$ people are part of $C$. How many people are there if one can be a part of any number of groups?_
![[Pasted image 20240618161508.png]]
$$
A\cup B \cup C=A+B+C-(A\cap B) -(B \cap C) - (A \cap C)+ (A \cap B \cap C)
$$
![[Pasted image 20240618165651.png]]
If we generalize
$$
\sum_{i=1}^n(A_i)-\sum^n_{1\le i< j \le n}(A_i \cap A_j)+\sum^n_{1\le i< j<k  }(A_i \cap A_j\cap A_k)+...+(-1)^n*\sum^n(A_1 \cap A_2\cap...\cap A_n)
$$
>We are basically alternating between $+$ and $-$.

---
_Count the permutations of numbers from $0$ to $9$ such that the first element is greater then $1$ and the last lesser then $8$_.

$A \cup B=A+B-(A\cap B)$.

The permutations where the first element is lesser or equal then $1$ are $2*9!$, because the first element can be $1$ or $0$, and then the remaining elements can be in any order.

Same thing for the permutations where the last element is greater or equal then $8$.

The permutations that include both the conditions are 
$2*8!*2$, because the first element can be $1$ or $0$, while the last can be $9$ or $8$, and there are $8$ more free choices.

We can do $10!-A \cup B$ to get the solution

---
_Count the number of sequences of length $n$ consisting only of numbers $0,1,2$ such that each number occurs at least once._

There are a total of $3^n$ sequences (3 options for each index).
The bad sequences are $A_0\cup A_1 \cup A_2$, Where:
- $A_i$ is $2^n$ (we can only use $2$ digits)
- $A_i \cap A_j$ is $1$ (theres only one digit)
- $A_0\cap A_1 \cap A_2$ is 0 (there are no more digits left)
# Stars and Bars
The ways to place $n$ equal objects in $k$ containers is
$$
\begin{pmatrix}n+k-1  \\ n\end{pmatrix}
$$
>We have $n$ stars and $k-1$ bars between them to symbolize the containers.
$***|**||*|$
The number of ways to place the stars is the same as the number of ways to pick $n$ elements out of $n+(k-1)$ total positions.

---
_Count the solutions to this equation $x_1+x_2+...x_k=n$ where $x_i\ge 0$._

We need to find the ways to place $n$ numbers into $k$ containers, so the answer is the one above.
___
_Count the solutions to this equation $x_1+x_2+...x_k=n$ where $x_i\ge 1$._

We have $n$ stars and need to place $k-1$ bars between them. Since we cannot have empty bars, there are $n-1$ possible positions.
$$
\begin{pmatrix}n-1  \\ k-1\end{pmatrix}
$$
# Catalan Numbers

$$C_n=C_0*C_{n-1}\;+\;C_1*C_{n-2}\;+\;...\;+\;C_{n-1}*C_0$$

The first Catalan numbers are: $1,1,2,5,14,42,132$

---
### Example problems solved with the Catalan Numbers
1. _Count the valid parenthesis groupings of length $2n$._

We can represent every balanced set as $(A)B$, where $A$ and $B$ are valid groupings.
>It starts with an $($, somewhere theres a closing $)$, and to the right of it there can be more parenthesis.

Either $A$ or $B$ can be empty, and can contain a max of $n-1$ parenthesis.

If $A$ contains $k$ parenthesis, $B$ contains $n-k-1$.

To get the formula for length $n$ we need to count the number of ways that $A$ can have $0$ pairs and $B$ $n-1$ plus the number of ways that $A$ can have $1$ pair and $B$ $n-1-1$ and so on.


$C_0=1$
$C1=C_0*C_0$
$C_2=C_0*C_1\;+\;C_1*C_0$
$C_3=C_0*C_2\;+\;C_1*C_1\;+\;C_2*C_0$
>There is one way to have no parenthesis.
>We multiply the 2 because of the [[#Multiplication Principle]].

---
2. _If $2n$ people are in a circular table, in how many ways can they all shake hands such that none of the arms cross each other?_
![[Pasted image 20240619151413.png|400]]
If we pick one person, he will need to leave an even number of people on each side.
He can leave $0$ on the right and $n-1$ pairs on the left, $1$ pair and $n-1-1$ on the left and so on.

---
3. _In an $n*n$ grid, count the number of paths of length $2n$ from the upper left corner to the lower right that stay on or above the main diagonal._
![[Pasted image 20240619153425.png]]
This is equivalent to "mountain ranges" staying above $y=0$.
The mountain ranges are equivalent to valid parenthesis sequences.

---
4. The number of full binary trees (with either 2 or 0 children per vertex) with $n+1$ leaves. 
5. The number of ways to cover a ladder $1..n$ using $n$ rectangles.

