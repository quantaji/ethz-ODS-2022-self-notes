# Chapter 5 Coordinate Descent
- **Key Idea** Update only one coordinate
- **Results:** Worse case $d$ times more of iteration, under suitable condition results may improve.

## Polyak-Łojasiewicz inequality
- *Goal* only to prove function values can converge to optimal, no care of complexity.
- **Definition 5.1** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ has a global minimum $\mathbf{x}^{\star}$. We say that $f$ satisfies the *Polyak-Łojasiewicz inequality* (PL inequality) if $\exists\mu > 0$ s.t. $\forall \mathbf{x} \in \mathbb{R}^{d}, \frac{1}{2}\Vert\nabla f(\mathbf{x})\Vert^{2} \geq \mu\left(f(\mathbf{x})-f\left(\mathbf{x}^{\star}\right)\right)$.
- **Lemma 5.2 (Strong Convexity -> PL ineq)** The proof is in *Lemma E* of lecture 03.
- PL ineq is strictly weaker than strong convexity.
    -  E.g. $f\left(x_{1}, x_{2}\right)=x_{1}^{2}$ is not strongly convex, but satisfies PL ineq.
    -  Example of non-convex function that satisfies PL ineq: $f(x)=x^2 + \mathrm{Sigmoid}(\frac{x}{20})\times 5 x^2$.
-  **Theorem 5.3 (Exponential decay)** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ has a global minimum $\mathbf{x}^{\star}$. Suppose that $f$ is $L$-smooth satiefies $\mu$-PL ineq. Choosing stepsize $\gamma = L^{-1}$, then gradient descent starting with arbitrary $\mathbf{x}_0$ satiefies $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq\left(1-\frac{\mu}{L}\right)^{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$
    -  **Proof**
        -  By sufficient descent $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L}\Vert\nabla f\left(\mathbf{x}_{t}\right)\Vert^{2}$,
        -  by PL ineq $\mathsf{RHS} \leq f\left(\mathbf{x}_{t}\right)-\frac{\mu}{L}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right)$, then we get what we want.

## Coordinate Smoothness
- **Definition 5.4** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ and $\mathcal{L} = (L_1 , L_2 , \ldots, L_d ) \in \mathbb{R}^d $. Function **f** is called *coordinate-wise smooth (with parameter $\mathcal{L}$)* if $\forall i \in [d]$, $\forall \mathbf{x} \in \mathbb{R}^{d}, \lambda \in \mathbb{R},f\left(\mathbf{x}+\lambda \mathbf{e}_{i}\right) \leq f(\mathbf{x})+\lambda \nabla_{i} f(\mathbf{x})+\frac{L_{i}}{2} \lambda^{2}$
    - If $\forall i, L_i = L$, $f$ is said to be coordinate-wise smooth with parameter $L$.
- Coordinate-wise smoothness is more fine grined.
    - $f\left(x_{1}, x_{2}\right)=x_{1}^{2}+10 x_{2}^{2}$ is $(2, 20)$-coordinate-smooth, but only $20$-smooth.
    - $f\left(x_{1}, x_{2}\right)=x_{1}^{2}+x_{2}^{2}+M x_{1} x_{2}$ is $(2,2)$-coordinate-smooth, but only $(M+2)$-smooth.
- **Algorithm**
    - (i) choose an active coordinate $i \in [d]$
    - (ii) $\mathbf{x}_{t+1}:=\mathbf{x}_{t}-\gamma_{i} \nabla_{i} f\left(\mathbf{x}_{t}\right) \mathbf{e}_{i}$
- **Lemma 5.5 (Sufficient Descent)** A stepsize of $\gamma_i = L_i^{-1}$ gives $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L_{i}}\vert\nabla_{i} f\left(\mathbf{x}_{t}\right)\vert^{2}$.

### Randomized CD
- **Algorithm Change** (i) sample $i \in [d]$ uniformly at random.
- **Theorem 5.6** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ has a global minimum $\mathbf{x}^{\star}$. Suppose that $f$ is coordinate-wise $L$-smooth and satisfies the $\mu$-PL inequality. Choosing stepsize $\gamma_i = \ L^{-1}$, then randomized coordinate descent with arbitrary start satisfies $\mathbb{E}\left[f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right)\right] \leq\left(1-\frac{\mu}{d L}\right)^{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$.
    - **Proof**
        - Take expectation over sufficient descent with each coordiate of probability $1/d$, $\mathbb{E}\left[f\left(\mathbf{x}_{t+1}\right) \vert \mathbf{x}_{t}\right] \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L} \sum_{i=1}^{d} \frac{1}{d}\vert\nabla_{i} f\left(\mathbf{x}_{t}\right)\vert^{2} =f\left(\mathbf{x}_{t}\right)-\frac{1}{2 d L}\Vert\nabla f\left(\mathbf{x}_{t}\right)\Vert^{2}$
        - With PL ineq, $\mathsf{RHS} \leq f\left(\mathbf{x}_{t}\right)-\frac{\mu}{d L}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right)$
        - Take expectation over condition $\mathbb{E}[\cdot \vert \mathbf{x}_{t}]$, we arrive at our conclusion.
    - **Comment** $\left(1-\frac{\mu}{L}\right) \approx\left(1-\frac{\mu}{d L}\right)^{d}$, this is nearly the same as vanilla GD, need improvement.

