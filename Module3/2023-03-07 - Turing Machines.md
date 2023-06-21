
![[formal_model.png]]

In turing machines, computation happens _locally_. 
- __Input tape__: read-only tape. 
	- The number of input tapes must be fixed once you dealing with a particular task. You cannot have as many input tapes as the length of the input (it would void the principle of locality). 
- __Work tape__: r/w, used to store the intermediate results of the computation. 
- __Output tape__: r/w, used to store the output of the computation.  
- __Register__: contains the states of the machine at _current execution time_. It is something that changes at each instruction. The register could contain also the final state of the machine.  
	- The state can be seen as the program counter register. 

'>' indicates the start of the tape. 
Each of the tape is infinite on the right hand side, but during the computation _only a finite quantity_ of them is used.  

#### Machine description
- It consists of $k$ tapes, where a tape is an infinite one-directional line of cells, each of which can hold a symbol from a _finite_ alphabet $Γ$, the _alphabet_ of the machine. 
	- once you _fix the machine_, $k$ is _fixed_: $k$ cannot be in the function of the algorithm/input. 

- Each tape is equipped with _tape head_, which can read or write one symbol at a time from or to the tape. Each head can move _left_ or _right_. 

- Some of the tapes can be _designated as input tapes_, and are _read-only_.
- The last tape can be taken as the _output tape_, and in that case contains the result of the computation. 
	- This slightly deviates from what we have said in our informal account, and is needed to compute arbitrary functions on $\{0, 1\}^∗$. 
	- The output tape(s) can be absent, and in that case the result can be taken as 0 or 1 depending on the final state.

##### The Instructions
- The machine has a finite _set of states_, called $Q$, which determine the action to be taken at the next step. 
- At each step, the machine: 
	1. __Read__ the symbols under the $k$ tape heads. 
	2. For the $k − 1$ read-write tapes, __replace__ the symbol with a new one, or leave it unchanged. 
	3. __Change its state__ to a new one. 
	4. __Move__ each of the $k$ _tape heads_ to the _left_ or to the _right_ (or stay in place).

These instructions are of course very basic, and far from being close to the kind of instructions programming languages offer. 
The point here is to have a simple, but expressive model.

#### Mathematical definition
A Turing Machine (TM for short) working on $k$ tapes is described as a triple $(Γ, Q, δ)$ containing 
- A finite set $Γ$ of _tape symbols_ (alphabet), which we assume contains the blank symbol $\square$, the start symbol $>$, and the binary digits 0 and 1.
- A finite set $Q$ of _states_ which includes a designated _initial state_ $q_{init}$ and a designated _final state_ $q_{halt}$ (we can have multiple final states).
- A _transition function_
$$
\delta : Q \times \Gamma^k \rightarrow Q \times\Gamma^{k-1} \times \{L,S,R\}^k
$$
describing the instructions regulating the functioning of the machine at each step.
- When the first parameter is $q_{halt}$, then $δ$ cannot touch the tapes nor the heads:
$$
\delta(q_{halt}, (\sigma_1, ..., \sigma_k)) = (q_{halt}, (\sigma_2, ..., \sigma_k), (S,...,S))
$$
and the machine is _stuck_: it doesn't change its configuration. 

#### Machine configurations and Computations
Given a TM $M = (Γ, Q, δ)$ working on $k$ tapes: 
- A _configuration_ consists of 
	- The _current state_ $q$. 
	- The _contents_ of the $k$ tapes. 
	- The _positions_ of the $k$ tape heads. 
	One such configuration will be indicated as $C$. 

- The _initial configuration_ for the _input_ $x ∈ \{0, 1\}^∗$ is the configuration $I_x$ in which: 
	- The current state is $q_{init}$. 
	- The first tape contains $\triangleright x$, followed by _blank symbols_, while the other tapes contain $\triangleright$, followed by blank symbols. 
	- The tape heads are positioned on the _first symbol_ of the $k$ tapes. 

- A _final configuration_ for the output $y ∈ \{0, 1\}^∗$ is any configuration whose state is $q_{halt}$ and in which the content of the output tape is $\triangleright y$, followed by _blank symbols_.
	- If you're machine is not designed properly, the output tape of the final configuration has not always an output. 

