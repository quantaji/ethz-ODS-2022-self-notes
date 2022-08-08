# Chapter 7 Smoothing Techniques

## Convex Conjugate
- **Definition 7.1 (Convex conjugate)** For any function $f: \mathbb{R}^{n} \rightarrow \mathbb{R}$, its *convex conjugate* is $f^{\star}(y)=\sup _{x \in \operatorname{dom}(f)}\left\{x^{\top} y-f(x)\right\}$
- **Fenchel's inequality** $f^{\star}(y) \geq x^{\top} y-f(x), \forall x, y \; \Rightarrow \; x^{\top} y \leq f(x)+f^{*}(y), \forall x, y$.
    - which is a generalization if Yong's inequality $x^{\top} y \leq \frac{\Vert x\Vert ^{2}}{2}+\frac{\Vert y\Vert _{*}^{2}}{2}, \forall x, y$
- **Lemma 7.2 (Sec.12, [Roc97], no proof here)** If $f$ is convex, lower semi-continuous and proper, then $\left(f^{\star}\right)^{\star}=f$.
    - *Lower semi-continuous (l.s.c)* means $f(\mathbf{x}) \leq \liminf _{t \rightarrow \infty} f\left(\mathbf{x}_{t}\right)$ for $\mathbf{x}_{t} \rightarrow \mathbf{x}$. *Proper* means $f(x) \geq -\infty$.
