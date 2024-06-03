#Definitions
A graph is bipartite if you can color it with 2 colors, such that no 2 adjacent nodes have the same color.
![[Pasted image 20240511121227.png]]
We pick the first node for each set and assign it path length 0. For each other node its **Parity** refers to whether the length of the path from the start is even or odd:
- **Even** length: 0 parity
- **Odd** length: 1 parity
A graph is bipartite if it doesnt contain an odd lenght cycle (Since you alternate parity it would require the beginning to be both even and odd).
>A triangle isnt bipartite.
# Calculating parity with XOR

Given the parity/length of $n$ segments, the total parity is their XOR:
$$
abc=a⊕b⊕c
$$
$$
ab=a⊕b
$$
When calculating the total parity from 2 segments we want to know if it will be odd or even, depending on if the starting segments are odd or even.

>We know that
Odd+Odd=Even
Even+Even=Even
Even+Odd=Odd
Odd+Even=Odd

This is EXACTLY LIKE XOR. This is why it works.

>$1⊕1=0$, 
>$0⊕0=0$
>$1⊕0=1$
>$0⊕1=1$ combined path is odd (1).



https://cp-algorithms.com/graph/kuhn_maximum_bipartite_matching.html