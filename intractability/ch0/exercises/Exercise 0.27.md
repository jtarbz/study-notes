**1.** Let $0 < \alpha < 1/2$. In the definition of RP, replace $1/2$ with $\alpha$ and call the resulting class $\mathrm{RP} _\alpha$. Show that $\mathrm{RP} _{1/2} = \mathrm{RP} _\alpha$.

We first show that any problem in $\mathrm{RP} _{1/2}$ can be converted to a problem in $\mathrm{RP} _\alpha$. Then we show the reverse direction to close the proof. Naturally, these must be polynomial time reductions for the proof to be correct.

Any problem in $\mathrm{RP} _{1/2}$ is trivially in $\mathrm{RP} _\alpha$ by the definition of $\mathrm{RP} _\alpha$, because $1/2 > \alpha$.

It is only slightly more difficult to show the reverse direction. Consider that the algorithm $M _\alpha$ for some problem $A \in \mathrm{RP} _\alpha$ reports a false negative (says $x \notin A$ when it actually is in $A$) with probability $(1 - \alpha)$. We must improve this false negative rate to at most $1/2$ using a polynomial time reduction. The key is to run $M _\alpha$ up to $n$ times and report TRUE when any of the $n$ iterations reports TRUE. We determine $n$ like so:

$$ P[FN] = (1 - \alpha)^n $$
$$ P[FN] \le 1/2 \Rightarrow (1 - \alpha)^n \le 1/2 $$
$$ \log_2[(1 - \alpha)^n] \le \log_2[1/2] $$
$$ n \log_2[1 - \alpha] \le \log_2[1/2] $$
$$ n \ge \frac{\log_2(1/2)}{\log_2[1 - \alpha]} $$

So we pick $n = \left\lceil \dfrac{\log _2(1/2)}{\log _2(1 - \alpha)} \right\rceil$. Since $n$ depends only on the constant $\alpha$, the resulting modification of $M _\alpha$ runs in $n \cdot \text{poly}(|x|)$ time, which is always polynomial in the length of an input $x$. This is sufficient to show that $\mathrm{RP} _\alpha \subseteq \mathrm{RP} _{1/2}$.

$\mathrm{RP} _\alpha \subseteq \mathrm{RP} _{1/2} \wedge \mathrm{RP} _{1/2} \subseteq \mathrm{RP} _\alpha \Rightarrow \mathrm{RP} _\alpha = \mathrm{RP} _{1/2}$. $\blacksquare$

**2.** Let $\alpha(n)$ be a decreasing function from $\mathbb{N}$ to $(0, 1/2)$. In the definition of RP, replace $1/2$ with $\alpha(|x|)$ and call the resulting class $\mathrm{RP} _{\alpha(n)}$. Is $\mathrm{RP} _{1/n^2} = \mathrm{RP}$? Is $\mathrm{RP} _{1/2^n} = \mathrm{RP}$?

$\underline{\mathrm{RP} _{1/n^2} = \mathrm{RP}}$. To see this, we consider the inequality from (1), substituting the false negative rate $(1 - 1/n^2)$ for the previously constant rate $(1 - \alpha)$:

$$ y \ge \frac{-1}{\log _2(1 - 1/n^2)} $$

where $y$ is the required number of iterations. It is clear that $\left|\log _2(1 - 1/n^2)\right|$ shrinks quadratically with $n$, so $y$ grows quadratically with $n$. Since $y$ is quadratically bounded with respect to the input length $n$, we need only rerun $M(x)$ a polynomially-bounded number of times, so $\mathrm{RP} _{1/n^2} = \mathrm{RP}$ by the closure of polynomials under multiplication.

$\underline{\mathrm{RP} _{1/2^n} \ne \mathrm{RP}}$. _Probably_. By the same argument, it is clear that $\alpha(n) = 1/2^n$ requires rerunning the machine an exponentially-bounded number of times to achieve the required false negative rate. However, it is difficult to prove that some other method cannot help to achieve the required rate.

Consider that $\mathrm{SAT} \in \mathrm{RP} _{1/2^n}$. Picking a random assignment of $x$ variables, where $x$ is the length of the $\mathrm{SAT}$ instance, has a $1/2^n$ chance of being correct. So $\mathrm{RP} _{1/2^n} = \mathrm{RP}$ would imply that there is some randomized polynomial solution to $\mathrm{SAT}$, which seems too good to be true. $\blacksquare$

> [!note]- Source page
> ![[attachments/exercise-0.27-1.jpg|500]]
> ![[attachments/exercise-0.27-2.jpg|500]]