- **Proposition 7.3** If $f$ is $\mu$-strongly convex then $f^{\star}$ is continuously differentiable and $\mu^{-1}$-Lipschitz smooth.
    - **Proof**
        - *Differentiable*
            - Since $f$ strongly-convex -> "$f$ + linear term" strictly convex -> $\sup _{x \in \operatorname{dom}(f)}\left\{x^{\top} y-f(x)\right\}$ have a unique solution for all $y\in \mathbb{R}^n$, $\mathbf{dom}(f^{\star})$ is then convex.
            - $f^{\star}$ is convex since $\sup _{x \in \operatorname{dom}(f)}\left\{x^{\top} (\lambda y_1 + (1-\lambda)y_2)-f(x)\right\} \geq  \lambda \sup _{x \in \operatorname{dom}(f)}\left\{x^{\top} y_1-f(x)\right\} + (1-\lambda) \sup _{x \in \operatorname{dom}(f)}\left\{x^{\top} y_2-f(x)\right\}$
                - then $\partial f^{\star}$ is non-empty.
            - Since $f$ strongly convex, its level set $\{x: f(x) \leq \alpha\}$ is closed, and its proper $f(x)>-\infty$. By lemma 7.2, $\left(f^{\star}\right)^{\star}=f$.
            - Consider $f^{\star}$'s subdifferential, $g\in \partial f^{\star}(z) \Leftrightarrow \forall y, f^{\star}(y) \geq f^{\star}(z) + g^{\top}(y-z) \Leftrightarrow \forall y, g^{\top} y - f^{\star}(y) \leq g^{\top} z - f^{\star}(z)$
                - this means $g^{\top} z - f^{\star}(z) = \left(f^{\star}\right)^{\star}(g) = f(g)$, equivalently, $f^{\star}(z) = g^{\top} z - f(g) \Leftrightarrow g \in \arg \max _{x \in \operatorname{dom} f}\left\{y^{\mathcal{T}} x-f(x)\right\}$.
                - Since $f$ strongly convex, $\arg\max$ is unique, this is a single-valued mapping.
            - By Lemma 26.1 in [[Roc97]](https://books.google.ch/books?hl=zh-CN&lr=&id=1TiOka9bx3sC&oi=fnd&pg=PR7&dq=R.+Tyrrell+Rockafellar.+Convex+Analysis.&ots=HtTHVAJTgb&sig=FV2qBFxDygeUI7ezTkfU8HsGUzU#v=onepage&q=R.%20Tyrrell%20Rockafellar.%20Convex%20Analysis.&f=false), *$\partial f$ is single values mapping iff $f$ essentially smooth. In that case $\partial f$ reduce to gradient mapping $\nabla f$*.
                - This means $f^{\star}$ is differentiable.
        - *$\mu^{-1}$-smoothness*
            - By similar argument, we know $y \in \partial f(g_y)$, by strong convexity $f(g_z) \geq f(g_y) + y^{\top}(g_z - g_y) + \mu \Vert g_y - g_z\Vert _{a}^2 /2$
                - and also $f(g_y) \geq f(g_z) + z^{\top}(g_y - g_z) + \mu \Vert g_y - g_z\Vert _{a}^2 /2$
            - combine two, we have $\mu \Vert g_y - g_z\Vert _{a}^2  \leq (g_z - g_y)^{\top} (z - y) \leq \Vert g_z - g_y\Vert _{a} \Vert z - y\Vert _{a*} \Rightarrow \Vert g_y - g_z\Vert _{a} \leq \mu^{-1} \Vert z - y\Vert _{a*}$
- **Lemma 7.4 (Exercise 49)** Let $f$ and $g$ be two proper, convex and semi-continuous functions, then
    - (a) $(f+g)^{\star}(x)=\inf _{y}\left\{f^{\star}(y)+g^{\star}(x-y)\right\}$
    - (b) $(\alpha f)^{\star}(x)=\alpha f^{\star}\left(\frac{x}{\alpha}\right)$ for $\alpha>0$.
    - **Proof**
        - (a)
            - $(f+g)^{\star}(x) = \sup _{y \in \operatorname{dom}(f\cap g)}\left\{x^{\top} y-f(y) - g(y)\right\} = \sup _{y \in \operatorname{dom}(f\cap g)} \inf_{z} \{ (x-z)^{\top} y- g(y) +  f^{\star}(z)\}$
            - Since function $h(y,z) := (x-z)^{\top} y- g(y) +  f^{\star}(z)$ is convex w.r.t $z$ and concave w.r.t. $y$, then by *von Neumann-Fan minimax theorem* $\min \max = \max\min$
                - then $(f+g)^{\star}(x) = \inf_{z} \sup _{y \in \operatorname{dom}(f\cap g)}  \{ (x-z)^{\top} y- g(y) +  f^{\star}(z)\} = \inf_{z}  \{ g^{\star}(x-z) +  f^{\star}(z)\}$
        - (b) $(\alpha f)^{\star}(x) = \sup _{y \in \operatorname{dom}(f\cap g)}\left\{x^{\top} y- \alpha f(y) \right\} = \alpha \sup _{y \in \operatorname{dom}(f\cap g)}\left\{\alpha^{-1} x^{\top} y-  f(y) \right\} = \alpha f^{\star} (x / \alpha)$.
- **Examples**
    - $f(\mathbf{x})=\frac{1}{2} \mathbf{x}^{\top} Q \mathbf{x}, Q \succ 0, f^{\star}(y)=\frac{1}{2} \mathbf{y}^{\top} Q^{-1} \mathbf{y}$.
    - $f(\mathbf{x})=\sum_{i=1}^{n} x_{i} \log \left(x_{i}\right), f^{\star}(\mathbf{y})=\sum_{i=1}^{n} e^{y_{i}-1}.$
    - $f(\mathbf{x})=-\sum_{i=1}^{n} \log \left(x_{i}\right), f^{\star}(\mathbf{y})=-\sum_{i=1}^{n} \log \left(-y_{i}\right)-n.$
    - $f(\mathbf{x})=\Vert \mathbf{x}\Vert , f^{\star}(\mathbf{y})= \begin{cases}0, & \Vert \mathbf{y}\Vert _{*} \leq 1 \\ +\infty, & \Vert \mathbf{y}\Vert _{*}>1.\end{cases}$

## Smoothing Techniques

### Common Smoothing techs
- **Nesterov's smoothing based on conjugate function** $f_{\mu}(x)=\max _{y \in \operatorname{dom}\left(f^{*}\right)}\left\{x^{\top} y-f^{\star}(y)-\mu \cdot d(y)\right\} = \left(f^{\star}+\mu d\right)^{\star}(x)$, where $d(y)$ is some proximity function, $d$ is (i) 1-strongly convex (ii) non-negative everywhere and (possibly iii) $\min d(y) = 0$.
    - By Prop 7.3, $f_\mu$ is *continuously differentiable* and $\mu^{-1}$-Lipschitz.
    - Examples of $d$: 
        - $d(\mathbf{y})=\frac{1}{2}\lVert \mathbf{y}-\mathbf{y}_{0}\rVert _{2}^{2}$
        - $d(\mathbf{y})=\frac{1}{2} \sum w_{i}\left(y_{i}-y_{0, i}\right)^{2} \text { with } w_{i} \geq 1$
        - $d(\mathbf{y}) = V_{\omega}(\mathbf{y}, \mathbf{y}_0)$.
- **Moreau-Yosida smoothing/regularization** $f_{\mu}(x)=\min _{y \in \operatorname{dom}(f)}\left\{f(y)+\frac{1}{2 \mu}\Vert x-y\Vert _{2}^{2}\right\}$.
    - when $d(y)=\frac{1}{2}\Vert y\Vert _{2}^{2}$, Nesterov's smoothing turns to Moreau-Yosida regularization (by Lemma 7.4).
- **Lasry-Lions regularization** double application of M-Y reg $f_{\mu,\delta}(x)=\max _{y} \min _{z}\left\{f(z)+\frac{1}{2 \mu}\Vert z-y\Vert _{2}^{2}-\frac{1}{2 \delta}\Vert y-x\Vert _{2}^{2}\right\}$
    - If $f$ 1-Lipschitz, then choosing $(\delta, \mu)=O(\epsilon)$ guarantees $\epsilon$ approximation error, and $f_{\mu, \delta}(\mathbf{x})$ is $O(1 / \epsilon)$-smooth.
    - Computation ineﬃciency due to solving nonconvex problems.
- **Randomized smoothing** $f_{\mu}(x)=\mathbb{E}_{Z} f(x+\mu Z)$ where $Z$ is isotopic Gaussian or uniform.
    - Choosing $\mu=O(\epsilon)$ guarantees  $\epsilon$ approximation error.
    - $f_{\mu}(\mathbf{x})$ is $O\left(\frac{\sqrt{d}}{\epsilon}\right)$-smooth, <u>dimension dependent</u>.
    - Stochastic gradient is computationally efficient through sampling.
- **Ben-Tal-Teboulle smoothing** based on recession function. Only applicable to particular class $f(x)=F\left(f_{1}(x), f_{2}(x), \ldots, f_{m}(x)\right)$, where $F(y)=\max _{x \in \operatorname{dom}(g)}\{g(x+y)-g(x)\}$ is the recession function of some func $g$. 
    - The smoothed function is $f_{\mu}(x)=\mu g\left(\frac{f_{1}(x)}{\mu}, \ldots, \frac{f_{m}(x)}{\mu}\right)$.

## Nesterov's Smoothing 
- **Proposition 7.10** $\forall \mu > 0$, let $D_{Y}^{2}=\max _{y \in Y} d(y)$, $f(x)-\mu D_{Y}^{2} \leq f_{\mu}(x) \leq f(x)$.
    - **Proof**
        - Left: $f_{\mu}(x)=\max _{y \in \operatorname{dom}\left(f^{*}\right)}\left\{x^{\top} y-f^{\star}(y)-\mu \cdot d(y)\right\} \geq \max \left\{x^{\top} y-f^{\star}(y)\right\} - \mu\max \left\{d(y)\right\} = f(x) - \mu D^2$
        - Right: $f_{\mu}(x)=\max _{y \in \operatorname{dom}\left(f^{*}\right)}\left\{x^{\top} y-f^{\star}(y)-\mu \cdot d(y)\right\} \leq f_{\mu}(x)=\max _{y \in \operatorname{dom}\left(f^{*}\right)}\left\{x^{\top} y-f^{\star}(y)\right\}=f(x)$
- **Remark** Tadeoff b.t.w. approximation error and optimization efficiency $f(\mathbf{x})-f^{*} \leq \underbrace{f(\mathbf{x})-f_{\mu}(\mathbf{x})}_{\text {approximation error }}+\underbrace{f_{\mu}(\mathbf{x})-\min _{x} f_{\mu}(\mathbf{x})}_{\text {optimization error }}$.
- Suppose we can access $\nabla f_{\mu}(x)$ and apply gradient methods to solve $\min _{x \in X} f_{\mu}(x)$
    - PGD gives $f\left(x_{t}\right)-f_{*} \leq O\left(\frac{R^{2}}{\mu t}+\mu D_{Y}^{2}\right)$
        - to achieve arror of $\epsilon$, we have to set $\mu=O\left(\frac{\epsilon}{D_{Y}^{2}}\right)$, and the number of iteration is $T = O\left(\frac{R^{2} D_{Y}^{2}}{\epsilon^{2}}\right)$
    - Accelerater GD gives $f\left(x_{t}\right)-f_{*} \leq O\left(\frac{R^{2}}{\mu t^{2}}+\mu D_{Y}^{2}\right)$
        - similarly to achieve error of $\epsilon$, we have to set $\mu=O\left(\frac{\epsilon}{D_{Y}^{2}}\right)$ and $T = O\left(\frac{R D_{Y}}{\epsilon}\right)$

### Example
- The function form is generalized as $f(x)=\max _{y \in Y}\{\langle A x+b, y\rangle-\phi(y)\}$, where $\phi$ convex and continuous, $Y$ convex and compact.
    - example like $f(x)=\max _{1 \leq i \leq m}\vert a_{i}^{\mathcal{T}} x-b_{i}\vert$ can be re-written as $f(x)=\max _{y \in Y} \sum_{i=1}^{m}\left(a_{i}^{\mathcal{T}} x-b_{i}\right) y_{i}$, where $Y:=\left\{y \in \mathbb{R}^{m}: \sum_{i=1}^{m}\vert y_{i}\vert  \leq 1\right\}$.
    - The smoothed function is $f_{\mu}(x)=\max _{y \in Y}\{\langle A x+b, y\rangle-\phi(y)-\mu \cdot d(y)\} = (\phi + \mu \cdot d)^{\star} (Ax+b)$
- As an example, $f(x) = \vert x \vert = \sup _{\vert y\vert  \leq 1} y x = \sup _{\substack{y_{1}, y_{2} \geq 0 \\ y_{1}+y_{2}=1}}\left(y_{1}-y_{2}\right) x$,
    - $d(y) = y^2/2$, then $f_{\mu}(x)=\sup _{\vert y\vert  \leq 1}\left\{y x-\frac{\mu}{2} y^{2}\right\}= \begin{cases}\frac{x^{2}}{2 \mu}, & \vert x\vert  \leq \mu \\ \vert x\vert -\frac{\mu}{2}, & \vert x\vert >\mu\end{cases}$.
    - $d(y)=1-\sqrt{1-y^{2}}$ (half circle), clearly 1-strongly convex. $f_{\mu}(x)=\sup _{\vert y\vert  \leq 1}\left\{y x-\mu\left(1-\sqrt{1-y^{2}}\right)\right\}=\sqrt{x^{2}+\mu^{2}}-\mu$.
        - This is a special case of Ben-Tal & Teboulle's smoothing $\vert x\vert =\sup _{y}\{g(x+\mu)-g(y)\}, g(y)=\sqrt{1+y^{2}}$ and $f_{\mu}(x)=\mu g\left({x}/{\mu}\right)=\sqrt{x^{2}+\mu^{2}}$
    - $d(y)=y_{1} \log y_{1}+y_{2} \log y_{2}+\log 2$, where $Y=\left\{\left(y_{1}, y_{2}\right): y_{1}, y_{2} \geq 0, y_{1}+y_{2}=1\right\}$, $f_\mu (x) =\mu \log \left(\frac{e^{-\frac{x}{\mu}}+e^{\frac{x}{\mu}}}{2}\right)$
        - This can be seen as $\vert x\vert =\max \{x,-x\}=\sup _{y}\{g(x+\mu)-g(y)\}, \quad g(y)=\log \left(e^{y_{1}}+e^{y_{2}}\right)$.

## Moreau-Yosida Regularization
- **Definition 7.11** the *proximal operator* of convex $f$ at a given point $x$ is defined as $\operatorname{prox}_{f}(x)=\underset{y}{\operatorname{argmin}}\left\{f(y)+\frac{1}{2}\Vert x-y\Vert ^{2}\right\}$.
- **Examples**
    - Indicator function $f(\mathbf{x})=\delta_{X}(\mathbf{x})= \begin{cases}0, & \mathbf{x} \in X \\ +\infty, & \mathbf{x} \notin X\end{cases}$, $\operatorname{prox}_{f}(\mathbf{x})=\Pi_{X}(\mathbf{x})$ is the projection.
    - $f(\mathbf{x})=\mu\Vert \mathbf{x}\Vert _{1}$, $\operatorname{prox}_{\mu\vert \cdot\vert }\left(x_{i}\right)= \begin{cases}x_{i}-\mu & \text { if } x_{i}>\mu \\ 0 & \text { if }\vert x_{i}\vert  \leq \mu \\ x_{i}+\mu & \text { if } x_{i}<-\mu\end{cases} = \operatorname{sign}(\mathbf{x}) \otimes[\vert \mathbf{x}\vert -\lambda]+$ *soft thresholding operator*.
    - 2-norm $f(\mathbf{x}):=\Vert \mathbf{x}\Vert _{2}$, $\operatorname{prox}_{\lambda f}(\mathbf{x})=\left[1-\lambda /\Vert \mathbf{x}\Vert _{2}\right]+\mathbf{x}$
    - Support function $f(\mathbf{x}):=\max _{\mathbf{y} \in \mathcal{C}} \mathbf{x}^{T} \mathbf{y}$, $\operatorname{prox}_{\lambda_{f}}(\mathbf{x})=\mathbf{x}-\lambda \pi_{\mathcal{C}}(\mathbf{x})$
    - Square norm $f(\mathbf{x}):=(1 / 2)\Vert \mathbf{x}\Vert _{2}^{2}$, $\operatorname{prox}_{\lambda f}(\mathbf{x})=(1 /(1+\lambda)) \mathbf{x}$
    - log function $f(\mathbf{x}):=-\log (x)$, $\operatorname{prox}_{\lambda f}(x)=\left(\left(x^{2}+4 \lambda\right)^{1 / 2}+x\right) / 2$
    - log det $f(\mathbf{x}):=-\log \operatorname{det}(\mathbf{X})$, $\operatorname{prox}_{\lambda f}(x)$ is the log function prox applied to the individual eigenvalues of $\mathbf{X}$.
- **Proposition 7.13** $f$ convex, then 
    - (a) *fixed point* $x^{\star}$ minimized $f(x)$ iff $x^{\star}=\operatorname{prox}_{f}\left(x^{\star}\right)$
        - **Proof** (one direction is obvious)
            - $x^{\star}=\operatorname{prox}_{f}\left(x^{\star}\right) \Leftrightarrow \forall y, f(x^{\star}) \leq f(y) + \frac{1}{2}\Vert x^{\star} - y\Vert ^2$
            - If $\exists y_0$ s.t. $f(y) < f(x^{\star})$, by convexity, $f(y_\lambda) := f(x^{\star} + \lambda(y-x^{\star})) \leq f(x^{\star}) - \lambda (f(x^{\star})-f(y)) \Rightarrow  f(x^{\star}) \geq f(x^{\star} + \lambda(y-x^{\star})) + \lambda (f(x^{\star})-f(y))$
            - Setting $\lambda \min \{1, \alpha 2(f(x^{\star})-f(y)) / \Vert x^{\star} - y\Vert ^2\}$, we get $f(x^{\star}) \geq f(y_\lambda) + \frac{\alpha}{2} \Vert x^{\star} - y_\lambda\Vert ^2$, setting $\alpha > 1$ and we get contradiction.
            - Another path is to prove $0 \in \partial f\left(x_{*}\right)+\left(x_{*}-x_{*}\right) \Longrightarrow 0 \in \partial f\left(x_{*}\right)$.
    - (b) *Non-expansive* $\lVert \operatorname{prox}_{f}(x)-\operatorname{prox}_{f}(y)\rVert  \leq\Vert x-y\Vert$
        - **Proof**
            - by optimial condition (suppose $\mathbf{dom}(f) = \mathbb{R}^n$), $\nabla f(\mathrm{prox}(x)) + \mathrm{prox}(x) - x = 0$ and $\nabla f(\mathrm{prox}(y)) + \mathrm{prox}(y) - y = 0$
            - By convexity $(\nabla f(\mathrm{prox}(y)) - \nabla f(\mathrm{prox}(x)))^{\top}(\mathrm{prox}(y) - \mathrm{prox}(x))\geq 0$
                - This means $(y - x)^{\top}(\mathrm{prox}(y) - \mathrm{prox}(x))\geq \Vert \mathrm{prox}(y) - \mathrm{prox}(x)\Vert _2^2 \Rightarrow$ QED
    - (c) *Moreau Decomposition* $x=\operatorname{prox}_{f}(x)+\operatorname{prox}_{f^{\star}}(x)$
        - **Proof**
            - By fact of $\partial f^{\star}(g) = \arg\max_{x} \{ g^{\top} x - f(x) \} = \{x\vert  \nabla f(x) = g  \}$
            - we already have $\nabla f(\mathrm{prox}(x)) + \mathrm{prox}(x) - x = 0$
                - which can be seen as $g=\nabla f(\mathrm{prox}(x))$, so $\nabla f^{\star} (g) = \mathrm{prox}(x))$
                - then this means $g + \nabla f^{\star} (g)  - x = 0$, which is the optimial condition for $\mathrm{prox}_{f^{\star}}(x)$, QED.

