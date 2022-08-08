# Chapter 6 Subgradient Methods

## Subgradient and Subdifferential
- **Definition (Subgradient)** Let $f: \mathbb{R}^{n} \rightarrow \mathbb{R} \cup\{+\infty\}$ be convex. A vector $g \in \mathbb{R}^{n}$ is a *subgradient* of $f$ at a point $x \in \operatorname{dom}(f)$ if $f(y) \geq f(x)+g^{\top}(y-x), \forall y \in \operatorname{dom}(f)$.
    - The set of all subgradient at $x$ is called the *subdifferential* of $f$ at $x$ denoted as $\partial f$.
- **Lemma 6.2 (uniqueness when differentiable)** If $f$ is convex and differentiable at $x \in \operatorname{dom}(f)$ , then $\partial f = \{\nabla f(x)\}$.
    - **Proof**
        - Let $y=x+\epsilon d$, if $g\in\partial f$, then $f(x+\epsilon d) \geq f(x)+\epsilon g^{\top} d$, then $\frac{f(x+\epsilon d)-f(x)}{\epsilon} \geq g^{\top} d, \forall d, \forall \epsilon$.
        - Letting $\epsilon \to 0$, then we have $\nabla f(x)^{\top} d \geq g^{\top} d, \forall d$. This only holds when $g = \nabla f(x)$.
        - If $f$ convex, we can check the definition of subgradient holds.
    - **Or** can use Taylor expanssion and get $(\mathbf{g}-\nabla \mathbf{f}(\mathbf{x}))^{\top}(\mathbf{y}-\mathbf{x}) \leq \mathrm{r}(\mathbf{y}-\mathbf{x})$, then let $\mathbf{y}=\mathbf{x}+\epsilon(\mathbf{g}-\nabla \mathbf{f}(\mathbf{x}))$.
