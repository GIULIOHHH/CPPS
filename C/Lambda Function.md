#C 
FIX LATER 



```
[capture list](parameters) -> return value { body }
```

one) Capture List: simple! We don't need it here, so just put `[]`

two) parameters: simple! e.g. int x, string s

three) return value: simple again! e.g. pair<int, int> which can be omitted most of the times (thanks to [Jacob](https://codeforces.com/profile/Jacob "International Master Jacob"))

four) body: contains function bodies, and returns return value.

e.g.

```
auto f = [] (int a, int b) -> int { return a + b; };
cout << f(1, 2); // prints "3"
```

You can use lambdas in `for_each`, `sort` and many more STL functions:

```
vector<int> v = {3, 1, 2, 1, 8};
sort(begin(v), end(v), [] (int a, int b) { return a > b; });
for (auto i: v) cout << i << ' ';
```