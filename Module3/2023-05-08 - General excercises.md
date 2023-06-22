# General excercises
## Theory questions
##### q2
Turing machines:
- [v] Are such that certain problems that can be computed in time $n^2$ cannot be computed in time $n$
- [ ] All work in polynomial time
- [v] Can contain an arbitrary number of working tapes
- [ ] All work in exponential time

I'm not even gonna write an explanation for this tbh. 

##### q3
The Halting problem is:
- [v] Undecidable but NP-hard
- [ ] Nothing about its computational complexity is known (no, look below)
- [ ] Decidable in polynomial time (no: it's undecidable)
- [ ] NP-complete (no: it's not in NP)

If a problem is undecidable, it certainly cannot be in NP. But, we can still prove that it is NP-hard (i.e. proving that it's "at least as hard as" another problem).
For example, the halting problem can be reduced to 3SAT, in this way:

We begin by noting that all NP-complete problems are reducible to 3SAT. Now we have a Turing machine that iterates over all possible assignments, and if a satisfying assignment is not found then it runs forever. This machine halts if and only if the 3SAT instance is satisfiable.

Completing the proof - we can, in polynomial time, reduce any instance of an NP-complete problem to 3SAT. From there, we can reduce this problem to an instance of the halting problem by pairing the input with a description of the Turing machine described above (which has constant size). This pairing can be done in polynomial time, because the Turing machine has only constant size. Then, the original NP-complete problem has answer "yes" iff __3SAT instance is satisfiable iff the Turing machine halts on the given input__.

##### q4
Suppose that a language $L$ is both in $P$ and $NP$-complete. Then:
- [v] The classes EXP and P are different 
- [ ] The classes EXP and NP are equal (no, look below)
- [ ] The classes P and NP are different (no, look below)
- [v] The clases P and NP are equal

1) Remember, $P \ne EXP$, and $NP \ne NEXP$ (this is basically always true).
2) If $L \in P$ and $L$ is _NP-hard_, it means $NP \subseteq P$. In this case, P = NP.
	Also, if $L$ is NP-complete, then $L \in P \iff NP = P$. 

##### q5
The notion of PAC-learnable concept class:
- [v] Does not make any reference to the complexity time of the learning algorithm.
- [v] Needs to hold for every distribution $\text{D}$ on the instance class.
- [ ] Cannot be reached when the underlying concept class is the conjuction of literals. (No: this is true when it is a _disjuction_ of literals)
- [ ] Requires the output concept to have probability of $\epsilon$ in all cases. (No: it has to be $< \epsilon$)

1) Time (and space) complexity is not mentioned in the definition of pac-learnable.  
2) Meaning, on the instance space. To revise: 
Let $C$ be a _concept class_ over the _instance space_ $X$. 
We say that $C$ is __PAC learnable__ iff there is an algorithm $A$ such that for every $c ∈ C$, _for every distribution_ $D$, for every $0 < ε < \dfrac{1}{2}$ and for every $0 < δ < \dfrac{1}{2}$ , then 
$$\text{Pr}[error_{D,c}(A(EX (c, D), ε, δ)) < ε] > 1 − δ$$
where the probability is taken over the calls to $EX (c, D)$.

## Real mfking excercises 
#### es1
Construct a TM which decides the following language:
$L = \{w \in \{0,1\}^* | \text{ Every 1 occurring in w is immediately followed by a  0} \}$
Study the complexity of the TM you O(n)defined. 

##### Solution
I work with a TM with only the input tape, and 2 halting states, $q_{acc}$ and $q_{neg}$ for accepting and rejecting a word respectively.
- Alphabet: $\Gamma = \{ \triangle , \square, 0, 1\}$
- Q = {$q_{neg}, q_{acc}, q_i, q_{o}$}
- The initial state of the machine is $q_i$.

Transition function:
$\delta (q_i, \triangle) = (q_i, R)$ - starting position, we go right
$\delta (q_i, 0) = (q_i, R)$ - if we find a 0, continue reading
$\delta (q_i, 1) = (q_o, R)$ - if we find a 1, oh no! DUEL TIME! WE NEED TO CHECK!!!
$\delta (q_o, 0) = (q_i, R)$ - Check passed, all good sir!! (there was a 0 after a 1)
$\delta (q_o, 1) = q_{neg}$ - CHECK NOT PASSED! IMMA NEED YOU TO STAY PUT, BUD!! HANDS BEHIND YOUR BACK, IM TAKING YOU IN!
$\delta (q_o, \square) = q_{neg}$ - CHECK NOT PASSED AGAIN! THERE MUST BE A 0, NO EXCUSES! (you die in a prison)
$\delta (q_i, \square) = q_{acc}$ - all good! you experience a life full of bliss! 

The complexity of the turing machine is $O(n)$, since we slide on the input in the worst case. 

#### es2
Prove that the following problem is in FP:
Given 2 lists, $L = L_1,...,L_n$ and $P = P_1, ..., P_n$ compute the scalar product $L_1\times P_1 + ... + L_n\times P_n$.
You can give a TM or define some pseudocode.

##### Solution
Here my fucking code:

```
sum = 0
for i in 1:n {
	sum += L[i]*P[i] 
}
```
The number of instruction is $O(n)$. 
sum memory consumption is $O(|L_1| + |P_1| + ... + |L_n| + |P_n|)$, since $|L_1 \cdot P_1| \le |L_1| + |P_1|$.
Every instruction is polytime computable. 

#### es3
We studied SAT and 3SAT. You are required to classify the problem 1SAT, containing the satisfiable 1CNF . To which complexity does 1SAT belong?
Prove your answer. 

##### Solution
A formula $L_1 \land L_2 \land L_3 .... \land  L_n$ is 1CNF, since each disjuction clause has at most 1 literal. 
Such a formula is satisfiable if there's no contradiction ($x_i \land ¬x_i$).

We give a polytime algorithm:
- Input: a list $L_1, ..., L_n$ of literals.
- Output: return 1 iff there is no contradiction in $L_1,..., L_n$.

```
X <- array of bools of size n

for i in n :
	if L[i] is pos : 
		if X[i] = false then return UNSAT
		else X[i] = true
	else if L[i] is neg : 
		if X[i] = true reutrn UNSAT
		else X[i] = false 

return SAT
```

- O(n) instructions. 
- X is size n
- all instructions are polytime compatible. 

Thus, 1SAT is in P. 