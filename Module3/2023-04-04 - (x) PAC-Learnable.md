We have so far taken functions and languages as our notions of _computational tasks_:
![[pac1.png]]
- Is it that learning problems, among the most crucial tasks in AI, can be seen as computational problems? 
- The answer is positive: any learning algorithm $A$ actually computes a function $f_A$ whose input is a _finite sequence of labelled data_ and whose output can be seen as a _classifier_:
![[data_colas.png]]
- Could data and classifiers be encoded as strings, thus turning $f_A$ as a function of the kind we know? 
- How could we formalize the fact that $A$ correctly solves a given learning task?

![[trainin_ai_clas.png]]
- Training data are _labelled_, so as to let $f_A$ learn what the _relationship exists_ between _data_ and _labels_. 
- $C$ takes as input data which is _not labelled_, and $C$’s task is precisely the one of _finding the appropriate label_ for any of them. 
- Most often, $C$ is drawn from a rather _restricted set of classifiers_, i.e. not all algorithms can be obtained in output from $f_A$.

## An Example Problem
![[example_learning.png]]
Suppose that the data the algorithm $A$ takes in input are points $(x, y) ∈ R_{[0,1]} × R_{[0,1]}$ (where $R_{[0,1]}$ is the set of real numbers between 0 and 1). These are labelled as positive or negative depending on they being inside a rectangle.
- The algorithm $A$ should be able to guess a _classifier_, namely a _rectangle_, based on the labelled data it received in input. 
- It knows that the data are _labelled according to a rectangle_, $R$ but it does not know _which rectangle is being used_. 
- The algorithm $A$ cannot guess the rectangle $R$ with perfect accuracy if the data it receives in input are too few. As the data in $D$ grow in number, we would expect the rectangle $f_A(D)$ to converge to $R$, wouldn’t we?

## The Rules of the Game
Again, $A$ _knows_ that the the way input data are labelled is _by way of a rectangle_ (whose sides are parallel to the axes). 
- But it does not know which one! 

$A$ _does not know the distribution_ $D$ from which the points $(x, y)$ are drawn. 
- It is supposed to “do the job” for each possible distribution $D$. 

$A$ is an ordinary algorithm. 
- Ultimately, it can be seen as a TM. 
- We thus assume that real numbers can be appropriately approximated as binary strings. 
- In some cases, it is useful to assume $A$ to have the possibility to “flip a coin”, i.e., to _be a randomized algorithm_.

### The Algorithm $A_{BFP}$
We could define an Algorithm $A_{BFP}$ as follows: 
1. Given the data $((x_1, y_1), p_1), . . . ,((x_n, y_n), p_n)$; 
2. Determine the _smallest rectangle_ $R$ including all the positive instances; 
3. Return $R$. 
 
In the previous problem, one would get, when running $A_{BFP}$, the little bold rectangle. Of course, the result is always a _sub-rectangle_ of the _target rectangle_. 
The output classifier is a rectangle, which can be easily represented as a _pair of coordinates_.

![[abfp.png]]

Keep in mind, $A_{BFP}$ only works with rectangles!

### Is $A_{BFP}$ (Approximately) Correct?
The output of $A_{BFP}$ can be very different from the target rectangle. 
- Is the algorithm blatantly incorrect, then? 
	- The answer is negative.

For a given rectangle $R$ and a target rectangle $T$, the probability of error in using $R$ as a replacement of $T$ (when the distribution is $D$) is 
$$error_{D,T}(R) = \text{Pr}_{x∼D}[x ∈ (R − T) ∪ (T − R)]$$
(The error is given by probability of $x$ of being in $R$ but not in $T$ and viceversa. It coincides with the probability that $x$ is uncorrectly classified)

