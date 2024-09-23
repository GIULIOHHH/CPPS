#Definitions 
A binary tree (each parent has 2 children), where:
- the left child contains only values smaller then the parent
- the right child contains only values bigger then the parent.
![[Pasted image 20240902130948.png]]



## Constructing a binary search tree:
Lets say we have the array
$30,50,20,40,15,10$.

1. We start with 30 as the root.

2. We move on to the next element, 50. 50 is bigger then 30 so we add it to the right.

3. 20 is smaller then 30 so we add it to the left.

4. 40 is bigger then 30 so we go left. 40 is smaller then 50 so we go left.
![[Pasted image 20240902131917.png]]