### Importance Sampling
- **Algorithm Change** (i) when $L_i$ not the same, sample $i\in [d]$ with probability $\frac{L_{i}}{\sum_{j=1}^{d} L_{j}}$
- **Theorem 5.7** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ has a global minimum $\mathbf{x}^{\star}$. Suppose that $f$ is coordinate-wise $\mathcal{L}$-smooth and satisfies the $\mu$-PL inequality. Let $\bar{L}=\frac{1}{d} \sum_{i=1}^{d} L_{i}$ be the average of all coordinate-wise smoothness constants. Then coordinate descent with importance sampling with arbitrary start satisfies $\mathbb{E}\left[f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right)\right] \leq\left(1-\frac{\mu}{d \bar{L}}\right)^{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$
    - **Proof**
        - $\mathbb{E}\left[f\left(\mathbf{x}_{t+1}\right) \vert \mathbf{x}_{t}\right] \leq f\left(\mathbf{x}_{t}\right)- \sum_{i=1}^{d}  \frac{1}{2 L} \frac{L_{i}}{\sum_{j=1}^{d} L_{j}}\vert\nabla_{i} f\left(\mathbf{x}_{t}\right)\vert^{2} =f\left(\mathbf{x}_{t}\right)-\frac{1}{2 d \bar{L}}\Vert\nabla f\left(\mathbf{x}_{t}\right)\Vert^{2}$
    - **Comment** $\bar{L}$ can be much smaller than $L=\max _{i=1}^{d} L_{i}$, this is a improvement in constant. When all $L$ are equal, no improvement.

### Steepest Coordinate Descent
- **Algorithm Change** (i) Choose $i=\underset{i \in[d]}{\operatorname{argmax}}\vert\nabla_{i} f\left(\mathbf{x}_{t}\right)\vert$, also called *Gauss-Southwell* rule.
- Since $\max _{i}\vert\nabla_{i} f(\mathbf{x})\vert^{2} \geq \frac{1}{d} \sum_{i=1}^{d}\vert\nabla_{i} f(\mathbf{x})\vert^{2}$, for a coordinate-$L$-smooth function, we have 
    - $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L} \max _{i}\vert\nabla_{i} f(\mathbf{x})\vert^{2}\leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L} \sum_{i=1}^{d} \frac{1}{d}\vert\nabla_{i} f\left(\mathbf{x}_{t}\right)\vert^{2} $
    - Then we arrive at **Corollary 5.8**, still $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq\left(1-\frac{\mu}{d L}\right)^{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$

#### Better Convergence of **Steepest Descent** with $\ell_1$ norm strong convexity (Also known as **Steeper**)
- **Lemma 5.9 ($\ell_1$ norm $\mu$-strong convexity -> $\ell_\infty$ norm $\mu$-PL inequality)** This is a natual resutl for *Lemma E*, as the dual norm of $\lVert\cdot \rVert_{1}$ is $\lVert\cdot \rVert_{\infty}$.
- Proof of dual norm $\lVert\cdot \rVert_{p *} = \lVert\cdot \rVert_{q}$ where $p^{-1} + q^{-1} = 1$ is by Hölder inequality $\sum_{k=1}^{n}\vert x_{k} y_{k}\vert \leq\left(\sum_{k=1}^{n}\vert x_{k}\vert^{p}\right)^{\frac{1}{p}}\left(\sum_{k=1}^{n}\vert y_{k}\vert^{q}\right)^{\frac{1}{q}}$ (equality holds if $x^p$ and $y^q$ are proportional.)
    - Proof of special case of $(1, \infty)$ is simple.
- **Theorem 5.10** Let $f: \mathbb{R}^d \to \mathbb{R} \in C^1$ has a global minimum $\mathbf{x}^{\star}$. Suppose that $f$ is coordinate-wise $L$-smooth and satisfies the $\mu_1$-PL inequality under $\ell_1$ norm. Then *steepest coordinate descent* with step size $\gamma_i = L^{-1}$ arbitrary start satisfies $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq\left(1-\frac{\mu_{1}}{L}\right)^{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$.
    - **Key Idea** $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L} \max _{i}\vert\nabla_{i} f(\mathbf{x})\vert^{2} = f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L}\Vert\nabla f\left(\mathbf{x}_{t}\right)\Vert_{\infty}^{2}$
