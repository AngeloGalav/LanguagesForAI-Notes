# Continuing P and FP
## The class EXP
What can we find if we try to go beyond the class $P$, i.e., if we allow TMs to have more time at their disposal?
- The next class of functions $T : N → N$, beyond the polynomials and having nice closure properties is the class of _exponential functions_.
- The classes $EXP$ and $FEXP$ are defined as follows:![[exp_fexp.png]]
- The tasks in these classes _can be solved mechanically_, but possibly _cannot be_ _solved efficiently_. Of course, it holds that:
$$
P \subseteq EXP, \ FP \subseteq FEXP
$$
>[!Theorem]
>The two inclusions above are strict.
>- Meaning, we can find something in this set -> $FP$ is not empty.  

There's a huge gap between $P$ and $EXP$. 
Polynomials grow wayyy slower than the exponentials. 

>[!WARNING]
>$EXP$ does not necessarily mean that the solution is difficult, but it means that the problem _might_ be difficult.  
> - It only means that it exponential in the worst case.

![[fexp_exp_set.png]]

If the problem it's outside of EXP, then it might not even be worth trying to solve it. 

# Excercises 

## Excercise 1
Prove that the function MINMAX which given (the encoding of) a sequence of natural numbers $(a_1, ..., a_n)$ returns both the least and the greatest elements in the sequence is in $FP$. 
Example: Input -> (11,23,3,7,2) the function MINMAX should produce the pair (2,23). 

### Solution 
We can describe the solution of this problem by way of some pseodocode. 

Input: a sequence $(a_1, ..., a_n)$, $a_i \in N$ appropriately enconded. 
Output: $(b, c)$ where $b$ is the least and $c$ is the greatest of the $a_i$. 

```
min <- a1
max <- a1

for i <- 2 to n do: 
	if (min > ai) :
		min <- ai
	if (max < ai) : 
		max <- ai
endfor

return (min, max)
```

>[!WARNING]
>If there's an `elif`, you should consider the most complex set of instruction. 

![[alg_stat.png]]
Since the algorithm above exhaustively look for the least and greatest elements in $(a_1,..., a_n)$, it is correct by construction. 

Let us then consider the three requirements we have to check to be sure that the algorithm above works in polynomial time. 

- About the number of instructions, they are at most: $(2 + O(n) + 4 + 1) \in O(n)$. 
	- This is polynomially bounded, then. 
- About the size of intermediate results, besides the input itself, we have to look at the size of auxiliary variables (which are `min` and `max`). Both of them, at any time contain one of the elements in the input list. In other words, their size is always bounded by the size of the input. 
- About the time necessary to execute each instruction, the only instruction which deserves to be considered are the comparisons, but we know that comparing natural numbers can be done in polynomial time in their sizes.   
- We can then conclude that $MINMAX \in FP$. 

## Excercise 2
Prove that the wedding plan problem as we have described it at the beginning of this module, is in the class $FEXP$. 

### Solution
First of all, let me recall how the problem is defined:
given a list of invitees, and a list of incompatibility constraints between invitees, one wants to build a sub-list of invitees which are compatible with each other and which has maximal length. 

Let us show how this problem is an instance of a problem about graphs. 
![[solution_wedding.png]]
And the descriptor of the graph would be:
$G$  = ({philip, christine, paul, thomas, jules}, {{philip, christine}, {thomas, jules}, {paul, jules}})

The problem then refers to undirected graphs, namely pairs $(V, E)$ where $V$ is a finite set of vertices/node and $E$ is a subset of $\{\{l, m\} | l, m \in V, l \ne m\}$. 
In an undirected graph $G = (V, E)$, whenever $v \in V$, we write $N(v) = \{w | \{v, w\} \in E\}$. 

The wedding problem, then, can be spelled out as follows: given an undirected graph $G = (V,E)$, determine a subset of $V$, call it $W$ such that:
1. For all $v \in W$,  $N(v) \cap W = \emptyset$ 
2. $|W|$, namely the number of nodes in $W$, is _maximum_ among that of all subsets of $V$ satisfying (1) 

The function we want to prove in $FEXP$ then is a function:
- $\text{FINDSET} : \text{GRAPHS} \rightarrow \text{FINITE-SETS}$

The algorithm we are looking for, thus, looks as follows:
- INPUT: an undirected graph $G = (V,E)$. 
- OUTPUT: $W \subseteq V$ satisfying 1. and 2. above.

```JS
Z <- empty
foreach W subset of V do
	ind <- True
	foreach w in W do
		if N(w) intersects W then // no element of w is connected to an 
									   // element in W
			ind <- False
	if ind then 
		if |Z| < |W| then 
			Z <- W
endfor
return Z
```
![[cost_of_each.png]]
Number of instructions:
$$2 + O(2^{|V|}) \cdot (4+O(|V|) \cdot 2) \in O(2^{|V|}) \cdot O(|V|) \in O(2^{|V|^2})
$$
As we can see, the problem $\in FEXP$.

>[!WARNING]
>This is basic stuff, but remember that the number of subsets of a set is $2^{|V|}$.

