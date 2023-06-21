# Exercises

## Exercise 1
We want to prove that:
$$
L = \{(G, k) | \exists f : v \rightarrow \{1,..., k\} \text{ such that } f(v) \ne f(w) \text{ if } (v, w) \in E\} \in NP
$$
This is the graph coloring problem. 
We need to use a certificate. 
- A certificate is a function $f$ such that $f: V \rightarrow \{1,..., k\}$ (the coloring function)
- Our verifier is such that:
	- INPUT -> $(G, k), f$
	- OUTPUT -> returns 1 if $f$ is a correct coloring, meaning iff $f(v) \ne f(w)$ for any $(v,w) \in E$

- Our certificate is polynomial in size since it has size $|V|$. 

- Our verifier should work in polynomial time. A simple algorithm that has the same semantic is this:

For each $(v, w) \in E$:
	if $f(v) \ne f(w)$ then return 0
return 1

Since the algorithm works in polynomial time (it has $O(|E|)$ simple instructions), _we just proved that it is in NP_. 

```
INPUT: (G, k)
	f = []
	for each v in V :
		choose i in {1, .., k}
		f.push((v,i))
		
	for each (u, v) in E:
		if f(v) == f(w) return 0

	return 1
```

If this algorithm is polytime and there exists at least one (non-deterministic) path that returns 1, then $L \in NP$. 

We'll now prove that. 
This algorithm is polytime:
- $O(|V|+ |E|)$ instructions
- Size of intermediate results: $f$ is polynomial in size ($O(|V|)$)
- Each instruction can be done in polytime. 
- Because of the `choose` keyword, it has a non-deterministic path that returns 1.  

So $L \in NP$.


## Exercise 2
$L = \{(n, m, k)\} | n,m \text{ are natural numbers with at least } k \text{ prime factors in common }\} \in NP$. 

- certificate: A list $p_1,..., p_k$ of integers. 
- verifier: 
	- INPUT: $(n,m,k), p_1,..., p_k$
	- OUTPUT: return 1 iff $p_1,...,p_k$ are prime numbers and $p_1, ...,p_k$ are factors of both $n$ and $m$. 

$p_1, ..., p_k$ is polysize $\le (k|n|)$.
- They are polysize since they are _always_ smaller than $n$ in quantity.  
We go to the verifier route, we need to find an algorithm to verify the solution. 

INPUT: $(n,m,k), p_1,...,p_k$

For each $1 \le i \le k$: 
	Verify that $p_i$ is prime (Aks algorithm)
	if $p_i$ is not prime, return 0
if $n \% (p_1 \cdot p_2 \cdot p_3 \cdot ... \cdot p_k) \ne 0$ then return 0
if $m \% (p_1 \cdot p_2 \cdot p_3 \cdot ... \cdot p_k) \ne 0$ then return 0
return 1


The algorithm is polytime, since:
- $O(k)$ instructions.
- Each instruction is polytime (especially verifying that a number is prime).
then $L \in NP$.

## Exercise 3
$L = \{(G, u, v) | \text{There exists a path in } G \text{ from } u \text{ to } v\}$

This is basically the reachability problem. 

Certificate: a sequence $u, w_1, ..., w_n, v$.
We ask that $n \le |V|$ (because if a path from $u$ to $v$ exists, then there is one with length $\le |V|$)

Verifier: Input: (G, )