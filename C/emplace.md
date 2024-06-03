#C 
```C++
emplace(position,value)
```
Inserts a value in an array.
```C++
v.emplace(v.begin+5;x)
```

To place it at the end you can `emplace_back`, its faster then `push_back`.
```C++
v.emplace_back(x)
```
