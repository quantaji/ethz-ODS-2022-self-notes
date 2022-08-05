# Subgradient Methods

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
    2. $f(x)=\mid x\mid, \partial f(x)=\left\{\begin{array}{l}\operatorname{sgn}(x), x \neq 0 \\{[-1,1], x=0}\end{array}\right.$
    3. $f(x)=\left\{\begin{array}{l}-\sqrt{x}, x \geq 0 \\+\infty, o . w .\end{array} \quad, \partial f(0)=\emptyset ;\right.$
    4. $f(x)=\left\{\begin{array}{l}1, x=0 \\0, x>0 \\+\infty, o . w\end{array}, \partial f(0)=\emptyset\right.$
- **Lemma 6.5** Let $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$ be convex, $\operatorname{dom}(f)$ open, $B \in \mathbb{R}_{+}$. Then $\forall x\in\mathbf{dom}(d), \forall g\in\partial f(x), \Vert g\Vert_{a*}\leq B$ $\Leftrightarrow$ $\forall x,y\in\mathbf{dom}(d), \mid f(x)-f(y)\mid \leq B\Vert x-y\Vert_{a}$.
    - **Proof**
        - (->)
            - Subgradient -> $f(y) - f(x) \leq g_y^{\top}(y-x)$ and $f(x) - f(y) \leq g_x^{\top}(y-x)$
            - This means $\mid f(y) - f(x)\mid \leq \max\{ g_y^{\top}(y-x),  g_x^{\top}(y-x)\} \leq \max\{\Vert g_y\Vert_{a*},  \Vert g_x\Vert_{a*}\} \Vert y-x\Vert_{a}$
            - If $\Vert g\Vert_{a*} \leq B$ we arrive at Lipschitz
        - (<-)
            - $f(y) \geq f(x) + g_x^{\top}(y-x)$, by Lipschitz, we have $B\Vert x-y\Vert_{a} \geq g_x^{\top}(y-x), \forall y$. Choosing $y-x = \arg\max_{\Vert y-x\Vert=1} g_x^{\top}(y-x)$, we get maximum of $\Vert g_x\Vert_{a*}\leq B$. This is true forall $x$.

### Topological Properties of Subgradients
- **Lemma 6.6** Let $f(x)$ be a convex function and $x \in \operatorname{dom}(f)$. Then $\partial f(x)$ is convex and closed.
    - **Proof** $\partial f(x) = \cap_{y}\left\{g \in \mathbb{R}^{n}: f(y) \geq f(x)+g^{\top}(y-x)\right\}$, solution of $g$ for each $y$ is convex and closed linear separation.
- **Deﬁnition 6.7 (separation of convex sets)** Let $S$ and $T$ be two nonempty convex sets in $\mathbb{R}^n$. A hyperplane $H=\left\{x \in \mathbb{R}^{n}: a^{\top} x=b, a\neq 0\right\}$ is said to *separate* $S$ and $T$ if $S \cup T \not \subset H$ and $S \subset H^{-}=\left\{x \in \mathbb{R}^{n}: a^{\top} x \leq b\right\}$ and $T \subset H^{+}=\left\{x \in \mathbb{R}^{n}: a^{\top} x \geq b\right\}$.
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
        - $z\lambda x+(1-\lambda) y \in \operatorname{dom}(f)$ for $\lambda \in [0,1]$.
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
- **Conic combination**
- **affine composition**
- **supremum**
- **superposition**
