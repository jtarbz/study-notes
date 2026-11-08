## Setup
- Assume $a \ge b$.
- $F(1) = 1,\ F(2) = 2,\ F(k) = F(k-1) + F(k-2)$ for $k \ge 3$.

## Theorem

If Euclid's algorithm requires $k$ steps on a pair, then the smaller number $b$ satisfies $b \ge F(k)$.

Take this as the predicate $P(k)$.

## Proof

Suppose $(a, b)$ takes $k$ steps. Then:

- $(b, r)$ takes $k - 1$ steps,
- $(r, r')$ takes $k - 2$ steps,

where $r = a \bmod b$ and $r' = b \bmod r$, so $b = qr + r'$.

**Case $k = 1$.** $F(1) = 1$, so we must show $b \ge 1$. This holds because $b$ is a positive integer, so the algorithm only takes a step when $b \ne 0$.

**Case $k = 2$.** $F(2) = 2$, so we must show $b \ge 2$. If $b = 1$, then $b \mid a$ and the algorithm would finish in one step, so $b \ne 1$. So it must be that $b \ge 2$.

**Case $k \ge 3$.** Assume $P(k-1)$ and $P(k-2)$.

- $P(k-1)$ on $(b, r)$ $\implies$ $r \ge F(k-1)$.
- $P(k-2)$ on $(r, r')$ $\implies$ $r' \ge F(k-2)$.

So:

$$r + r' \ge F(k-1) + F(k-2) = F(k)$$

Since $r < b$, we must have $q \ge 1$, so $b = qr + r' \ge r + r'$. Therefore $b \ge F(k)$. $\blacksquare$

## Consequence

$F(k)$ grows like $\varphi^k$ with $\varphi = (1+\sqrt{5})/2$, so $b \ge F(k)$ gives $k = O(\log b)$. 

>[!note]- Source page
![[attachments/lame-theorem.jpg|500]]