## Boolean Formulas and SAT

- Inductive definition of a Boolean formula:
    1. Any boolean var is a boolean formula
    2. $A, B$ are boolean formulas $\Rightarrow (A \wedge B), (A \vee B), \neg A$ are boolean formulas
- Definition: A **literal** is either a boolean var or its negation.
- Problem: SAT
    - Instance: A Boolean formula $\varphi(x_1, \dots, x_n)$
    - Question: Is there a satisfying assignment for $\varphi$? That is, does there exist $(b_1, \dots, b_n)$ such that $\varphi(b_1, \dots, b_n) = \text{TRUE}$?
    - Naively, we check all $2^n$ possible inputs to solve this problem.
    - Even if we have some method to check just $2^{n/10}$ inputs, it's just fancy brute force.

## Polynomial Time

- **Set Membership Problem:** Let $A$ be a set of strings. Is there an algorithm that, given an input string $x$, quickly determines whether $x \in A$?
    - To measure speed, we study the algorithm's running time as a function of $n = |x|$.
- Definition: A **decision problem** is a set membership problem. The answer is YES or NO.
    - A "problem" will typically mean a decision problem, or else, a function evaluation or relation.
    - We say **instance** to refer to the input $x$ for a problem.
- Definition: P is the set of decision problems that can be solved in time that is bounded above by a polynomial in $n = |x|$.
- Definition: FP is the set of functions that can be computed in time that is bounded above by a polynomial in $n = |x|$.
    - For determining **membership** in these sets, we sometimes cheat in the representation of an input's size, e.g.: a dense graph on $n$ vertices takes $\Theta(n^2)$ to represent, but we say the length of the input is $n$.
- To prove that a problem is in P, we provide a polynomially-bounded algorithm to solve it.
- To prove that a problem is **not** in P, we need a model of computation.
    - We also need this to make the definition of P rigorous.

> [!note]- Source page
> ![[attachments/ch0-notes-1.jpg|500]]

## "Turing Machines"

- These are a common formal model of computation.
- We do not **DESERVE** to know the definition of a Turing Machine.
- "Imagine a Java program..."
- Here's what must be known:
    1. Turing machines work in discrete steps; one can measure them.
    2. Let $A$ be a decision problem. The following are equivalent:
        1. There exists a program $M$ in Java and a polynomial $p$ such that, on input $x$, $M(x)$ will output $A(x)$ and terminate in $\le p(|x|)$ steps.
        2. There exists a Word-RAM algorithm $M$ and a polynomial $q$ such that, on input $x$, $M(x)$ will output $A(x)$ and terminate in $\le q(|x|)$ steps.
        3. There exists a Turing machine $M$ and polynomial $r$ such that ...
    3. We believe that anything that can be computed at all can be computed by a Turing machine (or Word-RAM). This assumption is the **Church-Turing thesis**.
    4. The **extended Church-Turing thesis** states (further) that anything that can be computed in polynomial time can be computed in polynomial time by a Turing machine. But this may be inconsistent with newer models of computation like quantum computing (and we get weird classes like BQP).

## Reductions

- Definition: Let $A$ and $B$ be decision problems. A reduction from $A$ to $B$ is a polynomial-time computable function from instances of $A$ to instances of $B$ such that $\forall x,\ x \in A$ iff $f(x) \in B$.
    - If such a reduction exists, we may write $A \le_p B$.
- Note the following (proofs as an exercise):
    
    1. If $A \le_p B$ and $B \le_p C$, then $A \le_p C$.
    2. If $B \in \mathrm{P}$ and $A \le_p B$, then $A \in \mathrm{P}$.
    3. If $A \notin \mathrm{P}$ and $A \le_p B$, then $B \notin \mathrm{P}$.
    
    - Item (3) is particularly useful to prove some problem $B$ is not in P.
- The reduction defined above is a **Karp reduction** (you can ask one question of $B$).
- Polynomial-time reductions where you may ask many questions are called **Cook reductions**.

> [!note]- Source page
> ![[attachments/ch0-notes-2.jpg|500]]

## NP: Nondeterministic Polynomial Time

