#CP

# Notes

- Try to avoid floating point whenever possible
	- If youre forced to use it try to stick to $+\;-\;*$


# Complex Numbers
To represent 2d points or vectors we use complex numbers.
$a+bi=(a,b)$ or $a+bi=\overrightarrow v =(a,b)$

If we have 2 points $a$ and $b$, the vector $\overrightarrow {ab}$ is $b-a$.

>We can also represent complex numbers with polar coordinates; made up of a radius $r$ and an angle $\theta$.
### Operations with them
We can do operations with them.
Adding or subtracting $\overrightarrow v$ and $\overrightarrow w$ is making $\overrightarrow w$ or its opposite start at the end of $\overrightarrow v$.

![[Pasted image 20240815151939.png]]

Multiplying by a real number $k$ is multiplying the length by $k$.
![[Pasted image 20240815152042.png]]
Multiplying 2 complex numbers it works like normal multiplication, except that $i^2=-1$
$(a+bi)*(c+di)=(ac-bd)+(ad+bc)*i$

## Representing Complex Numbers
![[complex]]

## Transformations
## Translation
If we translate an object by a vector $\overrightarrow v$ we just add  $\overrightarrow v$  to every point.
$$translate(p)=p+ \overrightarrow v$$
```C++
pt point_translate(pt v, pt p){return p+v;}
```
![[Pasted image 20240815154348.png]]

## Scaling
To scale an object by a certain amount $a$ around a center $c$, we need to shorten or lengthen the vectors from $c$ to every point.
$$
scale(p)=c+a*(p-c)
$$
>We are starting from $c$ and then adding to it the vector from $p$ to $c$ multiplied by $a$
```C++
pt point_scale(pt p, pt c, double amount) {
	return c+ (p-c)*amount;
}
```
![[Pasted image 20240815154607.png]]
# Rotation#
To rotate an object by an angle $\theta$ around a center $c$ we ned to rotate the vector from $c$ to every point by $\theta$, aka multiplying by $cos(\theta)+sin(\theta)*i$.

We often use counter clockwise rotation cantered on the origin:
```C++
pt point_rotate(pt p, double theta){return p*polar(1.0,theta);}
```

# Products
## Dot product
The dot product of 2 vectors is a measure of how similar their directions are.

$$
\overrightarrow v \cdot  \overrightarrow w =v_x*w_x+v_y*w_y
$$

>The dot product is positive if the angle is between $0$ and $89$
>$0$ if the angle is $90$ or $180$
>negative if its between $91$ and $179$

```C++
double dot(pt v, pt w) {return v.x*w.x+v.y*w.y};
```

Its used to test if 2 vectors are perpendicular (dot product equal to 0).

It can also be used to check the angle between 2 vectors, using the formula
$$
\theta =cos^{-1}(\:(a\cdot b)\:/\:(|a|*|b|)\:)$$
```C++
double angle(pt v, pt w){
	//clamp clamps the result between -1 and 1
	return acos(clamp(dot(v,w) / (abs(v)*abs(w)),-1.0,1.0));
}
```
## Cross product
The cross product is a measure of how perpendicular 2 vectors are.
$$
\overrightarrow v \times  \overrightarrow w =v_x*w_y-v_y*w_x
$$
```C++
double cross(pt v, pt w) {return v.x*w.y-v.y*w.x};
```

### Finding Orientation
We define $orient(A,B,C)=\overrightarrow{AB}\times \overrightarrow{AC}$.
Its positive if C is on the left of $\overrightarrow{AB}$, negative if C is on the right and 0 if C is on the same line as $\overrightarrow{AB}$.
![[Pasted image 20240815181942.png|300]]
>Its positive if when going from $A$ to $B$ to $C$ we turn counterclockwise.
![[Pasted image 20240815182022.png]]
```C++
double orient(pt a, pt b, pt c){return cross(b-a,c-a);}
```


If we have points $\overrightarrow{A}$ and $\overrightarrow{B}$, and $\overrightarrow{A}\times \overrightarrow{B}>0$, $\overrightarrow{A}$ is to the right of $\overrightarrow{B}$ when looking from the origin.
### Checking if a point is in an angle
We want to know if $P$ is in the angle between $AB$ and $AC$.

- If $orient(A,B,C) =0$ the question is invalid.
- If $orient(A,B,C)<0$ we swap $B$ and $C$ to reverse the sign.
![[Pasted image 20240815182502.png]]

$P$ is in the angle if $orient(A,B,P)\ge 0$ and $orient(A,C,P) \le 0$
>This is because the point is after $B$ but before $C$.
![[Pasted image 20240815182543.png]]
```C++
bool inAngle(pt a, pt b, pt c, pt p){
	if (orient(a,b,c)<0) swap(b,c);
	return orient(a,b,p)>=0 && orient(a,c,p)<=0;
}
```

### Checking if a polygon is convex
To check if a polygon of $n$ points is convex we compute $n$ orientations (wrapping from $n$ to $1$ when needed). 
The polygon is convex if all of the orientations are the same sign.
![[Pasted image 20240815210826.png]]
```C++
bool isconvex(vector<pt> p){
	bool pos=false, neg=false;
	for (int i=0, n=p.size(); i<n;i++){
		int o=orient(p[i],p[(i+1)%n],p[(i+2)%n]);
		if (o>0) pos=true;
		if (o<0) neg=true;
	}
	return !(pos&&neg);
}
```

### Polar sort
Sorting points in the order that they would collide with a rotating beam from an origin.
![[Pasted image 20240905114001.png]]



# Lines