- **Lemma 6.3** If we don't assume convexity, we can only say $\partial f(x) \subseteq\{\nabla f(x)\}$.
- **Examples 6.4**
    1. $f(x)=\frac{1}{2} x^{2}, \partial f(x)=x$
    2. $f(x)=\vert x\vert, \partial f(x)=\left\{\begin{array}{l}\operatorname{sgn}(x), x \neq 0 \\{[-1,1], x=0}\end{array}\right.$
    3. $f(x)=\left\{\begin{array}{l}-\sqrt{x}, x \geq 0 \\+\infty, o . w .\end{array} \quad, \partial f(0)=\emptyset ;\right.$
    4. $f(x)=\left\{\begin{array}{l}1, x=0 \\0, x>0 \\+\infty, o . w\end{array}, \partial f(0)=\emptyset\right.$
- **Lemma 6.5** Let $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$ be convex, $\operatorname{dom}(f)$ open, $B \in \mathbb{R}_{+}$. Then $\forall x\in\mathbf{dom}(d), \forall g\in\partial f(x), \Vert g\Vert_{a*}\leq B$ $\Leftrightarrow$ $\forall x,y\in\mathbf{dom}(d), \vert f(x)-f(y)\vert \leq B\Vert x-y\Vert_{a}$.
    - **Proof**
        - (->)
            - Subgradient -> $f(y) - f(x) \leq g_y^{\top}(y-x)$ and $f(x) - f(y) \leq g_x^{\top}(y-x)$
            - This means $\vert f(y) - f(x)\vert \leq \max\{ g_y^{\top}(y-x),  g_x^{\top}(y-x)\} \leq \max\{\Vert g_y\Vert_{a*},  \Vert g_x\Vert_{a*}\} \Vert y-x\Vert_{a}$
            - If $\Vert g\Vert_{a*} \leq B$ we arrive at Lipschitz
        - (<-)
            - $f(y) \geq f(x) + g_x^{\top}(y-x)$, by Lipschitz, we have $B\Vert x-y\Vert_{a} \geq g_x^{\top}(y-x), \forall y$. Choosing $y-x = \arg\max_{\Vert y-x\Vert=1} g_x^{\top}(y-x)$, we get maximum of $\Vert g_x\Vert_{a*}\leq B$. This is true forall $x$.

### Topological Properties of Subgradients
- **Lemma 6.6** Let $f(x)$ be a convex function and $x \in \operatorname{dom}(f)$. Then $\partial f(x)$ is convex and closed.
    - **Proof** $\partial f(x) = \cap_{y}\left\{g \in \mathbb{R}^{n}: f(y) \geq f(x)+g^{\top}(y-x)\right\}$, solution of $g$ for each $y$ is convex and closed linear separation.
- **Definition 6.7 (separation of convex sets)** Let $S$ and $T$ be two nonempty convex sets in $\mathbb{R}^n$. A hyperplane $H=\left\{x \in \mathbb{R}^{n}: a^{\top} x=b, a\neq 0\right\}$ is said to *separate* $S$ and $T$ if $S \cup T \not \subset H$ and $S \subset H^{-}=\left\{x \in \mathbb{R}^{n}: a^{\top} x \leq b\right\}$ and $T \subset H^{+}=\left\{x \in \mathbb{R}^{n}: a^{\top} x \geq b\right\}$.
    - PS: It's OK that one of $S$ or $T$ is a point, or part of the hyperplane.
    - *Strictly separation* if $S \subset H^{--}=\left\{x \in \mathbb{R}^{n}: a^{\top} x<b\right\}$ and $T \subset H^{++}=\left\{x \in \mathbb{R}^{n}: a^{\top} x>b\right\}$.
- **Theorem 6.8 (Hyperplane separation theorem, [Roc97, Thm 11.3], no proof)** Let $S$ and $T$ be two nonempty convex sets. Then S and T can be separated if and only if their (relative) interiors do not intersect, $\operatorname{rint}(S) \cap \operatorname{rint}(T)=\emptyset$.
    - **PS** The *Relative Interior* of a general non=convex set is defined to be $\operatorname{relint}(C):=\{x \in C: \forall y \in C, \exists \lambda>1, \text { s.t. } \lambda x+(1-\lambda) y \in C\}$
- **Corollary 6.9** Let $S$ be a nonempty convex set and $x_0\in\partial S$. $\exists H=\left\{x: a^{\top} x=a^{\top} x_{0}\right\}, a\neq 0$ s.t. $S \subset\left\{x: a^{\top} x \leq a^{\top} x_{0}\right\}, $ and $x_{0} \in H$.
    - just let $T=\{x_0\}$.
- **Theorem 6.10 (non emptiness of subgradient for interior)** Let $f(x)$ be a convex function and $x \in \operatorname{rint}(\operatorname{dom}(f))$. Then $\partial f(x)$ is nonempty and bounded if $x \in \operatorname{rin} t(\operatorname{dom}(f))$.
    - **Proof**
        - *Non-emptiness*
            - W.l.o.g., assume $\operatorname{dom}(f)$ is full-dimensional and $x \in \operatorname{int}(\operatorname{dom}(f))$ (*interior*).
            - $\mathrm{epi}(f)$ is convex and $(x, f(x))$ belongs to its boundary, by Thm 6.8 $\exists \alpha=(s, \beta) \neq 0$ s.t. $s^{\top} y+\beta t \geq s^{\mathcal{T}} x+\beta f(x), \forall(y, t) \in \operatorname{epi}(f)$
            - We claim $\beta > 0$. 
                - If $\beta < 0$, we can let $t\to\infty$ where $(y, t) \in \operatorname{epi}(f)$.
                - if $\beta = 0$,  this contradict to $s^{\top} y \geq s^{\mathcal{T}} x$, since $x$ is the interior, we can always find opposite direction to reverse the inequality.
            - Setting $g=-\beta^{-1} s$, we have $f(y) \geq f(x)+g^{\top}(y-x), \forall y$
        - *Boundedness*
            - If $\partial f(x) \nless \infty$, $\exists \{g_k\}_{k=1}^{\infty} \in \partial f(x)$ s.t. $\lim_{k\to\infty}\Vert g_k\Vert_2 = \infty$.
            - Since $x$ interior, exists ball $B(x, \delta) \subseteq \mathbf{dom}(f)$, then $y_{k}=x+\delta \frac{g_{k}}{\Vert g_{k}\Vert_{2}} \in \operatorname{dom}(f)$, by convexity, $f\left(y_{k}\right) \geq f(x)+g_{k}^{\top}\left(y_{k}-x\right)=f(x)+\delta\Vert g_{k}\Vert_{2} \rightarrow \infty$, contradiction.
- **Lemma A (non-emptyness -> convexity)** If $\forall x \in \operatorname{dom}(f), \partial f(x)\neq \emptyset$ and $\mathbf{dom}(f)$ convex, then $f$ convex.
    - **Proof** 
        - $z=\lambda x+(1-\lambda) y \in \operatorname{dom}(f)$ for $\lambda \in [0,1]$.
        - Let $g \in \partial f(z)$, we have $f(x) \geq f(z)+g^{\top}(x-z), f(y) \geq f(z)+g^{\top}(y-z)$, add them with weight $(\lambda, 1-\lambda)$ we get convexity.
- **Remark 6.11 (Exercise 42)** The subdifferential of a convex function $f(x)$ at $x \in \operatorname{dom}(f)$ is a monotone operator, i.e., $(u-v)^{\top}(x-y) \geq 0, \forall x, y \in \operatorname{dom}(f), u \in \partial f(x), v \in \partial f(y)$. *Proof similar to gradient*.


### Subdifferential and directional derivative
- **Def directional derivative** $f^{\prime}(x ; d)=\lim _{\delta \rightarrow 0^{+}} \frac{f(x+\delta d)-f(x)}{\delta}$.
- **Def differentiable** $f$ *differentiable* if $\Delta f = A \Delta x + o(\Delta x)$.
    - $\partial f$ can exists when $\mathrm{d} f$ not.
- **Lemma 6.12 (Exercise 43)** Let $f$ convex, the ratio $\phi(\delta):=\frac{f(x+\delta d)-f(x)}{\delta}$ is non-decreasing of $\delta>0$.
    - **Proof** Let $\delta_1 \geq \delta_2$, then let $\lambda := \delta_2/ \delta_1$, by convexity $\phi(\delta_2) = \frac{f(x+\lambda \delta_1 d)-f(x)}{\lambda \delta_1} \leq \frac{\lambda f(x+ \delta_1 d) + (1-\lambda) f(x) -f(x)}{\lambda \delta_1} = \phi(\delta_1)$.
- **Theorem 6.13**  Let $f$ be convex and $x \in \operatorname{int}(\operatorname{dom}(f))$, then $f^{\prime}(x ; d)=\max _{g \in \partial f(x)} g^{\top} d$.
    - **Proof**
        - By def of subgrad, we have $f^{\prime}(x ; d) \geq \max _{g \in \partial f(x)} g^{\top} d$.
        - Now we want to also prove the opposite.
            - Define $C_{1}=\{(y, t): f(y)<t\}$, (not $\mathbf{epi}(f)$), and $C_{2}(d)=\left\{(y, t): y=x+\alpha d, t=f(x)+\alpha f^{\prime}(x ; d), \alpha \geq 0\right\}$.
                - so both $C_1, C_2$ convex, non-empty.
            - Since $\phi(\delta)$ non-decreasing, $(t>)$ $f(x+\alpha d) \geq f(x)+\alpha f^{\prime}(x ; d), \forall \alpha \geq 0$, so $C_{1} \cap C_{2}=\emptyset$
            - By hyperplane separation theorem $\exists\left(g_{0}, \beta\right) \neq 0$ s.t. $g_{0}^{\top}(x+\alpha d)+\beta\left(f(x)+\alpha f^{\prime}(x ; d)\right) \leq g_{0}^{\top} y+\beta t, \forall \alpha \geq 0, \forall t>f(y)$
                - Similar to proof of Thm 6.10, we claim $\beta > 0$. Let $\tilde{g}=\beta^{-1} g_{0}$
                - we get $\tilde{g}^{\top}(x+\alpha d)+f(x)+\alpha f^{\prime}(x ; d) \leq \tilde{g}^{\top} y+f(y), \forall \alpha \geq 0$.
                - Setting $\alpha = 0$, we get $\tilde{g}^{\top} x+f(x) \leq \tilde{g}^{\top} y+f(y) \Leftrightarrow-\tilde{g} \in \partial f(x)$
                - setting $\alpha = 1, y=x$, we get $\tilde{g}^{\top} d+f^{\prime}(x ; d) \leq 0 \Leftrightarrow f^{\prime}(x ; d) \leq-\tilde{g}^{\top} d \leq \max _{g \in \partial f(x)} g^{\top} d$
        - This mean $f^{\prime}(x ; d) = \max _{g \in \partial f(x)} g^{\top} d$.
- **Theorem B [Roc97, Theorem 25.5] (differentiable almost everywhere)** A (proper) convex function $f$ is diﬀerentiable almost everywhere on (the interior of) $\mathbf{dom}(f)$.
- **Lemma C (optimal condition)** If $0 \in \partial f(x)$, then $x$ is a global minimum. **Proof** $f(\mathbf{y}) \geq f(\mathbf{x})+\mathbf{g}^{\top}(\mathbf{y}-\mathbf{x})=f(\mathbf{x})$.

### Calculus of Subgradient
- **Conic combination** $h(x)=\lambda f(x)+\mu g(x)$, then $\partial h(x)=\lambda \partial f(x)+\mu \partial g(x), \forall x \in \operatorname{int}(\operatorname{dom}(h))$.
- **affine composition** $h(x)=f(A x+b)$, then $\partial h(x)=A^{T} \partial f(A x+b)$.
- **supremum** $h(x)=\sup _{\alpha \in \mathcal{A}} f_{\alpha}(x)$, then $\partial h(x) \supseteq \operatorname{conv}\left\{\partial f_{\alpha}(x) \vert \alpha \in \alpha(x)\right\}$
- **superposition** $h(x)=F\left(f_{1}(x), \ldots, f_{m}(x)\right)$ where $F\left(y_{1}, \ldots, y_{m}\right)$ non-decreasing and convex, then $\partial h(x) \supseteq\left\{\sum_{i=1}^{m} d_{i} \partial f_{i}(x):\left(d_{1}, \ldots, d_{m}\right) \in \partial F\left(y_{1}, \ldots, y_{m}\right)\right\}$.
- **Example 6.14** Let $h(x)=\max _{y \in C} f(x, y)$ where $f(x,y)$ is convex in $x$ for any $y$ and $C$ is closed. Then $\partial f\left(x, y_{*}(x)\right) \subset \partial h(x)$, where $y_{*}(x)=\operatorname{argmax}_{y \in C} f(x, y)$
    - **Proof** Let $g \in \partial f\left(x, y_{*}(x)\right)$, then $h(z) \geq f\left(z, y_{*}(x)\right) \geq f\left(x, y_{*}(x)\right)+g^{T}(z-x)=h(z)+g^{T}(z-x)$
- **Lemma 6.15 (Exercise 44)** Consider the function $f(x)=\Vert x\Vert _{a}$. Then $\partial f(x)=\left\{g: g^{\mathcal{T}} x=\Vert x\Vert _{a} \;\wedge\;\Vert g\Vert _{a*} \leq 1\right\}$. (In particular $\partial f(0) = \left\{g:\Vert g\Vert _{a*} \leq 1\right\}$.
    - **Proof**
        - $g \in \partial f(x) \Leftrightarrow \forall \delta, \Vert x+\delta\Vert _{a} \geq \Vert x\Vert _{a} + g^{\top} \delta$
            - setting $\delta = -x$, we get $0 = \Vert 0\Vert _{a} = \Vert x\Vert _{a} - g^{\top} x$.
            - By triangular ineq, $\Vert x\Vert _{a} + \Vert \delta\Vert _{a} \geq \Vert x+\delta\Vert _{a} \geq \Vert x\Vert _{a} + g^{\top} \delta \Rightarrow \Vert \delta\Vert _{a} \geq g^{\top} \delta, \forall \delta$.
                - subjecting to $\Vert \delta\Vert _{a}=1$ we get $\Vert g\Vert _{a*} \leq 1$.

            
## Subgradient Methods
- **Remark 6.16** Negative subgradient may not be a descending direction.
    - *Example* $f\left(x_{1}, x_{2}\right)=\vert x_{1}\vert +2\vert x_{2}\vert$ at $\mathbf{x}=(1,0), \partial f(\mathbf{x})=\{(1, a): a \in[-2,2]\}$
        - $\mathbf{g}=(1,0), \mathbf{d}=-\mathbf{g}$ is descending direction, while $\mathbf{g}=(1,2), \mathbf{d}=-\mathbf{g}$ is not.
- **Problem** $\min f(x)$ s.t. $x\in X$. *Denote*:
    -  $R:=\sqrt{\max _{x, y \in X}\Vert x-y\Vert _{2}^{2}}$ the diameter of $X$.
    - $B:=\sup _{x, y \in X} \frac{\vert f(x)-f(y)\vert }{\Vert x-y\Vert _{2}}<+\infty$ the Lipschitz constant under $\ell_2$ norm.

### Subgradient Descent
- **Algorithm** $x_{t+1}=\Pi_{X}\left(x_{t}-\gamma_{t} g\left(x_{t}\right)\right)$, where $g\left(x_{t}\right) \in \partial f\left(x_{t}\right)$.
- **Lemma D(Descent Lemma)** $f$ convex, for any optimal $\mathbf{x}^{*} \in X^{*}$, $\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2} \leq\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-2 \gamma_{t}\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right)+\gamma_{t}^{2}\lVert \mathbf{g}_{t}\rVert _{2}^{2}$
    - **Proof**
        - $\Vert \mathbf{x}_{t+1} - \mathbf{x}^{\star}\Vert ^2_2 = \Vert \Pi_{X}\left(\mathbf{x}_{t}-\gamma_{t} \mathbf{g}_{t}\right) - \mathbf{x}^{\star}\Vert ^2_2$, 
        - by Fact 4.1 (ii) $\mathsf{RHS} \leq \Vert \mathbf{x}_{t}-\gamma_{t} \mathbf{g}_{t} - \mathbf{x}^{\star}\Vert ^2_2 =  \Vert \mathbf{x}_{t} - \mathbf{x}^{\star}\Vert ^2_2 - 2\gamma_t\langle \mathbf{g}_{t}, \mathbf{x}_{t} - \mathbf{x}^{\star} \rangle + \gamma_{t} ^2\Vert \mathbf{g}_{t}\Vert ^2_2$
        - By convexity $\langle \mathbf{g}_{t}, \mathbf{x}_{t} - \mathbf{x}^{\star} \rangle \leq f(\mathbf{x}_{t}) - f^{\star}$, then we arrive at conclusion.


### $\mathcal{O}(1/\sqrt{t})$ convergence for $B$-Lipschitz convex functions
- **Theorem 6.17** $f$ convex, then subgradient descent gives $\max\{\min _{1 \leq t \leq T} f\left(\mathbf{x}_{t}\right), f\left(\hat{x}_{T}\right) \} - f^{*}\leq \frac{\lVert \mathbf{x}_{1}-\mathbf{x}^{*}\rVert _{2}^{2}+\sum_{t=1}^{T} \gamma_{t}^{2}\lVert \mathbf{g}_{t}\rVert _{2}^{2}}{2 \sum_{t=1}^{T} \gamma_{t}} \leq \frac{R^{2}+\sum_{t=1}^{T} \gamma_{t}^{2}B^2}{2 \sum_{t=1}^{T} \gamma_{t}}$.
    - **Proof**
        - By descent lemma $\gamma_{t}\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right) \leq \frac{1}{2}\left( \lVert x_{t}-x^{*}\rVert _{2}^{2}-\lVert x_{t+1}-x^{*}\rVert _{2}^{2}+\gamma_{t}^{2}\lVert g\left(x_{t}\right)\rVert _{2}^{2} \right)$
        - sum over $t=[1:T]$ and divided by $\sum_t \gamma_t$ gives upper bound of the average$\mathbb{E} f_{t}$, after that is straightforward.
- **Step size** is crucial for convergence behavior, unlike GD
    - *Constant Stepsize* $\gamma_t \equiv \gamma$, $\epsilon_{T} \leq \frac{R^{2}}{2 T} \cdot \frac{1}{\gamma}+\frac{B^{2}}{2} \gamma \stackrel{T \rightarrow \infty}{\longrightarrow} \frac{B^{2}}{2} \gamma$,
        - choosing $\gamma_{*}=\frac{R}{B \sqrt{T}}$ yields $\epsilon_{T} \leq \frac{R B}{\sqrt{T}}$.
    - *Scaled stepsize* $\gamma_{t}=\frac{\gamma}{\lVert \mathbf{g}_{t}\rVert _{2}}$, $\epsilon_{T} \stackrel{T \rightarrow \infty}{\longrightarrow}B \gamma / 2$
    - *Non-summable but diminishing stepsize* $\sum_{t=1}^{\infty} \gamma_{t}=\infty,  \lim _{t \rightarrow \infty} \gamma_{t}=0$
        - Since $\gamma_t^2$ approach zero faster than $\gamma_t$, $\epsilon_{T} \stackrel{T \rightarrow \infty}{\longrightarrow} 0$.
        - *Example* $\gamma_{t}=O\left(t^{-q}\right)$ with $q \in(0,1]$, e.g. $q=1/2$, then $\epsilon_{T} \leq O\left(\frac{B R \ln (T)}{\sqrt{T}}\right)$
            - If we average among $[T/2:T]$, then $\min _{\left\lfloor\frac{T}{2}\right\rfloor \leq t \leq T} f\left(x_{t}\right)-f^{*} \leq O\left(\frac{B R}{\sqrt{T}}\right)$
 - *Non-summable but square-summable (Robbins-Monro) stepsize* $\sum_{t=1}^{\infty} \gamma_{t}=\infty,  \sum_{t=1}^{\infty} \gamma_{t}^{2}<\infty$, e.g. $\gamma_t = t^{-1}$, $\epsilon_{T} \stackrel{T \rightarrow \infty}{\longrightarrow} 0$.

- *Polyak stepsize* Assume $f^{*}=f\left(x^{*}\right)$ is known and $\gamma_{t}=\frac{f\left(x_{t}\right)-f^{*}}{\lVert g\left(x_{t}\right)\rVert _{2}^{2}}$
    - Using descent lemma, we get $\lVert x_{t+1}-x^{*}\rVert _{2}^{2} \leq\lVert x_{t}-x^{*}\rVert _{2}^{2}-\frac{\left(f\left(x_{t}\right)-f^{*}\right)^{2}}{\lVert g\left(x_{t}\right)\rVert _{2}^{2}}$
    - then $(f(x_t) - f(x^{\star}))^2 \leq \Vert \mathbf{g}_t\Vert ^2  ( \lVert x_{t}-x^{*}\rVert _{2}^2 - \lVert x_{t+1}-x^{*}\rVert _{2}^2 ) \leq B^2 (\lVert x_{t}-x^{*}\rVert _{2}^2 - \lVert x_{t+1}-x^{*}\rVert _{2}^2)$
    - then $\sum_{t=1}^{T}\left(f\left(x_{t}\right)-f^{*}\right)^{2} \leq R^{2} \cdot M<\infty$, this means $f\left(\mathbf{x}_{t}\right) \rightarrow f^{*}$.
    -  **Note** this is *last iterate* convergence, not *average* convergence!
    - can also show that $\exists \mathbf{x}^{\star}\in X^{\star}$ s.t. $\lim \sup _{t \rightarrow \infty}\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2} \rightarrow 0$, converge to one of the optimal point.
        - Since series $\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}$ is bounded and non-increasing, there exists a subsequence $\left\{\mathbf{x}_{t_{k}}\right\}$ with accumulation point $\hat{\mathbf{x}}$,
        - Since we already have $f\left(\mathbf{x}_{t}\right) \rightarrow f^{*}$, then $\hat{\mathbf{x}} \in X^{*}$.
        - So $\lVert \mathbf{x}_{t+1}-\hat{\mathbf{x}}\rVert _{2}^{2} \leq\lVert \mathbf{x}_{t_{k}}-\hat{\mathbf{x}}\rVert _{2}^{2}-\sum_{j=t_{k}}^{t} \frac{\left(f\left(\mathbf{x}_{j}\right)-f^{*}\right)^{2}}{\lVert g\left(\mathbf{x}_{j}\right)\rVert _{2}^{2}}$, 
        - taking $\lim\sup$ we have $\lim\sup_{t\to\infty} \lVert \mathbf{x}_{t}-\hat{\mathbf{x}}\rVert _{2}^{2} \leq\lVert \mathbf{x}_{t_{k}}-\hat{\mathbf{x}}\rVert _{2}^{2}-\sum_{j=t_{k}}^{\infty} \frac{\left(f\left(\mathbf{x}_{j}\right)-f^{*}\right)^{2}}{\lVert g\left(\mathbf{x}_{j}\right)\rVert _{2}^{2}}$
        - then take $k\to\infty$, we get $\limsup _{t \rightarrow \infty}\lVert \mathbf{x}_{t}-\hat{\mathbf{x}}\rVert _{2}^{2} \leq \lim _{k \rightarrow \infty}\lVert \mathbf{x}_{t_{k}}-\hat{\mathbf{x}}\rVert _{2}^{2}=0$.