- Notation: Let $\exists^p y$ denote "there exists $y$ of length $q(|x|)$," where $q$ is a polynomial that is understood.
- Definition: $A \in \mathrm{NP}$ if there exists a set $B \in \mathrm{P}$ such that $A = \{x \mid \exists^p y : (x, y) \in B\}$.
- Definition: $A \in \mathrm{NP}$ iff $A$ is accepted by a nondeterministic Turing machine that runs in polynomial time.
- We might consider the following intuition:
    - If $x \in A$, then there is a **short** witness $y$ that can be used to verify $x \in A$ quickly.
    - If $x \notin A$, then there is no such witness.

### Some NP Problems (+ Their Witnesses)

- 3-Coloring: Given a graph, is it 3-colorable? The 3-coloring is the witness.
- Clique: Given a graph $G$ and a number $k$, does $G$ have a clique of size $k$? The clique itself is the witness.
- Shortest path
- Subset sum
- Integer programming

## NP-Completeness

- Definition: $B$ is **NP-hard** if, for all $A \in \mathrm{NP}$, $A \le_p B$.
- Definition: $B$ is **NP-Complete** if $B \in \mathrm{NP}$ and $B$ is NP-hard.
- Shockingly, there are many natural NP-Complete problems.
- Theorem: SAT is NP-Complete. (Cook-Levin theorem)
- Notation: Let $\Sigma$ be an alphabet. Let $A, B \subseteq \Sigma^*$.
    1. $A \cdot B = {xy \mid x \in A \wedge y \in B}$
    2. $A^0 = {\varepsilon}$ where $\varepsilon$ is the empty string.
    3. $A^i = A \cdot A \cdot \ldots \cdot A$ ($i$ times)
    4. $A^* = A^0 \cup A^1 \cup A^2 \cup \ldots$
    5. $\overline{A} = \Sigma^* - A$

> [!note]- Source page
> ![[attachments/ch0-notes-3.jpg|500]]

## Co-NP

- The complement of SAT is the set of all formulas with no satisfying assignment:
    
    $$\text{CONTRA} = \{\varphi \mid \forall \vec{b} : \varphi(\vec{b}) = \text{FALSE}\}$$
    
- Clearly $\text{SAT} \in \mathrm{P} \iff \text{CONTRA} \in \mathrm{P}$.
    
- CONTRA is probably not in NP, as verifying $\varphi \in \text{CONTRA}$ requires the truth table for $\varphi$.
    
    - So we define the class coNP, where CONTRA naturally lies.
- Definition: $A \in \text{coNP}$ if there exists a set $B \in \mathrm{P}$ such that $A = {x \mid \forall^p y : (x, y) \in B}$.
    
    - Note there is an equivalent definition of NP not used in the text.
- Intuition:
    
    1. $x \notin A \Rightarrow$ there is a short witness ($y$) to verify $x \notin A$ quickly.
    2. $x \in A \Rightarrow$ there is no such witness.
- Another definition: $A \in \text{coNP}$ if $\overline{A} \in \mathrm{NP}$.
    
- Note CONTRA is coNP-complete.
    

## Strong NP-Completeness

- Example: Subset sum is NP-Complete when computed over $n + 1$ numbers in binary, but is in P when the input numbers are specified in unary.
- Definition: A problem is **Weakly NP-hard** if it is NP-hard when the inputs are in binary.
- Definition: A problem is **Strongly NP-hard** if it is NP-hard when the inputs are in unary.

> [!note]- Source page
> ![[attachments/ch0-notes-4.jpg|500]]

## Intermediate Problems

- Definition: A problem is **NP-intermediate** if it is in NP but neither in P nor NP-Complete.
- **Ladner's theorem:** If $\mathrm{P} \ne \mathrm{NP}$ then there exists an NP-intermediate problem.
    - The problem used in the proof is rather contrived.
