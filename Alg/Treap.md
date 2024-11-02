#CP
# What 
[[Binary Search Tree]]+[[Heap]]
# Time Complexity
Build/Union in
$$
O(nlog(n))
$$
Insert/Search/Erase
$$
O(log(n))
$$
# When 
Online range queries.
# Logic
Normal [[Binary Search Tree]]s can often be very imbalanced, causing them to have up to $O(n)$ depth.

Inserting in the [[Binary Search Tree|BST]] the elements:
$50,40,30,20,10,15$:
![[Pasted image 20240902131604.png|300]]
If we randomize the order of these elements, on average the tree will be more balanced. 
$30,50,20,40,15,10$:
![[Pasted image 20240902131917.png]]

### Cartesian Trees
Treaps are also called cartesian trees because each node contains 2 values (which we can turn into $(x,y)$ coordinates).

Each node stores:
- A value
	- Satisfies the [[Binary Search Tree|BST]] property (left child smaller then the parent right child bigger)
- A priority
	- Satisfies the [[Heap]] property (both children smaller then the parent).
	- Assigned randomly.

A treap is like a [[Binary Search Tree]] whose values have been sorted by priority before being added.



# Operations
All  operations on a treap can be implemented by using 2 recursive operations:
- __Split__
- __Merge__
![[Pasted image 20240902140840.png]]
#### __Split__
Splits the treap into 2 other treaps by a given value.
All the elements in the left treap are smaller and all the elements in the right treap are bigger.
![[Pasted image 20240902140929.png]]
__Algorithm:__
We want to create 2 treaps: __$left$__ and __$right$__.
- If $root\le x$, both the root and the left subtree are in $left$.
	- We recursively call the algorithm on the right subtree. it will return $left'$ and $right'$.
	- $left$ is the root, the left subtree and $left'$
	- $right$ is $right'$
- Else if $root>x$ both the root and the right subtree are in $right$.
	- We recursively call the algorithm on the left subtree. It will return $left'$ and $right'$
	- $left$ is $left'$
	- $right$ is the root, the right subtree and $right'$
#### __Merge__
If we have 2 treaps, and all elements in the first are smaller then the elements in the second, merges them together.
![[Pasted image 20240902140938.png]]
__Algorithm__:
We need to merge $left$ and $right$.
>the elements of $right$ are bigger then $left$.
- Since the root of the merged treap needs to have the biggest priority, we check the roots of $left$ and $right$.
- If $left.priority>right.priority$
	- We leave $left'$ (the left subtree of $left$) alone, since all the elements of $right$ are bigger then the root, and will thus be placed right.
	- We recursively call the algorithm between $right'$ and $right$.
- Else if $right.priority>left.priority$
	- We leave $right'$ alone.
	- We call the algorithm between $left$ and $left'$.

### Find
Finds a certain element. Works exactly like a normal [[Binary Search Tree]].
- If the element matches return.
- Else if the element is smaller go left.
- Else if the element is bigger go right.
### Insert
Inserts a new element in the tree.
We want to insert $(key,priority)$.
- We traverse the tree searching for $key$.
- Once we reach an index $i$ with a lower priority then _$priority$_, we call $split(i,key)$ (aka we split at the index by $key$).
- We now get a subtree smaller then $key$ and one bigger.
- We turn them into the children of $(key,priority)$.

We will build the tree with $N$ insert calls.
### Erase  
Deletes an element $X$ from the tree.
- We find $X$.
- We replace $X$ with $merge(left\_child,right\_child)$.

### Union
Merges 2 completely different tries.
We want to merge $T_1,T_2$.
- The root of the result needs to have the highest priority (we assume its $T_1$).
- We now have to merge $T_1-left,T_1-right$ and $T_2$ into 2 children of $T_1$
- We call $split(T_2,T_1-value)$, to split it into 2 subtrees.
- We call $union(T_1-left,T_2-left)$, $union(T_1-right,T_2-right)$

# Normal treap Template