### Similar result under Polyak's step size (Exercise Prob 5)
- For non-differentiable convex $f$, $\mu$-strongly convex w.r.t. 2-norm is defined to be $f_{\mu}(x):=f(x)-\frac{\mu}{2}\Vert \mathbf{x}\Vert ^{2}$ is convex. It can also be shown that this means $f(y) \geq f(x)+g^{\top}(y-x)+\frac{\mu}{2}\Vert x-y\Vert ^{2}$.
- **Lemma I** If $f$ convex and $B$-Lipschitz, optimization in whole space $x \in \mathbb{R}^{d}$ with Polyak step size gives $\min _{1 \leq t \leq T} f\left(x_{t}\right)-f\left(x^{*}\right) \leq \frac{B\lVert x_{1}-x^{*}\rVert _{2}}{\sqrt{T}}$.
    - **Proof**
        - Denote $h_t:=f\left(x_{t}\right)-f\left(x^{*}\right)$ and $d_t:=\lVert x_{t}-x^{*}\rVert _{2}$, update gives $d_{t+1}^{2} \leq d_{t}^{2}-\frac{h_{t}^{2}}{\lVert \mathbf{g}_{t}\rVert _{2}^{2}}$
        - equivalently $h_{t}^{2} \leq\lVert \mathbf{g}_{t}\rVert _{2}^{2}\left(d_{t}^{2}-d_{t+1}^{2}\right) \leq B^{2}\left(d_{t}^{2}-d_{t+1}^{2}\right)$
        - sum over $t=[1:T]$ gives the result.
