**1.** Show that, if $\mathrm{P} = \mathrm{NP}$, then $\text{MIN FORMULA} \in \mathrm{P}$.

We must work with the following formulation of MIN FORMULA:

$$ \text{MIN FORMULA} = \lbrace \text{Boolean formula } \varphi : \exists \psi, |\psi| \le L : \forall \vec{x}, \psi(\vec{x}) = \varphi(\vec{x}) \rbrace $$

Starting with the inner quantifier (showing the equivalence of $\psi$ and $\varphi$ under all inputs), we note the following:

1. It is very easy to show $\psi \not\equiv \varphi$, as it requires only a single input as a counterexample; this exercise is therefore in $\mathrm{NP}$ (evaluation is polynomial in the length of the formula).
2. Consequently, showing $\psi \equiv \varphi$ is in $\mathrm{coNP}$.
3. $\mathrm{P} = \mathrm{NP} \Rightarrow \mathrm{NP} = \mathrm{coNP} = \mathrm{P}$ because $\mathrm{P}$ is closed under complement.
4. So we have some polynomial time subroutine to show that $\forall \vec{x}, \psi(\vec{x}) = \varphi(\vec{x})$.

Now we reduce our MIN FORMULA formulation to an $\mathrm{NP}$ decision problem using $\psi$ as the witness. Assuming that $L < |\varphi|$ (as the problem is otherwise trivial), we must only check that $|\psi| \le L$ and that $\forall \vec{x}, \psi(\vec{x}) = \varphi(\vec{x})$. Both steps of this process are polynomial time under the assumption that $\mathrm{P} = \mathrm{NP}$, as explained above. We have a polynomial-time verifier for MIN FORMULA, so it is in $\mathrm{NP}$. Further, since we have assumed $\mathrm{P} = \mathrm{NP}$, it is also in $\mathrm{P}$. $\blacksquare$

> [!note]- Source page
> ![[attachments/exercise-0.31.jpg|500]]