```C++
//definition for the vertex.
struct vertex{
	//each vertex has a key and a priority.	
	int key, priority;

	//we need pointers to the children
	vertex* left_child;
	vertex* right_child;

	//we often want to have the count of nodes in each subtree, so we have a variable count initialized as 1
	int count=1;


	//this is a contructor. We can call it like a function to create a vertex with a given key.
	vertex(int key_value){
		//the key is the given one
		key=key_value;
		//since we didnt specify any priority we assign it at random
		priority=rand();
		//by default we leave the left and right children as null
		left_child=NULL;
		right_child=NULL;
		count=1;
	}

	//another constructor but this time we pass in a priority
	vertex (int key_value,int priority_value){
		//assigning the key
		key=key_value;
		//this time the priority is given
		priority=priority_value;
		//by default we leave the left and right children as null
		left_child=NULL;
		right_child=NULL;
		count=1;
	}
}

//when we want to get the count of a node we need another function, because we could call the count of NULL
int get_count(vertex* v){
	//if the vertex exists return its count
	if (t) return t->count;
	//otherwise return 0
	return 0;
}


/*we also want to update the count at the end of
-insert
-erase
-split
-merge
*/
void update_count(vertex* v){
	//if the vertex exists the count is 1 + the count of the left + the count of the right
	if (v)
		v->count=1+get_count(v->left_child)+get_count(v->right_child);
}

//splits treap by value k into 2 treaps: left_treap, right_treap.
//left treap and right treap are passed by reference!
void split (vertex* treap,int key, vertex* &left_treap, vertex* &right_treap){
	//if theres no treap you cannot split it.
	if (!treap) left_treap=right_treap=NULL;
	//if the root is smaller or equal then the key, we only need to split the right
	else if (treap->key <=key){
		/*split the right child by key, into left_treap and right_treap.
		We put left_treap (values smaller then key) inside the right child of treap (since the left child doesnt have anything to do with it) and right_treap (values bigger then key) becomes the new treap.

		*/
		split(treap->right_child,key,treap->right_child,right_treap);

		//the left treap is the original treap after the cut.
		left_treap=treap;
	}

	else{
		/*Split the left child by key, into left_treap and right_treap 
		We put right_treap (values bigger then key) into the left child of the original treap. (these values are bigger then key and so should be in the original treap). meanwhile left_treap is left as the values smaller or equal then key.
		*/
		split(treap->left,key,left_treap,treap->left);
		//the right treap is the original treap after the cut.
		right_treap=treap;
	}
	update_count(treap);
	
}


//inserts a value "new_vertex" in the treap
void insert(vertex* &treap, vertex *new_vertex){
	//if the treap doesnt exist and we try to insert a value, the treap IS the value
	if (!treap) treap=new_vertex;

	/*if we reach a subtree where the priority of the new_vertex is 
	greater then the root, this is where we want to insert it. 
	We split by the new_vertex key and then substitute the root with new_vertex. (The 2 new treaps become the children) */
	else if (new_vertex->priority>treap->priority){
	split(treap,new_vertex->key,new_vertex->left_child,new_vertex->right_child);
	
	new_vertex=treap;
	}
	//otherwise we want to continue going down normally
	else{
		//if the key we want to insert is smaller or equal then the root we want to go left
		if (treap->key <= new_vertex->key) 
			insert(treap->left_child,new_vertex);
		//otherwise we go right
		else
			insert(treap->right_child,new_vertex);
		
	}
	update_count(treap);
}

//merges left_treap and right_treap into treap.
//WE ASSUME THAT EVERY VALUE IN left_treap IS SMALELR THEN right_treap
void merge(vertex* &treap, vertex* left_treap, vertex* rigth_treap){
	//if left_treap doesnt exist the resulting treap is just r
	if (!left_treap) treap=right_treap;
	//same thing if the right treap doesnt exist
	else if(!right_treap) treap=left_treap;

	//otherwise both exist

	/*if the left treap has a higher priority the root becomes the left 
	treap. since every value in left treap is smaller then right treap, we 
	want to merge it with the right children of left treap*/
	else if(left_treap->priority>right_treap->priority){
		//into the right child of left treap we put the union between the right child of left treap and right treap
		merge(left_treap->right_child,left_treap->right_child,right_treap);
		//the complete treap becomes left treap
		treap=left_treap;
		}
	//otherwise right_treap has a higher priority so we put it as the root
	else{
		//we put in the left child of right treap the union between the left child of right treap and left treap
		merge(right_treap->left_child,left_treap,right_treap->left_child);
		treap=right_treap;
	}
	update_count(treap);
}


//erases from the treap KEY
void erase (vertex* &treap,int key){
	//if we have found the vertex
	if (treap->key==key){
		//we want to delete the current node. we create a new pointer to store the vertex we want to remove.
		vertex *to_delete=treap;
		//we merge the children of treap.
		merge(treap,treap->left_child,treap->right_child);
		//we delete the pointer, thus deleting the original one as well
		delete to_delete;
	}
	//otherwise we need to move down the treap
	else{
		//if the key is smaller or equal we go left
		if (key<=treap->key)
			erase(treap->left_child,key);
		else 
			erase(treap->right_child,key);
	}
	update_count(treap);
}
//returns a pointer to the root of the union between left_treap and right treap
vertex* unite(vertex *left_treap, vertex *right_treap){
	//if left_treap doesnt exist the union is just right_treap
	if (!left_treap) return right_treap;
	//same if right doesnt exist
	else if (!right_treap) return left_treap;

	//we assume that left_treap has higher priority. if thats not the case we swap
	if (left_treap->priority<right_treap->priority) swap(left_treap,right_treap);

	/*we now want to join right_treap with the children of left_treap.
	First off we need to split right_treap based on the root. We create 2 new variables to store the split trees*/

	vertex* left_split, right_split;
	split(right_treap,left_treap->key,left_split,right_split);

	//the left child of the left treap is the union between itself and the left split of right treap.
	left_treap->left_child=unite(left_treap->left_child,left_split);

	//the right child of the left treap is the union between itself and the rigth split of the right treap
	left_treap->right_child=unite(left_treap->right_child,right_split);

	return l;
}

```


