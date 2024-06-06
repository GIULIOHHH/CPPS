#CP #C 
# Fast IO
```C++
int main()
{
ios::sync_with_stdio(false);
cin.tie(nullptr);

freopen("input.txt", "r", stdin);  
freopen("output.txt", "w",stdout);

//we also need to cout endl.
```

## printf()
```C++
//prints a decimal
printf("%d", int);
//prints a decimal and goes to new line
printf("%d\n", int);
//prints a float
printf("%f")
//prints a double
printf("%lf")
//prints 2 ints
printf("%d %d", a,b)
//prints 3 ints
printf("The integers are: %d, %d, and %d\n", a, b, c);
//prints a string
printf("%s" string.c_str());
```

## scanf()
```C++
//scans 3 ints
int a,b,c;
scanf("%d %d %d",&a,&b&c);

//reads a string
char str[100];
scanf("%99s", str);
string s(str);
```