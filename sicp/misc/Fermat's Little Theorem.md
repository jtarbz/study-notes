**Theorem:** If $n$ is a prime number and $a$ is any positive integer less than $n$, then $a$ raised to the $n$th power is congruent to $a$ modulo $n$.

In other words: $n$ is prime and $a < n$

$$ \Rightarrow a^n \equiv a \pmod n $$

$$ \Rightarrow a^{n-1} \equiv 1 \pmod n. $$

**Proof:** Let $S = \lbrace 1, 2, \dots, n-1 \rbrace$. Let $S' = \lbrace a, 2a, \dots, (n-1)a \rbrace$.

These two sets must be equivalent modulo $n$ because no two elements in $S'$ are congruent modulo $n$, and none is congruent to $0$, as $n$ is prime, and both sets necessarily have $n-1$ elements.

Consequently, $\prod S \equiv \prod S' \pmod n$.

$$ a \cdot 2a \cdot \ldots \cdot (n-1)a \equiv 1 \cdot 2 \cdot \ldots \cdot (n-1) \pmod n $$

$$ \Rightarrow a^{n-1}(n-1)! \equiv (n-1)! \pmod n $$

$$ \Rightarrow a^{n-1} \equiv 1 \pmod n \quad \text{(canceling } (n-1)!\text{, which is not divisible by } n \text{ since } n \text{ is prime)}. \qquad \blacksquare $$

> [!note]- Source page
> ![[attachments/fermats-little-theorem.jpg|500]]