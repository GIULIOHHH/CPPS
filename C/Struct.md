#C
Basicaly OOP in C++.

```C++
struct my_struct{
var_type1= var_name1;
var_type2= var_name2;
};

//defining an instace
my_struct a;
//accessing variables
a.var_name1=4;
```

You can write your own function to compare structs.
```C++
struct Edge{
int a,b,weight;
//operator returns true or false. 
//It compares another Edge called other.
bool operator < (Edge const &other){
	//it returns a bool.
	return weight<other.weight;
}
};
```

```C++
//Creates a tree of nodes
struct Node{
	int max;
	long long sum;
} Tree[SIZE];
```

With structs we can use [[Constructor]]s.
With pointers to structs we can use the [[Arrow Operator]].