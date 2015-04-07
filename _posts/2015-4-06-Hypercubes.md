---
layout: post
title: "Grid coordinates of cubic shells in n-dimensional space"
published: true
---

Recently, I've been thinking about grid coordinates in n-dimensional systems. When I first encountered a problem that required the generation of the lattice coordinates in n-space I was temporarily stumped. I was unsure of how to write an algorithm that can generate all points as a function of the dimension and the length of the cube. Previously I always knew what dimensions I was working in and creating the coordinates of a cube of length _N_ in three dimensions simply came down to nested loops:


```python
coordinates = []
for i in range(length):
	for j in range(length):
		for k in range(length):
			coordinates += [(i,j,k)]		
```


This algorithm generates all of the lattice coordinates of a cube in three dimensions. However, it is limited to just that. Since I wanted a more robust algorithm that would return all coordinates based on the length as well as the dimension, I started to to think about approaching this problem from a different angle. Let us a consider a cube of length _N_ in d-dimensional space. First I calculated the total number of points in this cube _N**d_. now instead writing nested loops (one for each dimension) I wrote a single one to loop through the number of points I determined. For each iteration in the loop I change bases from base 10 to the base _N_. This works because we know that in a cube of length _N_ the only possible coordinate values are 0,1,...,n-1. The nested loop ensures that each point is described in _d_-dimensional space.


```python
points = N**d
coordinates =[]
for point in xrange(points):
	cord = ()
	for dimension in range(d-1,-1,-1): 
		N_d = N**dimension
		if N_d<=p:
			cord += (point//N_d,)
			point=point%N_d
		else:
			cord += (0,)
	coordinates += [cord]
```


Executing this code for d=3 and n=3 yields the following coordinates below. However, what has really happened here is that we converted a decimal numbers into base _d_ numbers.


```python
[(0, 0, 0), (0, 0, 1), (0, 0, 2), (0, 1, 0), (0, 1, 1), (0, 1, 2), (0, 2, 0), (0, 2, 1), (0, 2, 2), (1, 0, 0), (1, 0, 1), (1, 0, 2), (1, 1, 0), (1, 1, 1), (1, 1, 2), (1, 2, 0), (1, 2, 1), (1, 2, 2), (2, 0, 0), (2, 0, 1), (2, 0, 2), (2, 1, 0), (2, 1, 1), (2, 1, 2), (2, 2, 0), (2, 2, 1), (2, 2, 2)]
```

Now lets extend this idea into shells around the origin (0,0,...,0). In the previous example the origin was in the corner of the cube. We now want to move it to the center. we can do this by subtracting N//2 from each coordinate where N is the width of the shell the point is a part of. Additionaly we also want build the cube from the center incrementally. First we look at the origin which has a width of 1, the first shell has a width of 3 followed by a width of 5. We see that the width of the shells is givgen by _2*n+1_ where n is the shell number. Again, lets look at a cube of length _N_ in d dimensions.


```python
shells = []
for N in range(1,2*length+1,2):
	cords = []
	points = N**d
	for point in xrange(points):
		cord = ()
		for dimension in range(d-1,-1,-1):
			N_d = N**dimension
			if N_d<=point:
				cord += (point//N_d-N//2,)
				point=point%N_d
			else:
				cord += (0-N//2,)
		coordinates += [cord]
	if len(shells)>0:
		for i in range(len(shells)):
			coordinates = list(set(coordinates)-set(shells[i]))
		shells+=[coordinates]
	else:
		shells += [cords]
```

Not much has changed except for the movement of the origin and building up full shells. Lets print the first three shells in 2 dimensional space:


```python
Shell:  1
[(0, 0)]
Shell:  2
[(0, 1), (-1, 1), (-1, 0), (-1, -1), (0, -1), (1, 0), (1, -1), (1, 1)]
Shell:  3
[(1, 2), (-2, 2), (-2, 1), (-1, 2), (-2, 0), (2, 2), (2, -1), (-1, -2), (2, 1), (-2, -1), (2, 0), (2, -2), (-2, -2), (0, -2), (1, -2), (0, 2)]
```

Lets plot the results from _N=3_ and _d=2_ points to verify we have really obtained the correct points for each shell:

![2D shells]({{ site.baseurl }}/images/output_ST8wcz.gif "an image title")

And _N=3_ and _d=3_:

![2D shells]({{ site.baseurl }}/images/output_73UcYH.gif "an image title")

As we cand see we now have a working algorithm that produces the shells of a cube in any dimension. Currently, the code is fairly inefficient as it reproduces all coordinates for each shell. I will think about whether there is a good way of  producing just the shell coordinates later.

