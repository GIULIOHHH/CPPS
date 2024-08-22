#C
To represent 2d points we can use `complex<type>`

```C++
//renaming complex<double> to just point
typedef complex<double> pt;


//macros for getting the real part(x coord) and the imaginary (y coord)
#define x real()
#define y imag()

```

- We cannot modify only one coordinate, we need to modify both of them.
- We can do all operations with these.
	- If we multiply or divide with real numbers they have to be of the same type as the complex numbers.
	- For example if i have `complex<double>` to multiply by $2$ i have to write it as $2.0$

>For example 
>`pt p{10,4};`
>`cout<<p.x<<p.y;`
>`b=p*2.0+a;`

We can also get the polar coordinates
```C++
pt p{10,4};
abs(p); //gets the radius r
arg(p); //gets the angle theta
```