- **Lemma J** If $f$ $\mu$-strongly convex and $B$-Lipschitz, optimization in whole space $x \in \mathbb{R}^{d}$ with  Polyak step size gives $\min _{1 \leq t \leq T} f\left(x_{t}\right)-f\left(x^{*}\right) \leq \frac{4 B^{2}}{\mu T}$
    - **Proof**
        - Similarly, $d_{t+1}^{2} - d_{t}^{2} \leq -\frac{h_{t}^{2}}{\lVert \mathbf{g}_{t}\rVert _{2}^{2}} \leq  -\frac{h_{t}^{2}}{B^{2}} $
        - By strong convexity w.r.t. optimial point (since optimization on whole space, $0\in\partial f(\mathbf{x}^{\star})$, $h_{t} = f(\mathbf{x}_t) - f(\mathbf{x}^{\star}) \leq \mu d_t^2/2$,
            - this means $d_{t+1}^{2} - d_{t}^{2} \leq  -\frac{h_{t}^{2}}{B^{2}} \leq - \frac{\mu^2}{4B^2}d_t^4$. Denote $a_{t}=\mu^{2}\lVert x_{t}-x^{*}\rVert _{2}^{2} /\left(4 B^{2}\right)$, we have $a_{t+1} \leq a_t(1 - a_t)$
        - By strong convexity w.r.t. $x_0$ and $x^{\star}$, we have $f^{\star} \geq f(x_0) + \mathbf{g}_0^{\top}(x^{\star} - x_0) + \mu \Vert \Delta x_0\Vert _2^2 / 2$
            - by sufficient descent of Polyak step size, $0 \geq - h_0 \geq \mu d_0^2/2 - \mathbf{g}_0^{\top}\Delta x_0$,
            - this means $\mu d_0^2 /2 \geq \mathbf{g}_0^{\top}\Delta x_0 \leq B d_0$, take square we get $a_0 \leq 1 $.
        - We claim $a_t \leq 1/(t+1)$, by induction, we have $a_{t+1} \leq \frac{1}{t+2} \frac{t(t+2)}{(t+1)^2}$ (by monotonicity of $x(1-x)$ in $[0, 1/2]$)
            - Since $b_t := t/t+1$ is increasing, $b_{t}/b_{t+1} = \frac{t(t+2)}{(t+1)^2} \leq 1$, this means $a_{t+1} \leq \frac{1}{t+2}$, induction succeeds.
            - This means $d_t^2 \leq \frac{4B^2}{\mu^2} \frac{1}{t+1} \leq \frac{4B^2}{\mu^2} \frac{1}{t} $
        - Again, by $d_{t+1}^{2} - d_{t}^{2} \leq  -\frac{h_{t}^{2}}{B^{2}} $, sum over $t\in[T/2, T]$, we get $\sum_{t=T/2}^{T} h_t^2 \leq B^2 (d_{T/2}^2 - d^2_{T}) \leq  B^2 d_{T/2}^2 \leq \frac{8B^4}{\mu^2} \frac{1}{T}$
            - this means $\min_{t\in[T/2:T]} h_t^2 \leq \frac{16B^4}{\mu^2} \frac{1}{T^2}$, taking square root and we finish our proof.

    
### $\mathcal{O}(1 / t)$ convergence for strong convex functions
- **Lemma E(Descent Lemma under strong convexity)** $f$ is $\mu$-strongly convex, then $\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2} \leq\left(1-\mu \gamma_{t}\right)\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-2 \gamma_{t}\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right)+\gamma_{t}^{2}\lVert \mathbf{g}_{t}\rVert _{2}^{2}$
    - **Proof** Replace $\langle \mathbf{g}_{t}, \mathbf{x}_{t} - \mathbf{x}^{\star} \rangle \leq f(\mathbf{x}_{t}) - f^{\star}$ by $\langle \mathbf{g}_{t}, \mathbf{x}_{t} - \mathbf{x}^{\star} \rangle \leq f(\mathbf{x}_{t}) - f^{\star} - (\mu/2)\Vert \mathbf{x}_{t} - \mathbf{x}^{\star}\Vert _2^2$.
