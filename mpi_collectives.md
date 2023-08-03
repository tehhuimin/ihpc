# MPI Collectives

SPMD model (Single program, multiple data) -> running the same program in multiple processes (nodes)



## All to one reduce

![image-20230730180947162](./assets/image-20230730180947162.png)

![image-20230730180957623](./assets/image-20230730180957623.png)

```
// Assume P = 2^k
let s = local value
bitmask <- 1
while bitmask < P do
	PARTNER <- RANK ^ bitmask
	if RANK & bitmask then
		sendAsync(s->PARTNER)
		wait(*)
		break // once sent, drop out
	else if (PARTNER < P)
		recvAsync(t<-PARTNER)
		wait(*)
		s <- s + t // reduce
	bitmask <- (bitmask << 1) // shift left one bit
if RANK = 0 then print(s)

```

![image-20230730181337950](./assets/image-20230730181337950.png)



## One to All Broadcast

![image-20230730181714436](./assets/image-20230730181714436.png)

```
broadcast(A[1: m.P], root)
	let B[1:m][1:P] <- reshape(A)
	let T[1:m] = temp array
	scatter(B, root, T)
	allGather(T, B, root) // use bucketing for all gather
	A <- reshape(B)
```

![image-20230730185430293](./assets/image-20230730185430293.png)

## Scatter & Gather 

![image-20230730181740866](./assets/image-20230730181740866.png)

### Scatter Implementation #1: Naive

Tmsg = (alpha + beta * m) * P

```
scatter(In[1:m][1:P], root, Out[1:m])
	if RANK == root then
		for i != root do 
			sendAsync(In[:][i], i)
	else 
		recvAsync(Out[:], root)
	wait(*)
```

Scatter Implementation #2: Divide and Conquer

![image-20230730183255425](./assets/image-20230730183255425.png)

![image-20230730183325675](./assets/image-20230730183325675.png)

![image-20230730183349654](./assets/image-20230730183349654.png)

## All-Gather & Reduce-Scatter 

![image-20230730181756359](./assets/image-20230730181756359.png)

All Gather 

```
// All Gather pseudocode
gather(In, Out, root)
broadcast(reshape(Out), root)
```

![image-20230730182419638](./assets/image-20230730182419638.png)



## All-Reduce 

Bandwidth optimal all-reduce with reduce-scatter and all-gather

![image-20230730185525146](./assets/image-20230730185525146.png) 