(Some of the excercises and questions (beside q1) where already seen on [[2023-05-08 - General excercises|here]]. I'll repeat some solution in order to better remember how to do the excercises). 

# Theory questions
##### q1 
Let $f,g$ be the functions defined as $f(n)=2^{10}n^2log^2n$ and $g(n)=10^{−3}n^3logn$. 
- [ ] $f∈Ω(g)$ (no, $f$ is not an upper bound for $g$)
- [ ] $f∈Θ(g)$ (no, they grow differently)
- [v] $f∈O(g)$ 

This is because $n$ grows faster than $logn$, so $f$ is upper bounded by $g$.
To sum up, remember this:
- $\Omega(\cdot)$ -> it includes...
- $O(\cdot)$ -> it's included in...

##### q4
Suppose a language L is both in NP and in EXP. Then:
- [ ] The classes EXP and NP are necessarily equal. 
- [ ] L cannot be in P 
- [v] The classes EXP and NP are maybe different. 
- [v] L can even be NP-complete

All of the answers to these questions can be summed up by looking at this image:
![[np_vs_p.png]]

# Excercises
#### es1
Construct a deterministic TM of the kind you prefer, which decides the following language:
$$L=\{w∈\{0,1\}^∗∣w \text{ contains 10 as a substring}\}.$$
Study the complexity of TM you have defined
##### Solution
$\delta(\triangle, q_0) = (q_0, R)$ 
$\delta(0, q_0) = (q_0, R)$ - searching for anything...
$\delta(1, q_0) = (q_1, R)$ - found 1! now there has to be a 0...
$\delta(1, q_1) = (q_1, R)$ - found 1 again, no worries, if we found a 0 we're still good...
$\delta(0, q_1) = q_{acc}$ - yay! it contains 10!...
$\delta(\square, q_0) = q_{rej}$ - WTF?? THIS STRING IS ALL ZEROS >:( 
$\delta(\square, q_1) = q_{rej}$ - found nothing.

#### es3
We studied the classes SAT, and 3SAT, and 2SAT. 
You are required to classify the problem 1SAT containing all (and only) the satisfiable 1 CNFs. In particular, to which complexity class does 1SAT belong? Prove your claim.
##### Solution
Well well well... you want me to prove this shit. Aight. 
First, know that 1CNF is basically this $x_1 \land x_2 \land x_3 \land ¬x_1 ...$. 
Aka, a conjuction of single truth vars. 

1SAT means that there has to be a set of values for which this expression is true, and is this case, such value can be found unless there's a contradiction ($x_1 \land ¬x_1$ for example). How peculiar. 

What we need to do is just find an algorithm that find if there's a contradiction in the preposition. This is fairly easy and you can see the full solution on the page linked above. 