# Implicit Treaps
## What
An array that can in $O(logn)$:
1. Query an index
2. Insert an element anywhere
3. Remove any element
4. Answer range queries on any interval
5. Support range updates.
6. Reverse any interval.

## Concept
The keys of the treap should be the (0 based) indices of the elements in the array. 
>If we stored them explicitly when we inserted a value we would need to do $O(n)$ updates.

The _implicit_ key of a node (or index of an array) is the number of nodes with a smaller key. 
So the key of a node $N$ is the size of its left subtree, and for every ancestor where $N$ is in the right subtree we add $1$ __PLUS__ its left subtree.
>Because both the ancestor and the left subtree are smaller then $N$.

$key(N)=size(N->L)+ size(Parent->L)+1$
![[Pasted image 20240903152154.png]]
>In this case to get the key $5$ we do the size of the left subtree ($1$) and since its in the right subtree of key $3$ we do $(+1)$ ($+3$)
>$1+1+3=5$

To get the _implicit_ key of a node we can:
- Maintain the current sum since we get to the nodes from the top down
- Whenever we go left the sum doesnt change.
- Whenever we go right the sum increases by the left subtree $+1$.


### Querying an index/Merging/Erasing
To query we simply traverse the [[Binary Search Tree||BST]] as normal.
![[Pasted image 20240903152357.png]]
Merging/Erasing doesnt change.
### Splitting
Now whenever we split the tree we get all of the elements with indexes bigger/smaller then $n$.
![[Pasted image 20240903152439.png]]


### Inserting an element
We need to insert element $n$ at position $p$.
- We split the tree at $p$ into $left$ and $right$
- We merge $n$ into $left$.
- We merge back $left$ and $right$.
![[Pasted image 20240903152415.png]]
![[Pasted image 20240903182813.png|400]]



### Range queries in $O(logn)$
We can use treaps to answer range queries 
>Like more powerful [[Segment Tree]]s

Each node stores the value of all of its children and itself.
We can update this like the size of the subtrees.

To calculate the interval $[left\_query,right\_query]$:
1. we split the tree by $left\_query$ into $left\_useless$ (smaller then $left\_query$) and $left\_tree$ (bigger then $left\_query$).
2. We split $left\_tree$ by $right\_query$ (or $right\_query+1$) into $right\_useless$ (bigger then $right\_query$) and $final\_tree$.
![[Pasted image 20240903134024.png]]

### Lazy propagation
For each node we store a lazy value (means that the node is updated but the children arent).
Whenever we go down the tree we push the value.

### REVERSING
We add a boolean that is true if the subtree has to be reversed.
When we push this we swap the children and pass on the boolean.

