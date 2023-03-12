# Model preliminaries

## Tasks vs Processes

==Don't confuse process with tasks!== 
A task is the objective, the problem I actually want to solve, while the process is the tool (i.e. whatever we do) we use to solve the task. 
![[task_vs_processes.png]]
- In some cases, there could be many distinct processes solving the same tasks (i can always do a stupid variation of a program using no-ops etc.). 
	- Usually, a single process can only solve a single task.  
- In some other cases, the task is so hard that no process can solve it.

### Multiplication Problem example
Let's see a simple example:
- __Task__: multiplying two natural numbers a and b both expressible with n digits, by performing operations on digits
- One process solving the task above consists in reducing multiplication to addition:
![[a_times.png]]

Since addition of two n-digit numbers is a relatively easy process can easily be carried out in a linear amount of steps, the total number of steps is proportional to $b · n$. Call this process `RepeatedAddition`.

Another process solving the task above consists in rather multipling $a$ and $b$ with the elementary school direct algorithm, which takes an amount of steps which is proportional to $n · n$. Call this process `GridMethod`. (Multiplying each digit of $a$ by one of $b$ etc, like in elementary school). 

There is an exponential difference between the performances of `RepeatedAddition` and `GridMethod`. 
- Indeed, $b$ can be exponential in $n$, i.e. at most $10^n − 1$. 
- When, e.g., $n$ is 100, there is a huge different between $100 · 100$ and $(100^{100} − 1)$ · 100. 
- If each basic instruction takes a millisecond, `GridMethod` would take one second, while `RepeatedAddition` would take more than 1080 years. 
- Distinct processes can solve the same task in very different ways, and not all of them are acceptable. 
- `GridMethod` witnesses the fact that the task is in a _class of tasks called_ $P$.

### The Wedding Problem example
- __Task__: given a set $L$ of candidate invitees, and a list $I ⊆ L × L$ of incompatible pairs of candidate invitees, find the largest subset of $L$ composed of compatible invitees. 
- A process that is for obvious reasons a correct way to accomplish the task above consists in considering all possible subsets of $L$, and check for the presence of incompatibilities in each of them, at the same tracking their size.

- If $L$ has $n$ elements, how many subset does $L$ have? 
	- This turns out to be $2^n$, which is impractical. 
- The _problem is thus known to be in the class_ $NP$. 
- The real question is can we do better? 
	- If we manage to do significantly better, we would have solved the question of whether $NP$ is equal to $P$.

## What is a Process?
In the Theory of Computation, ==a _process_ is taken to be an algorithm==. 
Giving a precise definition of an algorithm is not possible at this point, but any algorithm must satisfy the following constratints: 
- It must be a _finite description_ of a series of _computation steps_. 
	- i.e. a real number is not an algorithm since it's infinite. 
- Each step must be _elementary_.
	- Each step must be atomic in nature. i.e. you can't cheat by making a commnad that solves half of a complex problem. 
- The way the next step is determined must be deterministic.

Any program (say, written in Python) can be seen as a description of an algorithm!
It is a very high-level description, and the resources necessary to execute each instruction are hard to estimate. 
In this course, we will give a different, more abstract and low level, definition.

## Proving the Nonexistence of Algorithms
- Proving tasks not solvable by processes beyond a certain level of efficiency be necessary for a proper classification of tasks. 
- But how could we proceed? There are infinitely many processes solving a certain task, so we cannot exhaustively examine them. 
- We are very rarely able to prove the nonexistence of efficient algorithm. 
	- Finding mathematical techniques which allow us to do so is the crux of computational complexity theory. 
- Rather than proving the non-existence of certain algorithms, complexity theory interrelates different tasks

## Some Research Questions in Complexity Theory 
- The P vs. NP question. 
- Can the use of randomness help in speeding up computation? 
- Can hard tasks become easier if we allow algorithms to err on a relatively small subset of the inputs? 
- Can we exploit the hardness of certain tasks? 
- Can we make use of some counterintuitive properties of quantum mechanics to build faster computing devices?

# Mathematical Preliminaries
## Sets and Numbers 
- The cardinality of any set $X$ is indicated as $|X|$. I It can be finite or infinite. 
- We will mainly work with discrete numbers. 
	- $N$ is the set of natural numbers, while $Z$ is the set of integers. 
