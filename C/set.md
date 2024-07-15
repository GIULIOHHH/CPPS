#C 
- Stores unique values in sorted order.
- Elements cant be modified, but can be removed and reinserted.
- Values are unindexed.
- Allows insertion, checking and deleting in $O(log(n))$

- `set.begin()` is an iterator to the first element.
- `set.rbegin()` is an iterator to the last element. (reverse begin).
- `set.end()` is an iterator to the element after the last.
- `set.size()` returns the size.
- `set.empty()` returns if the set is empty.
- `set.insert(val)` inserts a value.
- `set.erase(val)`/`set.erase(first,last)` removes elements. 
	- Does nothing if the val isnt there.
- `set.clear()` removes all elements.
- `set.find(val)` returns an iterator to the value. If it isnt found returns an iterator to the end.
```C++
//this sorts the elements in descending order
set<int, greater<int> > s1;
s1.insert(104);
```