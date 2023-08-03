# Breadth First Search



## Sequential BFS

```
BFS(G=(V, E), seV)
	D[V-{s}] <- inf // initialize all distances to infinity
	D[s] <- 0
	F <- {s}
	while F != 0 do // when F is not empty 
		v <- extract-one(F)
		for (v, w) e E do // loop over all v's neighbours
			if D[w] = inf then
				D[w] <- D[v] + 1
				F = F U {w}
	return D // D[x] = dist(s->x)
	
```

![image-20230731141059732](./assets/image-20230731141059732.png)

![image-20230731141153117](./assets/image-20230731141153117.png)

## Level Synchronous BFS

The Idea: 

![image-20230731141929301](./assets/image-20230731141929301.png)

![image-20230731141915558](./assets/image-20230731141915558.png)

Parallel BFS: 

1. Level synchronous
2. Process an entire level in parallel

```
Parallel BFS (G=(V, E), seV)
	D[v] <- inf
	D[s] <- 0 
	l <- 0
	F_0 <- {s}
	while F_l != 0 do 
		F_(l+1) <- {}
		processLevel(G, F_l, F_(l+1), D)
		l <- l + 1
	return D
```

![image-20230731142229141](./assets/image-20230731142229141.png)

- Span <= Diameter of graph



Bag to hold Frontiers

- Unordered collection
- With repetition

![image-20230731142702626](./assets/image-20230731142702626.png)