- We conjecture a few natural problems to be NP-intermediate:
    1. FACTORING. The decision problem version is just deciding if the input $N$ has a nontrivial factor $\le a$ for some $a$. We do not have a polynomial-time algorithm for FACTORING and there is no proof that it is NP-Complete.
        - The last improvement in factoring algorithms was the number field sieve in '88.
        - FACTORING is unlikely to be NP-Complete because it would imply $\mathrm{NP} = \mathrm{coNP}$.
    2. DISCRETE LOG. Similar exposition to FACTORING.
    3. GRAPH ISOMORPHISM. The fastest algorithm has running time $2^{(\log n)^c}$ for some constant $c$. It is believed that a quasipolynomial running time $2^{(\log n)^c}$ with $c > 1$ is the best one can do with current methods.
        - If GRAPH ISOMORPHISM is NP-Complete then all problems in NP are solvable in quasipolynomial time $2^{(\log n)^c}$. This would violate ETH, but ETH is false (RIP), so that part isn't a big deal.
        - If GRAPH ISOMORPHISM is NP-Complete then $\Sigma_2 = \Pi_2$ (Boppana et al.).
    4. Minimum circuit size problem (MCSP). Given the truth table of a boolean function $f$ and an integer $s$, does $f$ have a circuit with at most $s$ logic gates?
        - If $\text{MCSP} \in \mathrm{P}$ then there is no secure crypto system (Kabanets and Cai).

## Exponential Time Hypothesis

- Briefly, this hypothesis states that SAT requires exponential time. There is a more thorough formulation.
    - Notably, this was shown to be false **very recently**.
- The strong exponential time hypothesis states that $k$SAT gets harder as $k$ grows.
- Need to examine results following from ETH/SETH in light of new research.

> [!note]- Source page
> ![[attachments/ch0-notes-5.jpg|500]]

## Fixed-Parameter Tractability

- Consider the following NP-Complete problems:
    1. $\text{CLIQUE} = \{(G, k) \mid G \text{ has a clique of size } k\}$
    2. $\text{VERTEX COVER} = \{(G, k) \mid G \text{ has a vertex cover of size } k\}$
- We might fix $k$ to describe the following (related) problems:
    1. $\text{CLIQUE}_k = \{G \mid G \text{ has a clique of size } k\}$
    2. $\text{VERTEX COVER}_k = \{G \mid G \text{ has a vertex cover of size } k\}$
- We can solve the fixed-$k$ versions in time $O(n^k)$, so they are in P.
    - But the vanilla versions are **not** in P because the exponent $k$ is a function of the input size.
- $\text{VERTEX COVER}_k$ can be solved in time $O(2^k n)$, but $\text{CLIQUE}_k$ is believed to require time $n^{\Omega(k)}$.

## Complexity Classes That Use Randomization

- Definition: A decision problem $A$ is in **randomized polynomial time (RP)** if there is a polynomial-time algorithm $M$ that can flip coins satisfying the following properties:
    1. If $x \in A$, then the probability that $M(x)$ says $x \in A$ is $\ge 1/2$.
    2. If $x \notin A$, then the probability that $M(x)$ says $x \notin A$ is $1$.
- Some facts about RP:
    1. G. Miller obtained a polynomial-time algorithm for PRIMES that depends on the extended Riemann hypothesis being true. Rabin modified the algorithm to be in RP without any hypothesis. The final algorithm is the **Miller-Rabin test**.
    2. There are very few problems in RP that are not known to be in P. One such problem: Given a polynomial $f(x_1, \dots, x_n)$ and a prime $p$, is the polynomial identically zero over $\mathbb{Z}_p$?
    3. There is reason to believe $\mathrm{P} = \mathrm{RP}$.
- Definition: A set $A$ is in **bounded probabilistic polynomial time (BPP)** if some polytime $M$ can flip coins such that:
    1. If $x \in A$, then the probability that $M(x)$ says $x \in A$ is $\ge 3/4$.
    2. If $x \notin A$, then the probability that $M(x)$ says $x \notin A$ is $\ge 3/4$.
- Some facts about BPP:
    1. There are no (natural or unnatural) problems known to be in BPP but not known to be in RP.
    2. There is reason to believe $\mathrm{P} = \mathrm{BPP}$ (based on the reasonable existence of a pseudorandom generator).
    3. It is not known whether $\mathrm{BPP} \subseteq \mathrm{NP}$.
    4. $\mathrm{BPP} \subseteq \Sigma_2 \cap \Pi_2$. Sipser's proof of this started the field of time-bounded Kolmogorov complexity.

> [!note]- Source page
> ![[attachments/ch0-notes-6.jpg|500]]