- We say that a condition $P(n)$ depending on $n ∈ N$ holds for sufficiently large $n$ if there is $N ∈ N$ such that $P(n)$ holds for every $n > N$. 
- Some common (and useful notation): 
	- For a given real number $x$, $\lceil x \rceil$ is the the smallest element of $Z$ such that $\lceil x \rceil ≥ x$. Whenever a real number $x$ is use in place of anatural number, we implicitly read it as $\lceil x \rceil$ 
	- For a natural number $n$, $[n]$ is the set $\{1, . . . , n\}$. 
	- Contrarily to the common mathematical notation, $log x$ is _base 2_ logarithm.

## Strings
If $S$ is a finite set, then a _string_ over the alphabet $S$ is a finite, ordered, possibly empty, tuple of elements from $S$. 
- Most often, the _alphabet_ $S$ will be the set $\{0, 1\}$.
- The set of all strings over $S$ of length exactly $n ∈ N$ is indicated as $S^n$ (where $S^0$ is the set containing only the empty string $ε$).
- The concatenation of two strings $x$ and $y$ over $S$ is indicated as $xy$. The string over $S$ obtained by concatenating $x$ with itself $k ∈ N$ times is indicated as $x^k$ .
	- Strings form a _monoid_!

The length of a string $x$ is indicated as $|x|$.

## Tasks as functions
One of the principles of computational complexity is that any task of interest consists in computing a function from $\{0, 1\}^∗$ to itself (A _binary_ string). 
- Is it too strong an assumption? 

The notion of function is sufficiently general, indeed. 
- Most tasks (but not all!) consists in turning an input to an output. 
	- Not all, i.e. learning process algorithm which gives a classifier as an output. 

- How about the emphasis on strings? 
	- If the input and/or the output are not strings, but they are taken from a discrete set, they can be _represented_ as strings, following some encoding, which however should be as simple as possible. 
- Summing up, we always assume that _the task we want to solve is given_ as a function $f : A → B$ where both the _domain_ $A$ and $B$ _are discrete_ (i.e. countable) sets, sometime leaving the encoding of $A$ and $B$ into ${0, 1}^∗$ implicit. 
- The encoding of any element $x$ of $A$ as a string is often indicated as xxy or simply as $x$.

#### Examples of strings enconding
Strings in $S^∗$ , where $S = \{a, b, c\}$. 
- $a$ can be encoded as 00, $b$ becomes 01 and $c$ becomes 10. 
- this way, e.g., $abbc$ becomes 00010110. 

Natural numbers. 
- We can take of the many encodings of natural numbers as strings. 
- As an example, 12 becomes 1100. 
 
Pairs of binary strings, i.e. $\{0, 1\}^∗ × \{0, 1\}^∗$ . 
One can encode the pair $(x, y)$ as the string $x\#y$ over the alphabet $\{0, 1, \#\}$, and the proceed as before.
- We can object that this is not a binary string. 

## Decision problems and languages
An important class of functions from $\{0, 1\}^∗$ to $\{0, 1\}^∗$ are those whose _range are strings of length exactly one_, called __boolean functions__. 
- They are the so-called __characteristic functions__. 
- We identify such a function $f$ with the subset $L_f$ of $\{0, 1\}^∗$ defined as follows: $L_f = \{x ∈ \{0, 1\}^∗ \ | \  f(x) = 1\}$. 
- Any subset of $S^∗$ (the set of all strings over an alphabet $S$) is usually called a _language_. 
- This way, a _decision problem_ for a _given language_ $M$ (i.e. is $x ∈ \{0, 1\}^∗$ in $M$?) can be seen as the task of computing $f$ such that $M = L_f$. 
	- Optimization problems are not decision problems, since we want to minimize or maximize a certain result. 

## Asymptotic Notation
- A function $f : N → N$ is $O(g)$ if there is a positive real constants $c$ such that $f(n) ≤ c · g(n)$ for sufficiently large $n$.

	- Example: the function $n → 3 · n^2 + 4 · n$ is $O(n^2)$, but also $O(n^3)$, and certainly $O(2^n)$. It is not, however, $O(n)$. 

- A function $f : N → N$ is $Ω(g)$ if there if a positive real constants $c$ such that $f(n) ≥ c · g(n)$ for sufficiently large $n$ 
	
	 - Example: the function $n → 3 · n^2 + 4 · n$ is $Ω(n^2)$, but also $Ω(n)$, but not $Ω(n^3 )$.

 - A function $f$ is $Θ(g)$ if $f$ is both $O(g)$ and $Ω(g)$. 
	 
  - Example: the function $n → 3 · n^2 + 4 · n$ is $Θ(n^2)$. 
  
  Studying in which relation two functions $f$ and $g$ are can be done by studying the limit of $\dfrac{f(n)}{g(n)}$ for $n$ tending to infinity.