- There are two choice of step size $1/\mu t$ or $2/\mu(t+1)$, which give different convergence rate.
- **Theorem 6.18 ($\mathcal{O}(\ln T/T)$)** $f$ is $\mu$-strongly convex and $B$-Lipschitz, then subgradient descent of step size $\gamma_t = 1/\mu t$ gives $\max\{\min _{1 \leq t \leq T} f\left(\mathbf{x}_{t}\right), f\left(\hat{x}_{T}\right) \} - f^{*} \leq \frac{B^{2}(\ln (T)+1)}{2 \mu T}$ where $\hat{x}_{T}:=\frac{1}{T} \sum_{t=1}^{T} x_{t}$.
    - **Proof**
        - Plug in $\gamma = 1/\mu t$ we have $f\left(x_{t}\right)-f^{*} \leq\left(\frac{\mu}{2}(t-1)\lVert x_{t}-x^{*}\rVert _{2}^{2}-\frac{\mu}{2} t\lVert x_{t+1}-x^{*}\rVert _{2}^{2}+\frac{1}{2 \mu t}\lVert g\left(x_{t}\right)\rVert _{2}^{2}\right)$,
        - sum over $t=[1:T]$ we get $\sum_{t=1}^{T}\left[f\left(x_{t}\right)-f^{*}\right] \leq \sum_{t=1}^{T} \frac{1}{2 \mu t}\lVert g\left(x_{t}\right)\rVert _{2}^{2} \leq \frac{B^{2}}{2 \mu} \sum_{t=1}^{T} \frac{1}{t} \leq \frac{B^{2}}{2 \mu}(\ln (T)+1)$
- **Theorem F ($\mathcal{O}(1/T)$)** $f$ is $\mu$-strongly convex and $B$-Lipschitz, then subgradient descent of step size $\gamma_t = 2/\mu (t+1)$ gives $\max\{\min _{1 \leq t \leq T} f\left(\mathbf{x}_{t}\right), f\left(\hat{x}_{T}\right) \} - f^{*} \leq \frac{2 B^{2}}{\mu \cdot(T+1)}$ where $\hat{x}_{T}:=\sum_{t=1}^{T} t x_{t} / \sum_{t=1}^{T} t $.
    - **Proof**
        - By descent lemma $\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right) \leq \frac{1-\mu \gamma_{t}}{2 \gamma_{t}}\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-\frac{1}{2 \gamma_{t}}\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2}+\frac{\gamma_{t}}{2}\lVert \mathbf{g}_{t}\rVert _{2}^{2}$
        - Plug in $\gamma_t = 2/\mu (t+1)$ we get $\mathsf{RHS} = \frac{\mu(t-1)}{4}\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-\frac{\mu(t+1)}{4}\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2}+\frac{1}{\mu(t+1)}\lVert \mathbf{g}_{t}\rVert _{2}^{2}$
        - Times $t$ on each side and we get $t \left(f\left(\mathbf{x}_{t}\right)-f^{*}\right) \leq \frac{\mu t(t-1)}{4}\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-\frac{\mu(t+1)t}{4}\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2}+\frac{t}{\mu(t+1)}\lVert \mathbf{g}_{t}\rVert _{2}^{2}$
        - Since $\frac{t}{\mu(t+1)}\lVert \mathbf{g}_{t}\rVert _{2}^{2} \leq \frac{1}{\mu}B^{2}$, sum over $t=[1:T]$ and get $\sum_{t=1}^{T} t\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right) \leq-\frac{\mu T(T+1)}{4}\lVert \mathbf{x}_{T+1}-\mathbf{x}^{*}\rVert _{2}^{2}+\frac{T}{\mu} B^{2}$
        - else is straightforward.