## Template for reversing
#### IF IN ANY FUNCTION A VALUE HAS AN `&` IT MEANS THAT THE ANSWER IS RETURNED THERE!
```C++
//definition for the vertex
struct vertex{

	//the vertex needs a priorty (assigned at random)
	int priority=rand();

	//this is NOT the key (thats the index), this is just the value the vertex holds
	int val;

	//size of subtree
	int size=1;

	bool reverse=false;

	//pointers to children
	vertex* left_child=NULL;
	vertex* right_child=NULL;
}

//returns the size of a vertex (we need this because we might run into a NULL)
int get_size(vertex* v){
	//if it exists return the size otherwise 0
	if (v) return v->size;
	return 0;
}

//updates the size of a vertex based on its children. WE NEED TO ADD THIS TO THE END OF EVERY FUNCTION
void update_count(vertex* v){
	if (v) v->size=get_size(v->left_child)+get_size(v->right_child)+1;
}
//pushes the reverse boolean. WE NEED TO ADD THIS TO THE START OF EVERY FUNCTION
void push(vertex* v){
	//if v exists and has the boolean
	if (v&&v->rev){
		//sets it to false and swaps the children
		v->rev=false;
		swap(v->left_child,v->right_child);
		//for each children if they exist XORs their reverse (if it was already true sets it to false and viceversa)
		if (v->left_child) v->left_child->rev^=true;
		if (v->right_child) v->right_child->rev^=true;
		
	}
}
//merges left and right treap into one

void merge(vertex* &treap,vertex*left_treap,vertex*right_treap){
	//ALWAYS PUSH
	push(left_treap);
	push(right_treap);

	//if one of them doesnt exist the result is just the other
	if (!left_treap) treap=right_treap;
	else if (!right_treap) treap=left_treap;

	//otherwise if the left priority is higher we want to append right to the left child of left_treap 
	else if(left_treap->priority>right_treap->priority)
		merge(left_treap->left_child,left_treap->left_child,right_treap);
		//the final treap starts from the left child
		treap=left_treap;
	//otherwise we do the opposite
	else{
		merge(right_treap->right_child,left_treap,right_treap->right_child);
		treap=right_treap;
	}	
	update_count(treap);
}


//splits t into left_treap and rigth_treap based on target_key. We also need to keep track of the current sum, because we are keeping _implicit_ indexes, so we have to calculate them
void split(vertex* treap, vertex* &left_treap, vertex* &right_treap, int target_key, int current_sum)
{
	//if theres no treap you cant split
	if (!treap) left_treap=right_treap=NULL;

	//ALWAYS PUSH
	push(treap);

	//we need to calculate the current index. Its the current sum (elements above it where its in the right subtree) plus its left child
	int current_index=current_sum+treap->left_child;

	//we have 2 options:
	//the target index is outside the current index (we need to split the left child, (and we are going right so we update the sum to the left child +1))
	if(current_index<=target_index)
		//we are going to split the rigth child into left_answer and right_answer. The right child becomes the new treap. We also put left_answer into the right child (still connected to the original treap) and right_answer becomes right_treap
		split(treap->right_child,treap->right_child,right_treap,target_key,current_sum+treap->left_child+1);
		//the left treap becomes the original treap (the left child+the left answer of the right child)
		left_treap=treap;
}
	//otherwise we do the opposite but we arent going right so we dont update
	else{
		split(treap->left_child,left_treap,treap->right_child,target_key,current_sum);
		right_treap=treap;
	}
//insertes v into position p
void insert(vertex* &treap,int p,vertex* v){

	//if the treap doesnt exist it becomes v
	if (!treap) treap=v;
	else{
	push(treap);
	//we first create 2 pointers for the results of the split
	vertex* left_treap, right_treap;
	//we split by p
	split(treap,left_treap,right_treap,p);
	//we merge left_treap with v
	merge(left_treap,left_treap,v);
	//we merge left_treap and right_treap into treap
	merge(treap,left_treap,right_treap);
	update_count(treap);
	}
	
}

//reverses all items between left_bound and r_bound
void reverse(vertex*treap, int left_bound, int right_bound){
	
	vertex* treap_1,interval,treap_3;
	//we split the tree based on left bound
	split(treap,treap_1,interval,left_bound);
	//we split interval into interval and treap_3 based on( right_bound-left_bound+1) aka the number of elements (used because of implicit keys)
	split(interval,interval,treap_3,right_bound-left_bound+1);
	//sets the bool in interval to be true
	interval->rev=true;
	//merges everything back
	merge(treap,treap_1,interval);
	merge(treap,treap,treap_3);

}
```
# Notes
You can also get the intersection between 2 treaps!

