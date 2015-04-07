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
for i in xrange(points):
	cord = ()
	for j in range(d-1,-1,-1): 
		temp = N**j
		if temp<=i:
			cord += (i//temp,)
			i=i%temp
		else:
			cord += (0,)
	cords += [cord]
```


Executing this code for d=3 and n=3 yields the following coordinates:


```python
[(0, 0, 0), (0, 0, 1), (0, 0, 2), (0, 1, 0), (0, 1, 1), (0, 1, 2), (0, 2, 0), (0, 2, 1), (0, 2, 2), (1, 0, 0), (1, 0, 1), (1, 0, 2), (1, 1, 0), (1, 1, 1), (1, 1, 2), (1, 2, 0), (1, 2, 1), (1, 2, 2), (2, 0, 0), (2, 0, 1), (2, 0, 2), (2, 1, 0), (2, 1, 1), (2, 1, 2), (2, 2, 0), (2, 2, 1), (2, 2, 2)]
```