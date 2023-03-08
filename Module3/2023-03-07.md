![[formal_model.png]]

In turing machines, computation happens _locally_. 
- __Input tape__: read-only tape. 
	- The number of input tapes must be fixed once you dealing with a particular task. You cannot have as many input tapes as the length of the input (it would void the principle of locality). 
- __Work tape__: r/w, used to store the intermediate results of the computation. 
- __Output tape__: r/w, used to store the output of the computation.  
- __Register__: contains the states of the machine at current exectution time. It is something that changes at each instruction. The register could contain also the final state of the machine.  
	- The state can be seen as the program counter register. 

'>' indicates the start of the tape. 
Each of the tape is infinite on the right hand side, but during the computation _only a finite quantity_ of them is used.  

#### Machine description
- It consists of $k$ tapes, where a tape is an infinite one-directional line of cells, each of which can hold a simbol from a _finite_ alphabel $Γ$, the _alphabet_ of the machine. 
	- once you fix the machine, $k$ is fixed: $k$ cannot be in the function of the algorithm/input. 
	- ??

- Each tape is equipped with tape head, which can read or write one symbol at a time from or to the tape. Each head can move left or right. 

- Some of the tapes can be designated as input tapes, and are read-only.
- The last tape can be taken as the output tape, and in that case contains the result of the computation. 
	- This slightly deviates from what we have said in our informal account, and is needed to compute arbitrary functions on $\{0, 1\}^∗$. 
	- The output tape(s) can be absent, and in that case the result can be taken as 0 or 1 depending on the final state.

- The machine has a finite _set of states_, called $Q$, which determine the action to be taken at the next step. 
- At each step, the machine: 
	1. Read the symbols under the k tape heads. 
	2. For the $k − 1$ read-write tapes, replace the symbol with a new one, or leave it unchanged. 
	3. Change its state to a new one. 
	4. Move each of the $k$ _tape heads_ to the _left_ or to the _right_ (or stay in place).

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

- The _initial configuration_ for the input $x ∈ \{0, 1\}^∗$ is the configuration $I_x$ in which: 
	- The current state is $q_{init}$. 
	- The first tape contains $\triangleright x$, followed by blank symbols, while the other tapes contain $\triangleright$, followed by blank symbols. 
	- The tape heads are positioned on the first symbol of the $k$ tapes. 

- A _final configuration_ for the output $y ∈ \{0, 1\}^∗$ is any configuration whose state is $q_{halt}$ and in which the content of the output tape is $\triangleright y$, followed by _blank symbols_.
	- If you're machine is not designed properly, the output tape of the final configuration has not always 

Thanks to configurations, we can give a mathematical representation of how the machine actually computes. 

- Given any configuration $C$, the transition function $δ$ determines in a natural way the next configuration $D$, and we write $C \xrightarrow{\delta} D$.
- We say that $M$ returns $y ∈ \{0, 1\}^∗$ on input $x ∈ \{0, 1\}^∗$ in t steps if
$$
I_x \xrightarrow{\delta} C_1 \xrightarrow{\delta} C_2 \xrightarrow{\delta} ... \xrightarrow{\delta} C_t
$$
where $C_t$ is a final configuration for $y$. We write $M(x)$ for $y$ if this holds.

- Finally, we say that $M$ __computes__ a _function_ $f : \{0, 1\}^∗ → \{0, 1\}^∗$ iff $M(x) = f(x)$ for every $x ∈ \{0, 1\}^∗$. In this case, $f$ is said to be _computable_.
	- Beware: for the moment, _we do not put any constraint on the number of steps_ $M$ needs to compute $f(x)$ from $x$.
	- $t$ stands for _time_: since each steps is local (atomic), the value of t can be a good estimation for the time a certain algorithm uses for computation. 


#### Efficiency and Runtime
A TM M computes a function $f : \{0, 1\}^∗ → \{0, 1\}^∗$ in time $T : N → N$ iff $M$ returns $f(x)$ on input $x$ in a number of steps smaller or equal to $T(|x|)$ for every $x ∈ \{0, 1\}^∗$ . In this case, $f$ is said to be _computable in time_ $T$.

A language $L_f ⊆ \{0, 1\}^∗$ is decidable in time $T$ iff $f$ is computable in time $T$. 
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

In all the cases above (and in many others), one can prove that _the more restrictive notion of machine simulates the more general one with polynomial overhead_. 
- We do not have time to see all that, but you are encouraged to take a look at [AroraBarak2009], Section 1.3.1.