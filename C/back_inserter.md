#C
An iterator that inserts new elements at the end of a container.
```C++
back_inserter(container)
```

for example
```C++
copy(arr1.begin(),arr1.end(),back_inserter(arr2))
```

```C++
merge(arr1.begin(),arr1.end(),arr2.begin(),arr2.end(),back_inserter(arr3))
```