>[!WARNING]
>Note: T is _not_ what the algorithm uses in order to find the best approximation (i.e. it's not the loss function). It is something that we know apriori, and that it is used to compute the error.  

As the _number of samples in $D$ grows_, the result $A_{BFP}(D)$ _does not necessarily approach the target rectangle_, but its probability of error approaches zero.

>[!Theorem]
>For every distribution $D$, for every $0 < ε < \dfrac{1}{2}$ and for every $0 < δ < \dfrac{1}{2}$ , if $m ≥ \dfrac{4}{ε}ln(\dfrac{4}{δ})$ , then
>![[abfp_theorem.png]]

Consider the theorem: in the theorem, the value of $m$ goes up with $\epsilon$ and $\delta$ approaching zero. 
- $\epsilon$ and $\delta$ are 2 kinds of errors.
- $m$ is the number of instances of the labelled data that we need. 
The theorem tells us that the _probability_ that the _error_ produced by the algorithm is _smaller than_ $\epsilon$ (when passing some training data of dimension $m$) is _greater_ than  $1 - \delta$. 

(btw, since this theorem is related to $A_{BFP}$, it is only valid when using rectangles)

## The General Model (outside of rectangles)
### Terminology
We assume to work within an _instance space_ $X$. 
- $X$ is the set of (_encodings_) of instances of objects the learner wants to classify. 
- Data from the instance spaces are generated through a _distribution_ $D$, _unknown to the learner_. In the example, $X = R^2_{[0,1]}$. 
- __Concepts__ are subsets of $X$, i.e. collections of objects. These should be thought of as _properties of objects_ (i.e. a model, for example SVM, regression function with a certain space, etc.). 
	- In the example, concepts are arbitrary subsets of $X = R^2_{[0,1]}$, i.e. _arbitrary regions within_ $R^2_{[0,1]}$. 

- A __concept class__ $C$ is a collection of concepts, namely a subset of $P(X)$. These are the concepts which are sufficiently simple to describe, and that algorithms can handle.
	- i.e., in the example of the rectangle, it is the class of _all_ rectangles. 
	- The concept class $C$ we work with in the example is the one of _rectangles whose sides are parallel to the axes_. 
	- The __target concept__ $c ∈ C$ is the concept the learner wants to build a classifier for.

### The Learning Algorithm A
Every learning algorithm is designed to _learn concepts_ from a _concept class_ $C$ but it ==does not know the target concept $c ∈ C$, nor the associated distribution $D$==. 
- i.e., $A_{BFP}$ was designed only for rectangles, and it's interested in finding only rectangles in the instance space.

The interface of any learning algorithm $A$ can be described as follows: 
![[2pax.png]]
where: 
- $ε$ is _error parameter_, while $δ$ is the _confidence parameter_; 
- $EX (c, D)$ should be though as an __oracle__, a procedure that $A$ can call as many times she wants, and which returns an element $x ∼ D$ from $X$, labelled according to whether it is in $c$ or not. 
	- The oracle gets called every time the algorithm needs a new instance of a labelled training data. 
	
The error of any element of the concept class $h ∈ C$ is defined as $error_{D,c} = \text{Pr}_{x∼D}[h(x) \ne c(x)]$.
- i.e., $h$ can be the output classifier produced by $A$.

### Two Kinds of Errors
![[2kinds.png]]
### PAC Concept Classes
Let $C$ be a _concept class_ over the _instance space_ $X$. 
We say that $C$ is __PAC learnable__ iff there is an algorithm $A$ such that for every $c ∈ C$, _for every distribution_ $D$, for every $0 < ε < \dfrac{1}{2}$ and for every $0 < δ < \dfrac{1}{2}$ , then 
$$\text{Pr}[error_{D,c}(A(EX (c, D), ε, δ)) < ε] > 1 − δ$$
where the probability is taken over the calls to $EX (c, D)$.

- The algorithm can decide how many times to call the oracle $EX (c, D)$, depending on $\epsilon$ and depending on $\delta$. 

If the _time complexity_ of $A$ is _bounded by a polynomial_ in $\dfrac{1}{ε}$ and $\dfrac{1}{δ}$ , we say that $C$ is __efficiently PAC learnable__. 
- The complexity of $A$ is _measured taking into account the number of calls_ to $EX (c, D)$.

##### PAC-learnable rectangles
>[!COROLLARY]
>The concept-class of _axis-aligned rectangles_ over $R^2_{[0,1]}$ is efficiently __PAC-learnable__.

Why is the corollary true? It's because $m$, the quantity of data on which the algorithm is based on, is bounded by a polynomial in $\dfrac{1}{ε}$ and $\dfrac{1}{δ}$ as we've seen. 

What basically the PAC-learnable concept tells you is that there's a relationship between accuracy and traning data size. 