## Lower Bound Complexity
We can show that, the above $\mathcal{O}(1/\sqrt{t})$ and $\mathcal{O}(1/t)$ can not be improved. The worst case function is given by the piecewise-linear function similar to $f(x)=\max _{1 \leq i \leq n} x_{i}$.
- **Theorem 6.20 (Nemirovski & Yudin 1979)**. For any $1 \leq t \leq n, x_{1} \in \mathbb{R}_{n}$,
    - (i) *$B$-Lipschit* there exists a $B$-Lipschitz continuous function $f$ and a convex set $X$ with diameter $R$, such that for any first-order method that generates $\mathbf{x}_{t} \in \mathbf{x}_{1}+\operatorname{span}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t-1}\right)$, where $\mathbf{g}_{i} \in \partial f\left(\mathbf{x}_{i}\right)$, we have $\min _{1 \leq s \leq t} f\left(\mathbf{x}_{s}\right)-f^{*} \geq \frac{B \cdot R}{4(1+\sqrt{t})}$.
    - (ii) *$B$-Lipschitz and $\mu$-convex* there exists a $\mu$-strongly convex, $B$-Lipschitz continuous function $f$ and a convex set $X$ with diameter $R$, for any ﬁrst-order method as described above, we always have $\min _{1 \leq s \leq t} f\left(x_{s}\right)-f^{*} \geq \frac{B^{2}}{8 \mu t}$.
    - **Proof**
        - The function we construct is $f(x)=C \cdot \max _{1 \leq i \leq t} x_{i}+\frac{\mu}{2}\Vert x\Vert _{2}^{2},$ and we restrict ourself within region $X=\left\{x \in \mathbb{R}^{n},\Vert x\Vert _{2} \leq R\right\}$.
            - The subgradient is $\partial f(x)=\mu x+C \cdot \operatorname{conv}\left\{e_{i}: i \text { such that } x_{i}=\max _{1 \leq j \leq t} x_{j}\right\}$.
            - The optimal solution is $x_{i}^{*}=\begin{cases}-\frac{C}{\mu t} & 1 \leq i \leq t \\0 & t<i \leq n\end{cases}$ and $f^{*}=-\frac{C^{2}}{2 \mu t}$, can be verified by $0 \in \partial f(x)$.
        - Since all subgradient method controls $\gamma_T$, but not how to choose subgradient, we can design a process that gives worst case. The update is design to have $g(x)=C \cdot e_{i}+\mu x$, only one pure direction of $x_{i}=\max _{1 \leq j \leq t} x_{j}$, but not its combination.
            - To simplify, we always choose the smallest one.
        - We claim that if we start from $x_{s=1} = 0$, then $x_{s} \in \operatorname{span}\left(e_{1}, \ldots, e_{s-1}\right)$, always one dimension less than $s$.
            - When $s=2$, all coordinates are equal and we choose $e_1$ to update, so $x_{s=2} \in \mathrm{span}\{e_1\}$.
            - By our update rule $g(x)=C \cdot e_{i}+\mu x$, the span can only increase at most one dimension at each update, so induction still holds.
        - This means for $x_s, 1\leq s \leq t$, at least one coordinate is unchanged as $0$. So $\max _{1 \leq i \leq t} x_{i}$ -> $f(x_s) \geq 0$.
            - This gives $\min _{1 \leq s \leq t} f\left(x_{s}\right)-f^{*} \geq \frac{C^{2}}{2 \mu t}$
        - Choosing $C=\frac{B \sqrt{t}}{1+\sqrt{t}}, \mu=\frac{2 B}{R(1+\sqrt{t})}$, we have $\Vert \partial f(x)\Vert _{2} \leq C+\mu\Vert x\Vert _{2} \leq C+\mu R=: M$, this is the $M$-Lipschitz case, we have $\min _{1 \leq s \leq t} f\left(x_{s}\right)-f^{*} \geq \frac{C^{2}}{2 \mu t}=\frac{B \cdot R}{2(1+\sqrt{t})}$.
        - Choosing $C=\frac{B}{2}, \mu=\frac{B}{R}$, we have $\Vert \partial f(x)\Vert _{2} \leq C+\mu R=:M$, this is $M$-Lispchitz and $\mu$-strongly convex case, we have $\min _{1 \leq s \leq t} f\left(x_{s}\right)-f^{*} \geq \frac{C^{2}}{2 \mu t}=\frac{B^{2}}{8 \mu t}$
    - *Comment* This theorem is so strange that number of updates is related to dimension.


## Mirror Descent

- **Key Idea** Update can be viewed as $x_{t+1}=\underset{x \in X}{\operatorname{argmin}}\left\{\frac{1}{2}\lVert x-x_{t}\rVert _{2}^{2}+\left\langle\gamma_{t} g\left(x_{t}\right), x\right\rangle\right\}$, how about we change $\Vert \cdot\Vert _2^2$ to something else.

### Bregman Divergence
- **Definition 6.22** Let $\omega(x): X \rightarrow \mathbb{R}$ be a function that is *strictly* convex, continuously differentiable on a closed convex set $X$. The *Bregman divergence* is defined as $V_{\omega}(x, y)=\omega(x)-\omega(y)-\nabla \omega(y)^{T}(x-y), \forall x, y \in X$.
    - Asymmetric $V_{\omega}(x, y) \neq V_{\omega}(y, x)$, not a valid distance, triangle inequality may not hold.
    - $\omega(\cdot)$ is called *distance-generating function*.
    - If $\omega(\cdot)$ is also $\sigma$-strongly convex w.r.t. norm $\Vert \cdot\Vert _{a}$, then $V_{\omega}(x,y) \geq \sigma \Vert x-y\Vert _{a}^2 /2$.
