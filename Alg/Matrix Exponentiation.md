#CP
# What 
Speeding up dp in constant space.
# Time Complexity
$$
O(nlog(n))
$$
# When 
I know the space required before getting any input.
# Logic 
## Matrix multiplication 
We have 2 matrices: $a$ and $b$. 
>The columns of $a$ need to be the same length of the rows of b. 
>The result has the number of rows of $a$ and the columns of $b$.

In order to multiply them we take a row of $a$ (the y position of the final number) and a column of $b$ (the x position of the final number).
We then iterate over the length of both and add to a sum the product of elements with same index (the first elements multiplied by each other plus the second elements multiplied by each other etc.)

![[Pasted image 20240503174226.png]]

$$
P[i][j]=\sum_{n=1}^k=a[i][n]*b[n][j]
$$
# Matrix exponentiation
In a starting matrix each $P[y][x]$ is the number of ways/sum/product from $y$ to $x$. 

When raising a matrix to the $n^{th}$ power we simulate $n$ transitions.

The final $P[y][x]$ is the Total amount of ways/result to get from starting $y$ to final $x$.
# Algorithm
We need to find transitions between states.
Given some starting states, we multiply each of them by a coefficient to get one next state and then sum. 
We put the coefficient in a column of the transition matrix $M$, and repeat for each next state.

>If $F(i)$ depends of the last $k$ terms, we can try a $k*k$ matrix

$M[a][b]$ is now the cost/number of ways to get from $a$ to $b$.

$$
M^0=I
$$ Where $I$ is an identity matrix.
Then we use [[Binary Exponentiation]] for $M$.


# Example
## Fibonacci numbers
We have a beginning matrix with:
$$[F_{i-2}\;\;F_{i-1}]$$
We now need to multiply it by a matrix $M$ so that we get:
$$
[F_{i-1}\;\;F_i]
$$
$M$ is always a $k*k$ square matrix (in this case $k=2$).
$$
M=\begin{bmatrix}a & b \\ c & d\end{bmatrix}
$$
So now we get the formula for the new matrix:
$$
[F_{i-1}]=[F_{i-2}*a+F_{i-1}*c]
$$
$$
[F_{i}]=[F_{i-2}*b+F_{i-1}*d]
$$
Based on this we can now set values for the letters in order to make the formula work.
$$
M=\begin{bmatrix}0 & 1 \\ 1 & 1\end{bmatrix}
$$
# Template
```C++
const int SIZE=2;

void load_identity(long long I[][SIZE])
//Creates an identity matrix
{
//for every coordinate
    for(int y = 0; i < SIZE; i++)
        for(int x = 0; j < SIZE; j++)
        //if the x and y match put a 1 otherwise a 0
            I[y][x] = (y == x ? 1 : 0);
}

void multiply(long long A[][SIZE], long long B[][SIZE])
//multiplies 2 matrixes of the same size
{
//create an empty product
    long long product[SIZE][SIZE];
	//for every y in A
    for(int i = 0; i < SIZE; i++)
    {
	    //for every x in B
        for(int j = 0; j < SIZE; j++)
        {
	        //initialize the product as 0
            product[i][j] = 0;
			//Use the formula
            for(int k = 0; k < SIZE; k++)
            {
                product[i][j] += (A[i][k]*B[k][j]);
            }

        }
    }
	//copy the product into A
	memcpy(A, product, sizeof(product));

}
void power(long long X[][SIZE], long long power)
{
	//initialize result as an identity matrix
    long long result[SIZE][SIZE];
    load_identity(result);
	//Binary exponentiation
    while(power)
    {
        if(power&1)
            multiply(result, X);

        multiply(X, X);
        power>>= 1;
    }
	//copy the result into X
	memcpy(X, result, sizeof(result));

}
```
# Applications
https://cp-algorithms.com/algebra/binary-exp.html
## Number of paths of length $k$
Get the adjacency matrix of the graph $M$: ($m_{i,j}=1$ if the vertices are adjacent otherwise $m_{i,j}=0$). 
Raise $M$ to the $kth$ power.
$m_{i,j}$ are now the number of paths from $i$ to $j$ of length $k$.
## Minimum cost path of length $k$
Create an adjacency matrix where $\infty$ means that 2 edges arent connected, otherwise write the cost.
When multiplying matrixes instead of doing the sum of the products to the minimum of the sums.

$$
P[i][j]=\min_{n=1}^k=a[i][n]+b[n][j]
$$

Raise $M$ to the $kth$ power.
$m_{i,j}$ is now the min cost of a paths from $i$ to $j$ of length $k$.



# Misc problems