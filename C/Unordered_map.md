>A python dictionary. 

Use when space complexity doesnt allow an array.

Stores key-value pairs without sorting them. 
>Works based on hash tables.
```C++
unordered_map<key_type,value_type> map;


```


```C++
//setting a value.
map[5]=12;
```


WITCHCRAFT TO MAKE A MAP FASTER
```C++
//1024 is just a power of 2. (depends on the number of insertions).
mp.reserve(1024);
mp.max_load_factor(0.25);
```

# Methods

- `map.find()` returns an iterator to the element
- `map.erase()` deletes an element
- `map.empty()` checks if the map is empty
- `map.count()` returns the size.