### Proximal Point Algorithm 
- **Proximal Point Algorithm (PPA)** $\mathbf{x}_{t+1}=\operatorname{prox}_{\gamma{t} f}\left(\mathbf{x}_{t}\right)$
    - This comes from $x_{t+1}=x_{t}-L^{-1} \nabla f_{\mu}\left(x_{t}\right)$, (Danskin's thm used here)
- **Example** $\min_x \Vert Ax + b\Vert _2^2 + \lambda \Vert x\Vert _1 = \min_x f_1 + f_2$, where $f_1$ differentiable while $f_2$ is not. Usually we approximate $x_{t+1} = \mathrm{prox}_{f_2} (x_t - \frac{1}{L_{f_1}} \nabla f_1(x_t))$.
- **Theorem 7.14** If $f$ convex, then PPA satisfies $f\left(x_{t}\right)-f^{\star} \leq \frac{\lVert x_{0}-x_{*}\rVert _{2}^{2}}{2 \sum_{\tau=0}^{t-1} \gamma_{\tau}}$
    - **Proof**
        - by the optimality condition of $x_{t+1}$, $f(x_{t+1) + \frac{1}{2\gamma_t}\Vert x_{t+1} - x_{t}\Vert _2^2 \leq f(x_t)$, this is something like sufficient decrease.
        - by first order optimality condition, $0 \in \partial f\left(x_{t+1}\right)+\frac{1}{\gamma_{t}}\left(x_{t+1}-x_{t}\right) \Longrightarrow \frac{x_{t}-x_{t+1}}{\gamma_{t}} \in \partial f\left(x_{t+1}\right)$,
            - so by convexity/subgradient $f\left(x_{\tau+1}\right)-f^{\star}  \leq \frac{1}{\gamma_{\tau}}\left(x_{\tau}-x_{\tau+1}\right)^{T}\left(x_{\tau+1}-x_{*}\right) = \frac{1}{\gamma_{t}}\left[\left(x_{\tau}-x_{*}\right)^{T}\left(x_{\tau+1}-x_{*}\right)-\lVert x_{\tau+1}-x_{*}\rVert ^{2}\right]$
            - By Young's inequality, $\left(x_{\tau}-x_{*}\right)^{T}\left(x_{\tau+1}-x_{*}\right) \leq \frac{1}{2}\left[\lVert x_{\tau}-x_{*}\rVert ^{2}+\lVert x_{\tau+1}-x_{*}\rVert ^{2}\right]$, plug this in and we get 
            - $\gamma_{\tau}\left(f\left(x_{\tau+1}\right)-f^{\star}\right) \leq \frac{1}{2}\left[\lVert x_{\tau}-x_{*}\rVert ^{2}-\lVert x_{\tau+1}-x_{*}\rVert ^{2}\right]$
            - Then we can happily do our summation over $t\in[0:T-1]$
        - Since we have monotonicity in $f$, this algorithm have last iteration convergence.
    - *Comment* prox step is more like a second order step.... more computation, but also more effective.
    - Algorithm converge a.l.a $\sum_{t} \gamma_{t} \rightarrow \infty$.
    - Larger $\gamma_t$ makes algo converge faster, but harder to solve each step.
