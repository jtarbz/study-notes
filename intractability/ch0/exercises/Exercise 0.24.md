**Exercise 0.24:** Let PRIMES be the following problem: given a number, is it prime? Let UFT be the theorem that every number can be factored uniquely into primes.

**1. Prove that $\text{PRIMES} \in \mathrm{NP}$.**

It is sufficient to describe the following:

1. A certificate with sufficient information to prove that some number $q$ is prime.
2. A short (polynomial time) algorithm to verify this certificate.

We provide the certificate using Lucas' theorem, which can be derived from Fermat's little theorem. To prove that a number $q$ is prime, we must show some number $a$ such that:

1. $a^{q-1} \equiv 1 \pmod q$.
2. For every prime factor $p$ of $q - 1$, $a^{(q-1)/p} \not\equiv 1 \pmod q$.

So the certificate for $q$ is the list $(a, p_0, p_1, \dots, p_k)$ where $q$ is the number we are proving to be prime, $a$ is the number described above, and $q - 1 = p_0 \cdot p_1 \cdots p_k$ where all such $p_i$ are prime (by UFT).

The algorithm to verify the certificate is as follows:

1. Check that $a^{q-1} \equiv 1 \pmod q$. Return FALSE if not.
2. For each prime factor $p_i$ of $q - 1$, check that $a^{(q-1)/p_i} \not\equiv 1 \pmod q$. Return FALSE if not.
3. Return TRUE.

Step (1) requires only $O(\log q)$ steps using modular exponentiation by successive squaring. Step (2) similarly requires only $O(\log q)$ steps for each $p_i$, and there are **at most** $\lceil \log q \rceil$ such primes (otherwise, even if each factor were 2, the resulting product would necessarily be greater than $q$). So the running time of the verifier is $O(\log^2 q)$ modular multiplications, which is quadratic in the length of the input. (Counting bit operations instead, each multiplication of $O(\log q)$-bit numbers costs $O(\log^2 q)$, so the verifier runs in $O(\log^4 q)$ time.) Consequently, $\text{PRIMES} \in \mathrm{NP}$. $\blacksquare$

(Note: to be thorough, we must also verify each prime factor of $q - 1$ is actually prime. This is a recursive application of the certificate and verifier. However, the resulting tree is also polynomial in the input length. This can be shown by induction.)

**2. Use UFT and $\text{PRIMES} \in \mathrm{NP}$ to prove that $\overline{\text{FACTORING}} \in \mathrm{NP}$.**

We again describe a certificate and polynomial time verifier for $\overline{\mathrm{FACTORING}}$:

$$ \overline{\mathrm{FACTORING}} = \lbrace (N, a) \mid \text{There is no nontrivial factor of } N \text{ that is} \le a \rbrace $$

The certificate is just the prime factorization of $N$, which must exist by UFT, plus a certificate showing that each factor is in PRIMES. Our verifier algorithm is as follows:

1. Verify each prime factor of $N$ is prime (by the previous solution).
2. Verify that no prime factor of $N$ is less than or equal to $a$.

Step (1) requires only polynomial time because the number of prime factors is linear in the input length and $\text{PRIMES} \in \mathrm{NP}$. Step (2) is just a single comparison for each factor. So $\overline{\mathrm{FACTORING}} \in \mathrm{NP}$. $\blacksquare$

**3. Use $\text{FACTORING} \in \mathrm{NP} \cap \mathrm{coNP}$ to show that, if FACTORING is NP-Complete, then $\mathrm{NP} = \mathrm{coNP}$.**

If FACTORING were NP-Complete then any problem in NP could be reduced to FACTORING. But $\text{FACTORING} \in \mathrm{NP} \cap \mathrm{coNP} \Rightarrow \text{FACTORING} \in \mathrm{coNP}$. Therefore we could reduce any problem in NP to a problem in coNP, meaning $\mathrm{NP} \subseteq \mathrm{coNP}$. But for any problem $A \in \mathrm{coNP}$, we know that $\overline{A} \in \mathrm{NP} \subseteq \mathrm{coNP}$. So it must be that $\mathrm{NP} = \mathrm{coNP}$. $\blacksquare$

> [!note]- Source page
> ![[attachments/exercise-0.24-1.jpg|500]] 
> ![[attachments/exercise-0.24-2.jpg|500]]