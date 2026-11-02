**Exercise 0.20:** Show $\text{SUBSET SUM} \in \mathrm{P}$ when the inputs are in unary (use dynamic programming).

We require an algorithm to compute SUBSET SUM over unary inputs in polynomial time. The problem is as follows: Given integers $(a_0, \dots, a_{n-1}, B)$, does some subset of $a_0, \dots, a_{n-1}$ sum to $B$?

Let $\mathrm{DP}[n][B+1]$ be an $n \times (B+1)$ table of boolean values.

Set $\mathrm{DP}[0][j]$ to TRUE if $a_0 = j$, for all $j$; else set it to FALSE.

Set $\mathrm{DP}[i][0]$ to TRUE for all $i$.

Now we consider the following recurrence:

$$\mathrm{DP}[i][j] = \mathrm{DP}[i-1][j] \lor \mathrm{DP}[i-1][j-a_i],$$

where the second term is taken to be FALSE when $j < a_i$.

Fill all $\mathrm{DP}[i][j]$, top-to-bottom, left-to-right, according to this recurrence. Then return $\mathrm{DP}[n-1][B]$.

**Correctness:** We determine, for each subarray starting at $a_0$ and ending at $a_i$, whether it is possible to generate the sum $j$ from that subarray's elements, for each $j \le B$. For each terminating element and target sum, this is true (trivially) if and only if either of the following are true:

1. We can compute the target sum $j$ using the subarray terminating one element before our current terminating element $a_i$.
2. We can compute the difference $j - a_i$ using the one-shorter subarray (as in (1)).

The first case means we exclude $a_i$ from the running sum, while the second case means that we include it. Given the initial conditions in the algorithm above, it is clear that we compute correct results for each $\mathrm{DP}[i][j]$ in order, up to $\mathrm{DP}[n-1][B]$, so the algorithm must be correct. Formally, this is an induction on $i$: row $0$ is correct by the initial conditions, and each later row $i$ is correct because it follows from row $i-1$ by the recurrence.

**Runtime:** We fill $n(B+1)$ cells, each requiring $O(1)$ time. So the total runtime is $O(nB + n)$. Since our input has length $\sum a_i + B$ in unary, where $\sum a_i$ is necessarily greater than or equal to $n$, this runtime is polynomial in the length of our input.

> [!note]- Source page
> ![[attachments/exercise-0.20.jpg|500]]