##### Machine Computations
Thanks to configurations, we can give a mathematical representation of how the machine actually computes. 
- Given any configuration $C$, the transition function $δ$ determines in a natural way the next configuration $D$, and we write $C \xrightarrow{\delta} D$.
- We say that $M$ returns $y ∈ \{0, 1\}^∗$ on input $x ∈ \{0, 1\}^∗$ in $t$ steps if
$$
I_x \xrightarrow{\delta} C_1 \xrightarrow{\delta} C_2 \xrightarrow{\delta} ... \xrightarrow{\delta} C_t
$$
where $C_t$ is a final configuration for $y$. We write $M(x)$ for $y$ if this holds.

- Finally, we say that $M$ __computes__ a _function_ $f : \{0, 1\}^∗ → \{0, 1\}^∗$ iff $M(x) = f(x)$ for every $x ∈ \{0, 1\}^∗$. In this case, $f$ is said to be _computable_.
	- Beware: for the moment, _we do not put any constraint on the number of steps_ $M$ needs to compute $f(x)$ from $x$.
	- $t$ stands for _time_: since each steps is local (atomic), the value of $t$ can be a good estimation for the time a certain algorithm uses for computation. 

#### Efficiency and Runtime
A TM $M$ computes a function $f : \{0, 1\}^∗ → \{0, 1\}^∗$ in time $T : N → N$ iff $M$ returns $f(x)$ on input $x$ in a number of steps smaller or equal to $T(|x|)$ for every $x ∈ \{0, 1\}^∗$ . In this case, $f$ is said to be _computable in time_ $T$.

A language $L_f ⊆ \{0, 1\}^∗$ is _decidable_ in time $T$ iff $f$ is _computable_ in time $T$. 
- Examples: 
	- The set of palindrome words is decidable in time $T(n) = 3n$. 
	- Computing the parity of binary strings requires time $T(n) = n + 2$. 
	- Basic operations like addition and multiplication are computable in polynomial time.

##### Model correctness/robustness
- Question: why is our definition the right one? 
Actually, there are many details in our definion of a TM which are arbitrary: many alternative definitions are available in the literature. 

Examples: 
- Rather than an arbitrary tape alphabet $Γ$, restrict to $\{0, 1, \triangleright,\square \}$. 
- Just one tape, rather than many. 
- Tapes can be infinite in both directions. 

In all the cases above (and in many others), one can prove that the more restrictive notion of _machine simulates_ the more _general one with polynomial overhead_.

## Machines as Strings
One of the very nice consequences of keeping our definition of a Turing Machine very simple is that any machine $M = (Γ, Q, δ)$ is in fact _completely determined from the __graph__ of_ $δ$, seen as a subset of $$Q × Γ^k × Q × Γ^{k−1} × \{L, S, R\} ^k$$
Any _parsimonious encoding_ of $δ$ as a binary string in $\{0, 1\}^∗$ thus _constitutes an acceptable encoding_ $\lfloor M\rfloor$ of $M$, provided the following two conditions are satisfied:

1. _Every string_ in $\{0, 1\}^∗$ _represents_ a TM, i.e. for every $x ∈ \{0, 1\}^∗$ there is $M$ such that $x = \lfloor M\rfloor$. 
2. Every TM $M$ is represented by an _infinitely many strings_ (although exactly one is the “canonical” representation $\lfloor M\rfloor$ of $M$). 

The two conditions above are not essential in any other contexts (i.e. when the encoded data is not a program), but are technically crucial here.

## The Universal Turing Machine
>[!Theorem]
>There exists a TM $U$ such that for every $x, α ∈ {0, 1}^∗$, it holds that $U(x, α) = M_α(x)$, where $M_α$ denotes the TM represented by $α$.
>
>Moreover, if $M_α$ _halts_ on input $x$ within $T$ steps then $U(x, α)$ halts within $CT log(T)$ steps, where $C$ is _independent_ of $|x|$ and depending only on $M_α$.

It means that there exist a TM such that if you give it the code of another TM $\alpha$, it can compute the output of $M_\alpha$ given the input $x$. 