- **Example**
    - *Euclidean Distance* $\Omega=\mathbb{R}^{d}, \omega(\mathbf{x})=\frac{1}{2}\Vert \mathbf{x}\Vert _{2}^{2}$, $\omega$ is $1$ strongly convex w.r.t $\ell_2$ norm, 
        - $V_{\omega}(\mathbf{x}, \mathbf{y})=\frac{1}{2}\Vert \mathbf{x}-\mathbf{y}\Vert _{2}^{2}$.
    - *Mahalanobis distance* $\Omega=\mathbb{R}^{d}, \omega(\mathbf{x})=\frac{1}{2} \mathbf{x}^{T} Q \mathbf{x}$ where $Q \succeq I$, $\omega$ is $1$ strongly convex w.r.t $\ell_2$ norm, 
        - $V_{\omega}(\mathbf{x}, \mathbf{y})=\frac{1}{2}(\mathbf{x}-\mathbf{y})^{T} Q(\mathbf{x}-\mathbf{y})$.
    - *Kullback-Leibler divergence* $\Omega=\Delta_{d}, \omega(\mathbf{x})=\sum_{i=1}^{d} x_{i} \ln x_{i}$, $\nabla \omega (x) = \ln (x) - 1$, 
        - $V_{\omega}(x,y) = \sum_{i} x_i \ln x_i - y_i \ln y_i - (\ln(y_i) - 1)(x_i - y_i) = \sum_i x_i \ln \frac{x_i}{y_i} = \mathrm{KL}(x \Vert  y)$, since $\sum_i x_i = \sum_i y_i =1$.
        - The proof for $V_{\omega}(x,y) \geq \Vert x-y\Vert _1^2 /\ln 4$ is complicated, see [this](http://www.stat.yale.edu/~yw562/teaching/598/lec04.pdf).
- **Lemma 6.23 (Generalized Pythagorean Theorem, Exercise 46)** If $x^{*}$ is the Bregman projection of $x_0$ onto a convex set $C \subset X: x^{*} \operatorname{argmin}_{x \in C} V_{\omega}\left(x, x_{0}\right)$. Then $\forall y \in C$, $V_{\omega}\left(y, x_{0}\right) \geq V_{\omega}\left(y, x^{*}\right)+V_{\omega}\left(x^{*}, x_{0}\right)$.
    - **Proof**
        - $\nabla_x V_\omega(x,y) = \nabla \omega(x) - \nabla \omega(y)$, optimal condition means $\forall y \in C, (\nabla \omega(x^{*}) - \nabla \omega(x_0))^{\top}(y - x^{*}) \geq 0$.
        - equivalently, $- \nabla \omega(x_0)^{\top}(y - x_0 + x_0 -x^{*}) \geq -\nabla \omega(x^{*})^{\top}(y - x^{*})$
        - or $- \nabla \omega(x_0)^{\top}(y - x_0) \geq -\nabla \omega(x^{*})^{\top}(y - x^{*}) - \nabla \omega(x_0)^{\top}(x_0 -x^{*})$
        - then $\omega(x_0)  - \omega_(y)- \nabla \omega(x_0)^{\top}(y - x_0) \geq \omega(x_0)  - \omega_(x^{*}) +\omega(x^{*})- \omega_(y) -  \nabla \omega(x^{*})^{\top}(y - x^{*}) +- \nabla \omega(x_0)^{\top}(x_0 -x^{*})$, QED.

### Mirror Descent Algorithm
- *Prox-mapping* $\operatorname{Prox}_{x}(\xi)=\underset{u \in X}{\operatorname{argmin}}\left\{V_{\omega}(u, x)+\langle\xi, u\rangle\right\}$, and suppose $\omega$ is $1$-strongly convex on norm $\Vert \cdot\Vert _{a}$.
- **Algorithm** $x_{t+1}=\operatorname{Prox}_{x_{t}}\left(\gamma_{t} g\left(x_{t}\right)\right)$, where $g\left(x_{t}\right) \in \partial f\left(x_{t}\right)$.
- **Example 6.25** Under KL divergence, the prox-mapping becomes $\operatorname{Prox}_{x}\xi)=\left(\sum_{i=1}^{n} x_{i} e^{-\xi_{i}}\right)^{-1}\left[\begin{array}{c}x_{1} e^{-\xi_{1}} \\\ldots \\x_{n} e^{-\xi_{n}}\end{array}\right]$
    - **Proof** 
        - $V_{\omega}(u, x)+\langle\xi, u\rangle = \sum_{i} u_i \ln (u_i / x_i) + u_i \xi_i$, optimal condition under constraint is $0 = \ln(u_i/x_i) + 1 + \lambda + \xi_i$
        - So solution is $u_i = x_i e^{-\xi_i + \alpha}$ where $\sum_i u_i =1 \Rightarrow e^{-\alpha} = \sum_i x_i e^{-\xi_i}$ QED.

### $\mathcal{O}(1 / \sqrt{t})$ convergence for convex functions
- **Lemma 6.26 (Three point identity)** $\forall x,y,z \in \mathbf{dom}(\omega)$, $V_{\omega}(x, z)=V_{\omega}(x, y)+V_{\omega}(y, z)-\langle\nabla \omega(z)-\nabla \omega(y), x-y\rangle$
    - **Proof**
        - $V_{\omega}(x, y)+V_{\omega}(y, z)=\omega(x)-\omega(y)+\omega(y)-\omega(z)-\langle\nabla \omega(y), x-y\rangle-\langle\nabla \omega(z), y-z\rangle =V_{\omega}(x, z)+\langle\nabla \omega(z), x-z\rangle-\langle\nabla \omega(y), x-y\rangle-\langle\nabla \omega(z), y-z\rangle =V_{\omega}(x, z)+\langle\nabla \omega(z)-\nabla \omega(y), x-y\rangle$
    - When $\omega = \Vert \cdot\Vert _2^2$, this becomes law of cosines.
- **Lemma G (Descent Lemma)** $\gamma_{t}\left(f\left(\mathbf{x}_{t}\right)-f^{*}\right) \leq V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t}\right)-V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t+1}\right)+\frac{\gamma_{t}^{2}}{2}\lVert \mathbf{g}_{t}\rVert _{a*}^{2}$
    - **Proof**
        - By 3-point identiy, $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t})=V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1})+V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t})-\langle\nabla \omega(\mathbf{x}_{t})-\nabla \omega(\mathbf{x}_{t+1}), \mathbf{x}^{\star}-\mathbf{x}_{t+1}\rangle$, equivalently
            - $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1}) + \langle\nabla \omega(\mathbf{x}_{t})-\nabla \omega(\mathbf{x}_{t+1}), \mathbf{x}^{\star}-\mathbf{x}_{t+1}\rangle = V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t})$
        - by def of algo, the optimial condition is $\forall x\in X$, espectially $\mathbf{x}^{\star}$ $\langle \nabla_{\mathbf{x}_{t+1}}V_{\omega}(\mathbf{x}_{t+1},\mathbf{x}_{t}) + \gamma_t \mathbf{g}_{t},\mathbf{x}^{\star} - \mathbf{x}_{t+1}\rangle=  \langle \nabla \omega (\mathbf{x}_{t+1}) - \nabla \omega (\mathbf{x}_{t}) + \gamma_t \mathbf{g}_{t},\mathbf{x}^{\star} - \mathbf{x}_{t+1}\rangle \geq 0$, this means
            - $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1}) + \langle\gamma_t \mathbf{g}_{t}, \mathbf{x}^{\star}-\mathbf{x}_{t+1}\rangle \geq V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t})$
        - By convexity, $\langle\mathbf{g}_{t}, \mathbf{x}^{\star}-\mathbf{x}_{t}\rangle \leq f(\mathbf{x}^{\star}) - f(\mathbf{x}_{t})$, so $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1}) +\gamma_t ( f(\mathbf{x}^{\star}) - f(\mathbf{x}_{t}))  \geq V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) + \langle\gamma_t \mathbf{g}_{t}, \mathbf{x}_{t+1} - \mathbf{x}_{t}\rangle$
        - By *Young’s inequality*, $\langle a, b\rangle  \leq \Vert a\Vert _{a}\cdot \Vert b\Vert _{a*} \leq \Vert a\Vert _{a}^2/2 + \Vert b\Vert _{a*}^2/2$, therefore 
            - $\langle\gamma_t \mathbf{g}_{t}, \mathbf{x}_{t} - \mathbf{x}_{t+1}\rangle \leq \gamma_t^2 \Vert \mathbf{g}_t\Vert _{a*}^2/2 + \Vert \mathbf{x}_{t} - \mathbf{x}_{t+1}\Vert ^2_{a}/2$
        - By the assumption of $\omega$ is $1$-strongly convex, $V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) \geq  \Vert \mathbf{x}_{t} - \mathbf{x}_{t+1}\Vert ^2_{a}/2$
        - Combine the above two, $V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) + \langle\gamma_t \mathbf{g}_{t}, \mathbf{x}_{t+1} - \mathbf{x}_{t}\rangle \geq \Vert \mathbf{x}_{t} - \mathbf{x}_{t+1}\Vert ^2_{a}/2 - \gamma_t^2 \Vert \mathbf{g}_t\Vert _{a*}^2/2 - \Vert \mathbf{x}_{t} - \mathbf{x}_{t+1}\Vert ^2_{a}/2 = - \gamma_t^2 \Vert \mathbf{g}_t\Vert _{a*}^2/2$
        - Combine above, we arrive at the descent lemma.
           
