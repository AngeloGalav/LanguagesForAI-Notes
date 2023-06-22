## Representation Classes 
In our definition of efficient PAC learning, the algorithm $A$, having no access to the target concept $c ∈ C$, must work in _polynomial time_ __independently__ on $c$. 
- We assume _concepts_ in $C$ can be represented by way of _binary strings_, and each concept $e ∈ C$ requires $size(e)$ bits. We talk of a __representation class__. 
	- i.e., we could have a NN with 3 layers, or with billions of layers. They are 2 different representation classes of the same class of concepts.

- Examples 
	- $X_n$ could be $\{0, 1\}^n$, the set of _boolean vectors_ of of (fixed!) length $n$, and $C_n$ is the set of all subsets of $\{0, 1\}^n$ represented by CNFs. 
	- $X_n$ could rather be $R^n$, the _set of vectors of real numbers of length_ $n$, while $C_n$ are say, the subsets of $R^n$ represented by some form of neural network with $n$ inputs and 1 output. 
		- $C_n$ could be the class of concept having exactly size $n$, or exactly $n$ inputs and 1 outputs, or a set of NN having exactly $n$ layers.  

	- In many cases (e.g. SGD), one has a _single learning algorithm_ that work for _every value_ of $n$. In that case, we allow (in the definition of efficient PAC learning) the algorithm $A$ to take time polynomial in $n$, $size(c)$, $\dfrac{1} {ε}$ and $\dfrac{1 }{δ}$.
		- $size(c)$ is the size of the target concept. In this way, the learning algorithm is dependent on the size of the concept. 

In general, representation classes are just a way of stratifying a determined concept class.
	
### Boolean Functions as a Representation Class
- Suppose your _instance class_ is $X = \cup_{n∈N}X_n$ where $X_n = \{0, 1\}^n$ (i.e., the class $X$ is stratified with $X_n$ boolean vectors, each of them having bigger size).
- One first example of a _representation class_ for $X_n$ is the class $CL_n$ of all conjunctions of literals on the variables $x_1, . . . , x_n$. 
	- As an example, the conjunction $$x_1 ∧ ¬x_2 ∧ x_4$$, defines a subset of $\{0, 1\}^4$ . 
	- _Not all subsets_ of $\{0, 1\}^n$ can be captured this way. It is not universal wrt to the instance class. 
	
- A second example of a representation class for $X_n$ is a class we know, namely the class $CNF_n$ of _CNFs_ over $x_1, . . . , x_n$, which are conjunction of disjunctions of literals. 
	- CNFs are normal forms of any boolean functions. 
	- _All subsets_ of $\{0, 1\}^n$ can be _captured this way_. 
	- We could even consider $kCNF_n$ rather than arbitrary one, but this way we would lose universality.

### Learning Conjuctions of Literals 
Suppose your target concept is a conjunction of literals $c$ on $n$ variables $x_1, . . . , x_n$. How could a learning algorithm proceed? 
- Data are in the form $(s, b)$ where $s ∈ \{0, 1\}^n$ and $b ∈ \{0, 1\}$. The latter is a _label_ telling us whether $s ∈ c$ or $s \not ∈ c$. 

- A learning algorithm could proceed by keeping _a conjunction of literals_ $h$ as _its state_, initially set to $$x_1 ∧ ¬x_1 ∧ x_2 ∧ ¬x_2 ∧ · · · ∧ x_n ∧ ¬x_n$$
	and _updating it according to positive data_ (while negative data are discarded). 
	- If $n = 3$, the current state of h is $x_1 ∧ x_2 ∧ ¬x_2 ∧ ¬x_3$ and we receive $(101, 1)$, the hypothesis $h$ is updated as $x_1 ∧ ¬x_2$. 
	- 
>[!Theorem]
>The representation class of _boolean conjuctions_ of literals is __efficiently PAC-learnable__.

### Intractability of Learning DNFs 
We know that conjunctions of literals are efficiently learnable. But they are highly incomplete as a way to represent boolean functions. 

Let us take a look at a slight generalization of conjunctions of literals as a representation class. 
- A 3-term DNF formula over n bits is a propositional formula in the form $T_1 ∨ T_2 ∨ T_3$, where each $T_i$ is a conjunction of literals over $x_1, . . . , x_n$. I
- In a sense, this class is the dual to 3CNFs! 
- As such, it is more expressive than conjunctions of literals, but still not universal.

>[!Theorem]
> If $NP \ne RP$, then the representation class of 3-term DNF formulas is __not__ _efficiently PAC learnable_.