- **Comment** Since $\Vert\mathbf{y}-\mathbf{x}\Vert_{1} \geq\Vert\mathbf{y}-\mathbf{x}\Vert \geq\Vert\mathbf{y}-\mathbf{x}\Vert_{1} / \sqrt{d}$
    - If $f$ is $\ell_1$ $\mu$-strong convex -> $f$ is $\ell_2$ $\mu$-strong convex.
    - If $f$ is $\ell_2$ $\mu$-strong convex -> $f$ is $\ell_1$ $\mu/d$-strong convex.
    - Seems no better than $\ell_2$ case, but cases like $f(x) = x^{\top}\mathrm{diag}\{\lambda_i\} x/2$ shows $\ell_1$ gives much better results. [(Appendic C of this)](https://arxiv.org/abs/1506.00552)
    - Has sth to do with *convex conjugates*.

### Greedy coordinate descent
- **Algorithm Change** (ii) $\mathbf{x}_{t+1}:=\underset{\lambda \in \mathbb{R}}{\operatorname{argmin}} f\left(\mathbf{x}_{t}+\lambda \mathbf{e}_{i}\right)$
    - Might be easier when $f$ is analytic.
- **Problem** May stuck at non-minimum. 
    - Example: $f(\mathbf{x}):=\Vert\mathbf{x}\Vert^{2}+\vert x_{1}-x_{2}\vert$, $(x,x)$ is minimal for all single coordinate in range $\vert x\vert \leq 1 / 2$.
    - The following form is guaranteed to not be.
- **Theorem 5.11**  Let $f: \mathbb{R}^{d} \rightarrow \mathbb{R}$ be of the form $f(\mathbf{x}):=g(\mathbf{x})+h(\mathbf{x})$ where $h(\mathbf{x})=\sum_{i} h_{i}\left(x_{i}\right)$, $g, f_i$ all convex, $g\in C^1$, then whenever *greedy coordinate descent* makes no improvement, its in minimum.
    - **Proof**
        - No improvement means $\forall \lambda, i\in[d], f\left(\mathbf{x}+\lambda \mathbf{e}_{i}\right) \geq f\left(\mathbf{x}\right)$,
            - then $f(\mathbf{x} + (y_i- x_i) \mathbf{e}_i) \geq f(\mathbf{x}) $
            - let $p(z):=g(z, x_{-i}) - \partial_i g(x_i)(z-x_i)$ and $q(z):=h_i(z) + \partial_i g(x_i)(z-x_i)$, since $g$ convex and differentiabl, $p(z)$ reach global minimum at $z=x_i$, $f_i := f(z; x_{-i})= p(z) + q(z)$, so $f_i$ also reach global minimum at $z=x_i$.
            - Since $p(z)$ differentiabl, $\partial p(x_i) = 0$, if $q$ does not reach global minimum at $z = x_i$, then $\exists y$ s.t. $q(y) < q(x_i)$, then by convexit $q(x_i + \lambda (y-x_i)) \leq q(x_i) + \lambda (q(y) - q(x_i))$ is bounded by a negative slopt of $-(q(x_i) - q(y))$ at $z=x_i$.
            - This leads to contradiction where $f=p + q$ reaches minimum at $z=x$.
            - Therefore, $q(z)$ also reaches minimal at $z=x_i$.
        - The above discussion means $\forall y_i, q(y_i) \geq q(x_i)$, equivalently $h_i(y_i) +\partial_i g(\mathbf{x})(y_i - x_i) - h(x_i)\geq 0$
        - Since $g$ convex, $f(\mathbf{y}) \geq g(\mathbf{x}) + \nabla g(\mathbf{x})^{\top} (\mathbf{y}-\mathbf{x}) + h(\mathbf{y}) - h(\mathbf{x}) = f(\mathbf{x})+\sum_{i=1}^{d}\underbrace{\left(\nabla_{i} g(\mathbf{x})\left(y_{i}-x_{i}\right)+h_{i}\left(y_{i}\right)-h_{i}\left(x_{i}\right)\right)}_{\geq 0} \geq f(\mathbf{x})$
    - **Comment** LASSO $\min _{\mathbf{x} \in \mathbb{R}^{n}}\Vert A \mathbf{x}-\mathbf{b}\Vert^{2}+\lambda\Vert\mathbf{x}\Vert_{1}$ is of this form
        - Under mild regularity on $g$, convergence is affirmative.