- **Theorem 6.28** $f$ convex, then $\displaystyle \max\left\{\min _{1 \leq t \leq T} f\left(x_{t}\right), f\left(\hat{x}_{T}\right)\right\}-f^{*} \leq \frac{V_{\omega}\left(x^{*}, x_{1}\right)+\frac{1}{2} \sum_{t=1}^{T} \gamma_{t}^{2}\lVert g\left(x_{t}\right)\rVert _{*}^{2}}{\sum_{t=1}^{T} \gamma_{t}}$, where $\displaystyle \hat{x}_T = \frac{\sum_{t=1}^{T} \gamma_{t} x_{t}}{\sum_{t=1}^{T} \gamma_{t}}$.
    - **Proof** Similar to Theorem 6.17, sum over all $t\in[1:T]$ we get the result.
    - Convergence similar to subgradient case $\min _{1 \leq t \leq T} f\left(\mathbf{x}_{t}\right)-f^{*}=O\left(\frac{B R}{\sqrt{T}}\right)$, where $R=\sqrt{\max _{\mathbf{x} \in X} V_{\omega}\left(\mathbf{x}, \mathbf{x}_{1}\right)}$ and $B:=\sup _{\mathbf{x} \in X} \frac{\vert f(\mathbf{x})-f(\mathbf{y})\vert }{\Vert \mathbf{x}-\mathbf{y}\Vert }$.
- The constant may be different. **Example of Simplex** of $X=\left\{x \in \mathbb{R}^{d}: x_{i} \geq 0, \sum_{i=1}^{d} x_{i}=1\right\}$, assume $\Vert \mathbf{g}\Vert _{\infty} \leq 1$,
    - In normal case of $\omega(x)=\frac{1}{2}\Vert x\Vert _{2}^{2}$, $R^2 \leq 2= O(1)$, $B \sim \Vert g\Vert _2 = O(\sqrt{d})$, overall it's $O(\frac{\sqrt{d}}{\sqrt{T}})$.
    - If we choose $w(x)=\sum_{i=1}^{d} x_{i} \ln x_{i}$ and starting point to be $x_{1}=\operatorname{argmin}_{x \in X} \omega(x)$, then $\Omega \leq \max _{x \in X} \omega(x)-\min _{x \in X} \omega(x)=0-(-\ln d)=\ln (d)$, overall it's $O(\frac{\ln d}{\sqrt{T}})$
    - The ratio of efficiency is then $O\left(\frac{1}{\ln (d)} \cdot \frac{\max _{x \in X}\Vert g(x)\Vert _{2}}{\max _{x \in X}\Vert g(x)\Vert _{\infty}}\right)$, since $\Vert g(x)\Vert _{\infty} \leq\Vert g(x)\Vert _{2} \leq \sqrt{d}\Vert g(x)\Vert _{\infty}$, in worst case Mirror Descent is $O(\sqrt{d})$ faster than norm subgradient descent.

### Mirror Descent under Smoothness (Exercise 47)
- **Lemma H** If $f$ convex and gradient Lipschitz w.r.t. some norm, $\Vert \nabla f(x)-\nabla f(y)\Vert _{a*} \leq L\Vert x-y\Vert _{a}$, then setting $\gamma_{t}=1 / L$ will give $\min _{1 \leq t \leq T} f\left(x_{t}\right)-f^{*} \leq {L \cdot V_{\omega}\left(x^{*}, x_{1}\right)}/{T}$
    - **Proof**
        
        - By equivalence of smoothness and Lipschitz, $f(\mathbf{x}_{t+1}) \leq f(\mathbf{x}_{t})+\mathbf{g}_{t}^{\top}(\mathbf{x}_{t+1}-\mathbf{x}_{t})+\frac{L}{2}\Vert \mathbf{x}_{t}-\mathbf{x}_{t+1}\Vert _{a}^{2}$
            - Using the fact of $\omega$ is $1$-strongly convex, we have $\Vert \mathbf{x}_{t}-\mathbf{x}_{t+1}\Vert _{a}^{2}/2 \leq V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t})$
            - This makes $\frac{1}{\gamma_t} V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) + \langle \mathbf{g}_{t}, \mathbf{x}_{t+1}-\mathbf{x}_{t} \rangle \geq f(\mathbf{x}_{t+1}) - f(\mathbf{x}_{t})$
            - By the optimal condition of prox-mapping, we can show that $V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) + \langle \gamma_{t} \mathbf{g}_{t}, \mathbf{x}_{t+1} \rangle \leq \underset{=0}{V_{\omega}(\mathbf{x}_{t}, \mathbf{x}_{t})} + \langle \gamma_{t} \mathbf{g}_{t}, \mathbf{x}_{t} \rangle$
            - This means $f(\mathbf{x}_{t+1}) - f(\mathbf{x}_{t}) \leq 0$, always non-increasing.
        - In the third step of descent lemma, bu convexity we have $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1}) +\gamma_t ( f(\mathbf{x}^{\star}) - f(\mathbf{x}_{t}))  \geq V_{\omega}(\mathbf{x}_{t+1}, \mathbf{x}_{t}) + \langle\gamma_t \mathbf{g}_{t}, \mathbf{x}_{t+1} - \mathbf{x}_{t}\rangle$
            - take the above into it we get $V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{t+1}) + \gamma_t ( f(\mathbf{x}^{\star}) - f(\mathbf{x}_{t})) \geq \gamma_t(f(\mathbf{x}_{t+1}) - f(\mathbf{x}_{t}))$
        - take $\gamma_t = L^{-1}$ and sum over $t=[0:T-1]$, we have 
            - $\sum_{i=1}^{T} f(\mathbf{x}_{i}) - f(\mathbf{x}^{\star}) \leq L\left(V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{1}) - V_{\omega}(\mathbf{x}^{\star}, \mathbf{x}_{T})\right)$, QED
