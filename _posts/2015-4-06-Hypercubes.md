---
layout: post
title: Grid coordinates of cubic shells in to n dimensions
---
Recently, I've beeb thinking about grid coordinates in n-dimensional systems. When I first encountered a problem that required the generation of the lattice coordinates in n-space I was temporarily stumped. I was unsure of how to write an algorithm that can generate all points as a function of the dimension and the length of the cube. Previously I always knew what dimensions I was working in and creating the coordinates of a cube of length `n` in three dimensions simply came down to nested loops:

```Python2
length = 10
coordinates = []
for i in range(length):
	for j in range(length):
		for k in range(length):
			coordinates += [(i,j,k)]		
```

This algorithm generates all of the lattice coordinates of a cube in three dimensions. However, it is limited to just that. Since I wanted a more robust algorithm that would return all coordinates based on the lenght as well as the dimension, I started to to think about approaching this problem from a different angle.
