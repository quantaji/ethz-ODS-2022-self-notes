# Chapter 12 Stochastic Optimization
- **Formulation** $\min _{\mathbf{x} \in \mathbb{R}^{d}} F(\mathbf{x}):=\mathbb{E}_{\boldsymbol{\xi}}[f(\mathbf{x}, \boldsymbol{\xi})]$ or special case of $\min _{\mathbf{x} \in \mathbb{R}^{d}} F(\mathbf{x}):=\frac{1}{n} \sum_{i=1}^{n} f_{i}(\mathbf{x})$.
    - $\boldsymbol{\xi} \in \Xi \subset \mathbb{R}^{m}$ is a random vector with distribution $P$
    - usually $P$ unknown but can be sampled through data, $F$ and $\nabla F$ usually **hard to compute** even if $P$ given.
- **Algorithm** 
    1. $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma_{t} \nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)$ where $\boldsymbol{\xi}_{t} \stackrel{i i d}{\sim} P(\boldsymbol{\xi})$.
    2. $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma_{t} \nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)$ where $i_{t} \in [n]$ is uniformly sampled.
- We need $\gamma_{t} \to 0$ to achieve convergence.
- To reduce noise, we can use *mini-batch* $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma_{t} \cdot \frac{1}{b} \sum_{j \in J,\vert J\vert =b} \nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{j}\right)$ or *SGD with iterate averaging* $\overline{\mathbf{x}}_{t}=\frac{1}{t} \sum_{\tau=1}^{t} \mathbf{x}_{\tau}$.

### Convergence for convex functions
- SGD is a special case of *stochastic mirror descent*, $\mathbf{x}_{t+1}=\arg \min _{\mathbf{x} \in X}\left\{V_{\omega}\left(\mathbf{x}, \mathbf{x}_{t}\right)+\left\langle\gamma_{t} G\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right), \mathbf{x}\right\rangle\right\}$.
    - $\mathbb{E}[G(\mathbf{x}, \boldsymbol{\xi})] \in \partial F(\mathbf{x})$ and we assume $\mathbb{E}\left[\Vert G(\mathbf{x}, \boldsymbol{\xi})\Vert _{*}^{2}\right] \leq M^{2}$.
- **Theorem 12.4** $F$ convex, then SMD gives $\mathbb{E}\left[F\left(\hat{\mathbf{x}}_{T}\right)-F\left(\mathbf{x}_{*}\right)\right] \leq \frac{R^{2}+\frac{M^{2}}{2} \sum_{t=1}^{T} \gamma_{t}^{2}}{\sum_{t=1}^{T} \gamma_{t}}$, where $R^{2}=\max _{\mathbf{x} \in X} V_{\omega}\left(\mathbf{x}, \mathbf{x}_{1}\right)$ and $\hat{\mathbf{x}}_{T}=\frac{\sum_{t=1}^{T} \gamma_{t} \mathbf{x}_{t}}{\sum_{t=1}^{T} \gamma_{t}}$.
    - **Proof**
        - The descent lemma gives $\gamma_{t}\left(f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)-f(\mathbf{x}^{*}, \boldsymbol{\xi}_{t})\right) \leq V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t}\right)-V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t+1}\right)+\frac{\gamma_{t}^{2}}{2}\lVert G\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)\rVert _{a *}^{2}$
            - In the proof of this descent lemma, no information of $\mathbf{x}^{*}$ is used, so we can use this on $\mathbf{x}^{*} := \arg\min_{x} F(x)$.
        - Taking expectation w.r.t. $\boldsymbol{\xi}_t$ and condition on $\mathbf{x}_t$ we get ($\mathbf{x}_t$ is still stochastic.)
            - $\gamma_{t}\left(F\left(\mathbf{x}_{t}\right)-F(\mathbf{x}^{*})\right) \leq V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t}\right)- \mathbb{E}\left[ V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t+1}\right) \mid \mathbf{x}_{t} \right]+\frac{\gamma_{t}^{2}}{2}M^2$
            - <u>Or equivalently under 2-norm we can get with only convexity of $F$</u>,
                - $2 \gamma_{t}\left(F\left(\mathbf{x}_{t}\right)-F(\mathbf{x}^{*})\right) \leq \Vert  \mathbf{x}^{*}- \mathbf{x}_{t}\Vert _2^2 - \mathbb{E}\left[ \Vert  \mathbf{x}^{*}- \mathbf{x}_{t+1}\Vert ^2\mid \mathbf{x}_{t} \right] +\gamma_{t}^{2} \lVert G\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)\rVert _{2}^{2}$
        - Further taking expectation w.r.t $\mathbf{x}_t$ and condition on $\mathbf{x}_{t-1}$ and we get 
            - $\gamma_{t}\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F(\mathbf{x}^{*}) \mid \mathbf{x}_{t-1} \right] \leq \mathbb{E}\left[V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t}\right)-V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t+1}\right) \mid \mathbf{x}_{t-1} \right] +\frac{\gamma_{t}^{2}}{2}M^2$
            - add w.r.t. $t-1$ case and we get $\gamma_{t-1}\left(F\left(\mathbf{x}_{t-1}\right)-F(\mathbf{x}^{*})\right) + \gamma_{t}\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F(\mathbf{x}^{*}) \mid \mathbf{x}_{t-1} \right] \leq V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t-1}\right)- \mathbb{E}\left[ V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{t+1}\right) \mid \mathbf{x}_{t} \right]+(\gamma_{t-1}^{2} + \gamma_{t}^{2})M^2/2$
        - do this expectation and summation through $t\in[1:T]$ and we get 
            - $\sum_{t=1}^{T} \gamma_{t}\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F(\mathbf{x}^{*}) \mid \mathbf{x}_{1} \right] \leq V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{1}\right)- \mathbb{E}\left[ V_{\omega}\left(\mathbf{x}^{*}, \mathbf{x}_{T+1}\right) \mid \mathbf{x}_{t} \right]+\sum_{t=1}^{T} \gamma_{t}^{2} M^2/2$
            - then the conclusion is straightforward.
    - If we set $\gamma_{t} \equiv \frac{R}{M \sqrt{T}}$, then $\mathbb{E}\left[F\left(\hat{\mathbf{x}}_{T}\right)-F\left(\mathbf{x}^{*}\right)\right]=O\left(\frac{B R}{\sqrt{T}}\right)$, this means $O\left(1 / \epsilon^{2}\right)$ of sample complexity.

### Convergence for strongly convex functions  
- **Descent Lemma** assume $f$ is $\mu$-strongly convex and $\mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})\Vert _{2}^{2}\right] \leq B^{2}$, then  $\mathbb{E}\left[\lVert \mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2}\right] \leq\left(1-2 \mu \gamma_{t}\right) \mathbb{E}\left[\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}\right]+\gamma_{t}^{2} B^{2}$
    - **Proof**
        - $\lVert \mathbf{x}_{t+1}-\mathbf{x}_{*}\rVert _{2}^{2} \leq\lVert \mathbf{x}_{t}-\gamma_{t} \nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)-\mathbf{x}_{*}\rVert _{2}^{2} =\lVert \mathbf{x}_{t}-\mathbf{x}_{*}\rVert _{2}^{2}-2 \gamma_{t}\left\langle\nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right), \mathbf{x}_{t}-\mathbf{x}_{*}\right\rangle+\gamma_{t}^{2}\lVert \nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)\rVert _{2}^{2}$
        - Taking expectation over $\boldsymbol{\xi}_{t}$ and we get 
            - $\mathbb{E}\left[\lVert \mathbf{x}_{t+1}-\mathbf{x}_{*}\rVert _{2}^{2}\mid \mathbf{x}_{t}\right] \leq \lVert \mathbf{x}_{t}-\mathbf{x}_{*}\rVert _{2}^{2} - 2 \gamma_{t}\left\langle\nabla F\left(\mathbf{x}_{t}\right), \mathbf{x}_{t}-\mathbf{x}_{*}\right\rangle+\gamma_{t}^{2}B^{2}$
        - Strong convexity of $F$ means coercivity, $\left\langle\nabla F\left(\mathbf{x}_{t}\right), \mathbf{x}_{t}-\mathbf{x}_{*}\right\rangle = \left\langle\nabla F\left(\mathbf{x}_{t}\right) - F\left(\mathbf{x}^{\star}\right), \mathbf{x}_{t}-\mathbf{x}_{*}\right\rangle \geq \mu\lVert \mathbf{x}_{t}-\mathbf{x}_{*}\rVert _{2}^{2}$
            - plug this in and we get the result.
- **Theorem 12.3** Assume $F(x)$ is $\mu$-strongly convex, and $\exists B> 0$, s.t. $\forall \mathbf{x}\in X,  \mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})\Vert _{2}^{2}\right] \leq B^{2}$, then SGD with $\gamma_{t}=\gamma / t$ at iteration $t$ where $\gamma>1 / 2 \mu$ satisfies $\mathbb{E}\left[\lVert \mathbf{x}_{t}-\mathbf{x}_{*}\rVert _{2}^{2}\right] \leq \frac{C(\gamma)}{t}$ where $C(\gamma)=\max \left\{\frac{\gamma^{2} B^{2}}{2 \mu \gamma-1},\lVert \mathbf{x}_{1}-\mathbf{x}_{*}\rVert _{2}^{2}\right\}$.
    - **Proof**
        - By decsetn lemma than take expectation over all $t$, denote $\varepsilon_{t} := \mathbb{E}\left[\lVert \mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}\mid \mathbf{x}_1\right]$, we have 
            - $\varepsilon_{t+1} \leq (1 - 2\mu \gamma /t)\varepsilon _{t} + \gamma^2 B^2t^2$.
        - We use induction and assume $\exists C(\gamma)$ s.t. $\forall \tau \leq t$, $\varepsilon_{\tau} \leq C(\gamma)/\tau$.
        - To make $\varepsilon_{t+1} \leq C(\gamma)/(t+1)$, we need (denote $b:= 2\mu\gamma > 1$ and $a:= B^2\gamma^2$)
            - $\epsilon_{t+1} \leq \frac{C(\gamma)}{t}\left(1 - \frac{b}{t}\right) + \frac{a}{t^2} = \frac{C(\gamma)}{t+1}\left(1+\frac{1}{t}\right)\left(1-\frac{b}{t}\right)+ \frac{a}{t^2} = \frac{C(\gamma)}{t+1} - \frac{C(\gamma)}{t(t+1)}\left(b-1+\frac{b}{t}\right)+\frac{a}{t^{2}}$
            - We need the residual term $\frac{C(\gamma)}{t(t+1)}\left(b-1+\frac{b}{t}\right) - \frac{a}{t^{2}} > 0$ 
                - equivalently $C(\gamma) \geq \frac{1}{b-1+\frac{b}{t}}(1 + \frac{1}{t})a = \frac{a}{b-1}\frac{1+t}{b/(b-1) + t}$.
                - Since $b \geq 1$, $b/(b-1) >1$, $\frac{1+t}{b/(b-1) + t}$ increase w.r.t $t$, so $C(\gamma) \geq \max_{t}  \frac{1}{b-1+\frac{b}{t}}(1 + \frac{1}{t})a = \frac{a}{b-1}\frac{1+\infty}{b/(b-1) + \infty} = \frac{a}{b-1} = \frac{\gamma^{2} B^{2}}{2 \mu \gamma-1}$
        - To ensure indunction succeed, we also need $C(\gamma)\geq \varepsilon_{1} = \lVert \mathbf{x}_{1}-\mathbf{x}_{*}\rVert _{2}^{2}$.
    - sample compexity is $O(1 / \epsilon)$.
    - If $F$ also $L$-smooth, we have $\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right]=O\left(\frac{L \cdot C(\gamma)}{t}\right)$.
- **Example** SGD over $\min _{x} F(x):=\frac{1}{2} \mathbb{E}_{\xi \sim N(0,1)}\left[(x-\xi)^{2}\right]$ with $\gamma_t = 1/t$ gives $\mathbb{E}\left[\lvert x_{t+1}-x^{*}\rvert ^{2}\right]=\frac{1}{t}$, since $t x_{t+1} = (t-1) x_{t} + \xi_t$.

- **Lemma (Boundedness of Stochastic Gradients, Ex5.1 5.2)** $F(x)=\mathbb{E}[f(x, \xi)]$ and $f$ is convex and $L$-smooth, define $\mathbf{x}^{*}=\operatorname{argmin}_{\mathbf{x}} F(\mathbf{x})$, then 
    - $\mathbb{E}\left[\lVert\nabla f(x, \xi)-\nabla f\left(x^{*}, \xi\right)\rVert _{2}^{2}\right] \leq 2 L\left[F(x)-F\left(x^{*}\right)\right]$ and $\mathbb{E}\left[\Vert \nabla f(x, \xi)\Vert _{2}^{2}\right] \leq 4 L\left[F(x)-F\left(x^{*}\right)\right]+2 \mathbb{E}\left[\lVert\nabla f\left(x^{*}, \xi\right)\rVert _{2}^{2}\right]$
    - **Proof**
        - first
            - Define $f_y(x) := f(x,\xi) - \nabla f(y,\xi)^{\top}(x-y)$, then $x=y$ is the minima for $f_y$, 
            - by Lemma B in chap3, the quadratic bound for smooth function 
                - $f(\mathbf{x}^{\star},\xi) - \nabla f(y,\xi)^{\top}(\mathbf{x}^{\star}-y) - f(y,\xi) = f_y(\mathbf{x}^{\star}) - f_y(y) \geq \frac{1}{2L} \Vert \nabla f_y(\mathbf{x}^{\star})\Vert _2^2 = \frac{1}{2L} \Vert \nabla f(\mathbf{x}^{\star})- \nabla f(\mathbf{x}_y)\Vert _2^2$. 
            - Taking expectation over $\xi$ and $\nabla F(\mathbf{x}^{\star}) = 0$, we get 
                - $F(\mathbf{x}^{\star})- F(y) \geq  \frac{1}{2L} \mathbb{E} \Vert \nabla f(\mathbf{x}^{\star})- \nabla f(\mathbf{x}_y)\Vert _2^2$
        - Second
            - by $\Vert \mathbf{a}+\mathbf{b}\Vert _{2}^{2} \leq 2\Vert \mathbf{a}\Vert _{2}^{2}+2\Vert \mathbf{b}\Vert _{2}^{2}$
                - so $\Vert \nabla f(x, \xi)\Vert _{2}^{2} \leq 2\lVert\nabla f(x, \xi)-\nabla f\left(x^{*}, \xi\right)\rVert _{2}^{2}+2\lVert\nabla f\left(x^{*}, \xi\right)\rVert _{2}^{2} .$
- **Lemma of my own (might be useful in exam)** $f$ convex and $L$-smooth, then $\forall r, f(x) - f(y) \geq \nabla f_y^{\top}(x-y) - (\nabla f_x - \nabla f_y)^{\top} r - L\Vert r\Vert _2^2/ 2$
    - **Proof**
        - By smoothness, $f(x+r) \leq f(x) + \nabla f_x^{\top} r + L\Vert r\Vert _2^2/ 2$
        - By convexity $f(x+r) \geq f(y) + \nabla f_y^{\top} (x+r-y)$
        - combine them together and we get the result.

### Convergence of SGD under constant stepsize
- **Theorem 12.5** Assume $F$ is $\mu$-strongly convex and $L$-smooth, and also $\mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})\Vert _{2}^{2}\right] \leq \sigma^{2}+c\Vert \nabla F(\mathbf{x})\Vert _{2}^{2}$, then SGD with constant step size $\gamma_{t} \equiv \gamma \leq \frac{1}{L c}$ gives $\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}_{*}\right)\right] \leq \frac{\gamma L \sigma^{2}}{2 \mu}+(1-\gamma \mu)^{t-1}\left[F\left(\mathbf{x}_{1}\right)-F\left(\mathbf{x}_{*}\right)\right]$.
    - **Proof**
        - By Smoothness of $F$, we have $F(\mathbf{x}_{t+1, \xi_{t}}) - F(\mathbf{x}_{t}) \leq - \gamma \langle \nabla F(\mathbf{x}_{t}), \nabla f(\mathbf{x}_{t}, \xi_t) \rangle + \frac{L \gamma ^2}{2} \Vert \nabla f(\mathbf{x}_{t}, \xi_{t})\Vert _2^2$
        - Taking expectation over $\xi_{t}$, we get $\mathbb{E}[F(\mathbf{x}_{t+1}) \vert  \mathbf{x}_{t}]  - F(\mathbf{x}_{t}) \leq - \gamma \Vert  \nabla F(\mathbf{x}_{t})\Vert ^2 + \frac{L \gamma ^2}{2}\left( \sigma^2 + c \Vert  \nabla F(\mathbf{x}_{t})\Vert ^2\right)$
        - By strong convexity we have $\Vert \nabla F(\mathbf{x}_{t}) - \nabla F(\mathbf{x}^{\star})\Vert  \geq 2\mu (F(\mathbf{x}_{t}) - F(\mathbf{x}^{\star}))$, take half of this term into above and we get
        - $\mathbb{E}[F(\mathbf{x}_{t+1}) - F(\mathbf{x}^{\star}) \vert  \mathbf{x}_{t}]  \leq (1 - \gamma \mu)\left( F(\mathbf{x}_{t}) - F(\mathbf{x}^{\star})\right)  + \frac{L \gamma ^2 \sigma^2}{2} - (1 - L\gamma c)\frac{\gamma}{2} \Vert  \nabla F(\mathbf{x}_{t})\Vert ^2$
        - Since $\gamma \geq 1/cL$, we get $\mathbb{E}[F(\mathbf{x}_{t+1}) - F(\mathbf{x}^{\star}) ]  \leq (1 - \gamma \mu)\mathbb{E}[F(\mathbf{x}_{t}) - F(\mathbf{x}^{\star}) ]   + \frac{L \gamma ^2 \sigma^2}{2}$
            - $(1- \gamma \mu)^{-T+1}\mathbb{E}[F(\mathbf{x}_{t+1}) - F(\mathbf{x}^{\star}) ]\leq \mathbb{E}[F(\mathbf{x}_{t}) - F(\mathbf{x}^{\star}) ]  +\sum_{t=1}^{T-1}(1- \gamma \mu)^{-t} \frac{L \gamma ^2 \sigma^2}{2}$, else is easy.
    - If we start from $\Vert \mathbf{x}_{t} - \mathbf{x}^{\star}\Vert _2^2$ we can also get similar conclusion on it.
    - This means constant stepsize converges linearly to some neighborhood of the optimal solution.
    - If variance is bounded $\mathbb{E}[\Vert  \nabla f(\mathbf{x}, \boldsymbol{\xi})-\nabla F(\mathbf{x}) \Vert _{2}^{2}] \leq \sigma^{2}$, then the condition natually holds with $c=1$.
    - When $\mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})\Vert _{2}^{2}\right] \leq c\Vert \nabla F(\mathbf{x})\Vert _{2}^{2}$, the case is called *strong growth condition* with constant $c$. SGD then converge to global optimal.
        - When $F(\mathbf{x})=\frac{1}{n} \sum_{i=1}^{n} f_{i}(\mathbf{x})$, strong growth means *imterpolation*, $\nabla f_{i}\left(\mathbf{x}^{*}\right)=0, \forall i$.
        - Example: linear regression or over-parametrized neural network in the realizable case.
- **Lemma (PL -> strong growth)** If $f$ is $L$-smooth and $F$ is $\mu$-PL, then we have strong growth condition (I cannot prove assuming $F$ is smooth). 
    - **Proof** 
        - By Lemma B in chp3 $f(\mathbf{x},\xi) - f(\mathbf{x}^{\star},\xi)\geq  \frac{1}{2L} \Vert \nabla f(\mathbf{x},\xi)\Vert _2^2 + \mathrm{Linear\;Term}$, taking expectation over $\xi$
            - $\mathbb{E} \Vert \nabla f(\mathbf{x},\xi)\Vert _2^2 \leq 2L (F(\mathbf{x}) - F(\mathbf{x}^{\star} ))$, then by PL of $F$, $F(\mathbf{x}) - F(\mathbf{x}^{\star}) \leq \frac{1}{2\mu} \Vert \nabla F(\mathbf{x})\Vert _2^2$ we get strong growth condition with $c=L/\mu$.
- **Definition (Weak Growth)** $F=\mathbb{E} f$ is $L$-smooth and have a minima $\mathbf{x}^{\star}$, stochastic gradient satisfies the weak growth condition with constant $c$ if $\mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})\Vert _{2}^{2}\right] \leq 2 c L\left[F(\mathbf{x})-F\left(\mathbf{x}_{*}\right)\right]$.
- **Lemma (Ex 75.1)** If $F$ convex, strong growth -> weak growth.
    - **Proof** By smoothness $\Vert \nabla F(\mathbf{x})\Vert _{2}^{2} \leq 2L [F(\mathbf{x}) - F(\mathbf{x}^{\star})]$ (*PS: I don't see why assume convex*)
- **Lemma (Ex 75.2)** If $F$ $\mu$-strong convex, weak growth -> strong growth.
    - **Proof** By strong convexity $\Vert \nabla F(\mathbf{x})\Vert _{2}^{2} \geq 2\mu [F(\mathbf{x}) - F(\mathbf{x}^{\star})]$

### Convergence for nonconvex functions (handout 11)
- **Theroem 12.8** If $\mathbf{dom}(F) = X = \mathbb{R}^{n}$, $F$ is $L$-smooth and $\mathbb{E}\left[\Vert \nabla f(\mathbf{x}, \boldsymbol{\xi})-\nabla F(\mathbf{x})\Vert _{2}^{2}\right] \leq \sigma^{2}$, then stepsize of $\gamma_t = \min \left\{\frac{1}{L}, \frac{\gamma}{\sigma \sqrt{T}}\right\}$ gives $\mathbb{E}\left[\lVert \nabla F\left(\hat{\mathbf{x}}_{T}\right)\rVert ^{2}\right] \leq \frac{\sigma}{\sqrt{T}}\left(\frac{2\left(F\left(\mathbf{x}_{1}\right)-F\left(\mathbf{x}_{*}\right)\right)}{\gamma}+L \gamma\right)$, where $\hat{\mathbf{x}}_{T}$ is selected uniformly at random from $\left\{\mathbf{x}_{1}, \ldots, \mathbf{x}_{T}\right\}$.
    - **Proof**
        - similar to privous proof, $\mathbb{E}[F(\mathbf{x}_{t+1}) \vert  \mathbf{x}_{t}]  - F(\mathbf{x}_{t}) \leq - \gamma \Vert  \nabla F(\mathbf{x}_{t})\Vert ^2 + \frac{L \gamma ^2}{2} \mathbb{E} \Vert \nabla f(\mathbf{x}_{t}, \xi_t)\Vert _2^2$.
        - By our assumption, we have $\mathbb{E}\left[F\left(\mathbf{x}_{t+1}\right)-F\left(\mathbf{x}_{t}\right)\right] \leq-\frac{\gamma_{t}}{2} \mathbb{E}\lVert \nabla F\left(\mathbf{x}_{t}\right)\rVert ^{2}+\frac{L \sigma^{2} \gamma_{t}^{2}}{2}$, sum over $t\in[1:T]$ and we get the result.
    - This means in non-convex setting, finding $\epsilon$-stationary point $\mathbb{E}[\Vert \nabla F(\overline{\mathbf{x}})\Vert ] \leq \epsilon$ requirse at most $O\left(1 / \epsilon^{4}\right)$ iteration/data point.

### Lower Bound
- Sample complexity of $O\left(1 / \epsilon^{2}\right)$ and $O(1 / \epsilon)$ for convex / strongly convex cannot be improved.
- **Definition (Stochastic Oracle)** Given input $\mathbf{x}$, stochastic oracle returns $G(\mathbf{x}, \boldsymbol{\xi})$ such that $\mathbb{E}[G(\mathbf{x}, \boldsymbol{\xi})] \in \partial F(\mathbf{x})$ and $\mathbb{E}\left[\Vert G(\mathbf{x}, \boldsymbol{\xi})\Vert _{p}^{2}\right] \leq M^{2}$, $M > 0$ and $p\in[1,\infty]$.
- **Theorem (Agarwal et al., 2012)** Let $X=B_{\infty}(r)$ be a $\ell_\infty$ ball in $\mathbb{R}^{d}$.
    1. $\exists c_{0}>0$, convex function $ f$ with $\vert f(x)-f(y)\vert <M\Vert x-y\Vert _{\infty}$, for any algorithm making $T$ stochastic oracles with $1\leq p \leq 2$, 
        - $\mathbb{E}\left[f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{*}\right)\right] \geq \min \left\{c_{0} M r \sqrt{\frac{d}{T}}, \frac{M r}{144}\right\}$
    2. $\exists c_{1}, c_{2}>0$, $\mu$-strongly convex function $f$, for any algorithm making $T$ stochastic oracles with $p=1$, 
        - $\mathbb{E}\left[f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{*}\right)\right] \geq \min \left\{c_{1} \frac{M^{2}}{\mu^{2} T}, c_{2} M r \sqrt{\frac{d}{T}}, \frac{M^{2}}{1152 \mu^{2} d}, \frac{M r}{144}\right\}$.

## Adaptive Stochastic Gradient Methods
- **Generic Adaptive Scheme** 
    - $\mathbf{g}_{t}=\nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right)$
    - $\mathbf{m}_{t}=\phi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)$
    - $V_{t}=\psi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)$
    - $\hat{\mathbf{x}}_{t}=\mathbf{x}_{t}-\alpha_{t} V_{t}^{-1 / 2} \mathbf{m}_{t}$
    - $\mathbf{x}_{t+1}=\underset{\mathbf{x} \in X}{\operatorname{argmin}}\left\{\left(\mathbf{x}-\hat{\mathbf{x}}_{t}\right)^{T} V_{t}^{1 / 2}\left(\mathbf{x}-\hat{\mathbf{x}}_{t}\right)\right\}$
- **SGD** $\phi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\mathbf{g}_{t}, \quad \psi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\mathbb{I}$
- **AdaGrad** $\phi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\mathbf{g}_{t}, \quad \psi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)={\operatorname{diag}\left(\sum_{\tau=1}^{t} \mathbf{g}_{\tau}^{2}\right)}/{t}$
    - can be viewed as Mirror descent with Bregman Divergence of $\omega_{t}(\mathbf{x})=\frac{1}{2} \mathbf{x}_{t} H_{t} \mathbf{x}$, where $H_{t}=\epsilon \mathbf{I}+\left[\sum_{t=1}^{t} \mathbf{g}_{t} \mathbf{g}_{t}^{T}\right]^{\frac{1}{2}}$.
- **RMSProp** $\phi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\mathbf{g}_{t}, \quad \psi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\left(1-\beta_{2}\right) \operatorname{diag}\left(\sum^{t} \beta_{2}^{t-\tau} \mathbf{g}_{\tau}^{2}\right)$
    - momentum on gradient norm term to slow down the decay of learning rate.
- **Adam** $\phi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\left(1-\beta_{1}\right) \sum_{\tau=1}^{t} \beta_{1}^{t-\tau} \mathbf{g}_{\tau}, \quad \psi_{t}\left(\mathbf{g}_{1}, \ldots, \mathbf{g}_{t}\right)=\left(1-\beta_{2}\right) \operatorname{diag}\left(\sum^{t} \beta_{2}^{t-\tau} \mathbf{g}_{\tau}^{2}\right)$,
    - equivalently $\mathbf{m}_{t}=\beta_{1} \mathbf{m}_{t-1}+\left(1-\beta_{1}\right) \mathbf{g}_{t}, V_{t}=\beta_{2} V_{t-1}+\left(1-\beta_{2}\right) \operatorname{diag}\left(\mathbf{g}_{t}^{2}\right)$.
    - momentum on both gradient and gradient norm term.
- Adaptive Methods
    - Less sensitive to parameter tuning and adapt to sparse gradients.
    - Out perform SGD for NLP tasks, training GANs, deep RL, etc., but are less effective in CV tasks.
    - Tend to overfit and generalize worse than their non-adaptive counterparts.
    - Often display faster initial progress on the training set, but their performance quickly plateaus on the testing set.
- What we know in theory
    - SGD with momentum has no acceleration even for some convex quadratic functions.
    - For convex problems, Adagrad does converge, but RMSProp and Adam may not when $\beta_{1}<\sqrt{\beta_{2}}$.

- Example of non-convergence of Adam
    - $X=[-1,1], \;f(x, \xi)=\left\{\begin{array}{ll}C x, & \text { if } \xi=1 \\-x, & \text { if } \xi=0\end{array},\; P(\xi=1)=p=\frac{1+\delta}{C+1}\right.$
    - $F(x)=\mathbb{E}[f(x, \xi)]=\delta x$ and $x^{*}=-1$.
    - update rule gives $x_{t+1}=x_{t}-\gamma_{0} \Delta_{t}$ with $\Delta_{t}=\frac{\alpha m_{t}+(1-\alpha) g_{t}}{\sqrt{\beta v_{t}+(1-\beta) g_{t}^{2}}}$
    - For $C$ large enough, one can show that $\mathbb{E}\left[\Delta_{t}\right] \leq 0$.
- A fix: *AMSGrad*, can prove convergence for many convex case.
    - changes: $v_{t}=\beta_{2} v_{t-1}+\left(1-\beta_{2}\right) g_{t}^{2}, \hat{v}_{t}=\max \left(\hat{v}_{t-1}, v_{t}\right)$ and $\hat{V}_{t}=\operatorname{diag}\left(\hat{v}_{t}\right)$.
    - Idea: if $g_t \ll m_t$ then $v_t < v_{t+1}$, this might increase step size.
## Variance reduce methods
- **Idea** $\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right] \leq \frac{\gamma L \sigma^{2}}{2 \mu}+(1-\mu \gamma)^{t-1}\left(F\left(\mathbf{x}_{1}\right)-F\left(\mathbf{x}^{*}\right)\right)$, if $\sigma^2$ can be reduced -> better upper bound.
- **Mini-Batching** $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma_{t}\frac{1}{b} \sum_{i=1}^{b} \nabla f\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t, i}\right)$, $\sigma^2 \leftarrow \sigma^2/b$ but at cost of computation. sample complexity remains the same.
- **Importance sampling** since $\xi\sim P$, we can change to another distri $Q$ by $G\left(\mathbf{x}_{t}, \boldsymbol{\xi}_{t}\right) \Longrightarrow G\left(\mathbf{x}_{t}, \boldsymbol{\eta}_{t}\right) \frac{P\left(\boldsymbol{\eta}_{t}\right)}{Q\left(\boldsymbol{\eta}_{t}\right)}$, and variance may be smaller.
- **Momentum** $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma_{t} \hat{\mathbf{m}}_{t}$ where $\hat{\mathbf{m}}_{t}=c \cdot \sum_{\tau=1}^{t} \alpha^{t-\tau} \nabla f_{i_{\tau}}\left(\mathbf{x}_{\tau}\right)$
- **Key Idea of Modern Variance Reduction**
    - we want to estimate $\theta=\mathbb{E}[X]$, but estimator is $\hat{\Theta}:=X-Y$, where $\mathbb{E}[Y]=0$.
    - $\mathbb{V}[X-Y] < \mathbb{V}[X]$ if $\mathrm{Cov}(X,Y) > 0$ 
- **Point Estimator** $\hat{\Theta}_{\alpha}=\alpha(X-Y)+\mathbb{E}[Y]$, $\mathbb{E}\left[\hat{\Theta}_{\alpha}\right]=\alpha \mathbb{E}[X]+(1-\alpha) \mathbb{E}[Y]$ and $\mathbb{V}\left[\hat{\Theta}_{\alpha}\right]=\alpha^{2}(\mathbb{V}[X]+\mathbb{V}[Y]-2 \operatorname{Cov}[X, Y])$.
    - $\alpha = 0 \to 1$, highly bias and no variance $\to$ unbiased but larges variance.
    - If $\operatorname{Cov}[X, Y]$ is sufficiently large, then $\operatorname{Var}\left[\hat{\Theta}_{\alpha}\right]<\operatorname{Var}[X]$
    - *Idea* $\mathbf{g}_{t}:=\alpha\left(\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-Y\right)+\mathbb{E}[Y]$ s.t. $\mathbb{E}\left[\lVert\mathbf{g}_{t}-\nabla F\left(\mathbf{x}_{t}\right)\rVert ^{2}\right] \rightarrow 0$, as $t \rightarrow \infty$.
- **Choice 1** $Y=\nabla f_{i_{t}}\left(\mathbf{x}^{\star}\right)$, $\mathbb{E}[Y]=0$, unrealistic but concptually useful.
- **Choice 2** $Y=\nabla f_{i_{t}}\left(\overline{\mathbf{x}}_{i_{t}}\right)$, where $\overline{\mathbf{x}}_{i_{t}}$ is the last point for with we evaluated $\nabla f_{i}\left(\overline{\mathbf{x}}_{i}\right)$. $\mathbb{E}[Y]=\frac{1}{n} \sum_{i=1}^{n} \nabla f_{i}\left(\overline{\mathbf{x}}_{i}\right)$, but requires storage of $\left\{\overline{\mathbf{x}}_{i}\right\}_{i=1}^{n}$ or $\left\{\nabla f_{i}\left(\overline{\mathbf{x}}_{i}\right)\right\}_{i=1}^{n}$
- **Choice 3** $Y=\nabla f_{i_{t}}(\tilde{\mathbf{x}})$, where $\tilde{\mathbf{x}}$ is some fixed reference point. $\mathbb{E}[Y]=\frac{1}{n} \sum_{i=1}^{n} \nabla f_{i}(\tilde{\mathbf{x}})$, and requires computing full gradient.

## Stochastic Variance-Reduced Algorithms

### Stochastic Average Gradient (SAG) ($\alpha=\frac{1}{n}, Y=\mathbf{v}_{i_{t}}$)
- $\mathbf{g}_{t}=\frac{1}{n}\left(\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\mathbf{v}_{i_{t}}\right)+\frac{1}{n} \sum_{i=1}^{n} \mathbf{v}_{i}$ where $\mathbf{v}_{i}$ is the past gradient $\mathbf{v}_{i}^{t}= \begin{cases}\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right), & \text { if } i=i_{t} \\ \mathbf{v}_{i}^{t-1}, & \text { if } i \neq i_{t}\end{cases}$.
- Equivalently $\mathbf{g}_{t}=\mathbf{g}_{t-1}-\frac{1}{n} \mathbf{v}_{i_{t}}^{t-1}+\frac{1}{n} \nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)$, or the update rule $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\frac{\gamma}{n} \sum_{i=1}^{n} \mathbf{v}_{i}^{t}$.
- Same per-iteration cost as SGD but only additional memory cost of $O(nd)$.
- **Theorem 12.10 (Schmidt et al. 2017, Linear Convergence)** If $F$ is $\mu$-strongly convex and each $f_i$ is $L_i$-smooth and convex. Setting $\gamma=1 /\left(16 L_{\max }\right)$ where $L_{\max }:=\max_{i\in[n]} \left\{L_{i}\right\}$, SAG satisfies that $\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right] \leq C \cdot\left(1-\min \left\{\frac{1}{8 n}, \frac{\mu}{16 L_{\max }}\right\}\right)^{t}$.
- Full GD needs $O(\kappa \ln (\frac{1}{\epsilon}))$ iteration and $O(n)$ computation per-iter, so the total computation cost is $O(n\kappa \ln (\frac{1}{\epsilon}))$, where $\kappa = \overline{L_i} / \mu$.
- SAG needs  $O((n+\kappa_{\max}) \ln (\frac{1}{\epsilon}))$ iteration and $O(1)$ computation per-iter, $O(n+\kappa) \ll O(n\kappa)$ may be true if $n,\kappa$ are large.

### SAGA ($\alpha=1, Y=\mathbf{v}_{i_{t}}$, improved ver. of SAG)
- $\mathbf{g}_{t}=\left(\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\mathbf{v}_{i_{t}}\right)+\frac{1}{n} \sum_{i=1}^{n} \mathbf{v}_{i}$
- $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\gamma\left[\left(\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\mathbf{v}_{i_{t}}^{t-1}\right)+\frac{1}{n} \sum_{i=1}^{n} \mathbf{v}_{i}^{t-1}\right]$
- SAGA is unbiased, while SAG is biased, same $O(nd)$ memory cost as SAG, but proof much simpler.

### Stochastic Variance Reduced Gradient (SVRG) ($\alpha=1, Y=\nabla f_{i_{t}}(\tilde{\mathbf{x}})$)
- $\mathbf{g}_{t}=\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\nabla f_{i_{t}}(\tilde{\mathbf{x}})+\nabla F(\tilde{\mathbf{x}})$
- The induition is that $\mathbb{E}\left[\lVert\mathbf{g}_{t}-\nabla F\left(\mathbf{x}_{t}\right)\rVert ^{2}\right] \leq \mathbb{E}\left[\lVert\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\nabla f_{i_{t}}(\tilde{\mathbf{x}})\rVert ^{2}\right] \leq L_{\max }^{2}\lVert\mathbf{x}_{t}-\tilde{\mathbf{x}}\rVert ^{2}$
    - closer $\tilde{x}$ to $x_t$, smaller the variance, so update $\tilde{x}$ in outer loop.
- **Algorithm (Two-loop structure)**
    - **For** $s=1,2, \ldots$ **do** *(outer loop)*
        - Set $\tilde{\mathbf{x}}=\tilde{\mathbf{x}}^{s-1}$ and compute $\nabla F(\tilde{\mathbf{x}})=\frac{1}{n} \sum_{i=1}^{n} \nabla f_{i}(\tilde{\mathbf{x}})$ ($n$ grad computation)
        - Initialize $\mathbf{x}_{0}=\tilde{\mathbf{x}}$
        - **For** $t=0,1, \ldots, m-1$ **do** *(inner loop)* ($2m$ grad computation)
            - Randomly pick $i_{t} \in\{1:n\}$
            - Update $\mathbf{x}_{t+1}=\mathbf{x}_{t}-\eta\left(\nabla f_{i_{t}}\left(\mathbf{x}_{t}\right)-\nabla f_{i_{t}}(\tilde{\mathbf{x}})+\nabla F(\tilde{\mathbf{x}})\right)$
        - **End For**
        - Update $\tilde{\mathbf{x}}^{s}=\frac{1}{m} \sum_{t=0}^{m-1} \mathbf{x}_{t}$
    - **End for**
- Total of $O(n+2m)$ component gradient evaluations at each outer epoch.
- *Pros*: no need to store past gradients or past iterates
- *Cons*: More parameter tuning, two gradient computation per iteration.
- **Lemma 12.12 (Exercise 7.1)** $f_i(x)$ is convex and $L$-smooth, then $\frac{1}{n} \sum_{i=1}^{n}\lVert\nabla f_{i}(\mathbf{x})-\nabla f_{i}\left(\mathbf{x}^{\star}\right)\rVert _{2}^{2} \leq 2 L_{\max }\left(F(\mathbf{x})-F\left(\mathbf{x}^{\star}\right)\right)$
    - **Proof** is same as Exercise 6.1.
- **Lemma A** Denote $\mathbf{g}_{t}:=\nabla f_{i_{t}}\left(\mathbf{x}^{t}\right)-\nabla f_{i_{t}}(\tilde{\mathbf{x}})+\nabla F(\tilde{\mathbf{x}})$ then $\mathbb{E}\left[\lVert\mathbf{g}_{t}\rVert _{2}^{2}\right] \leq 4 L_{\max }\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)+F(\tilde{\mathbf{x}})-F\left(\mathbf{x}^{*}\right)\right]$.
    - **Proof**
        - Define $h_i(x) := f_i(x) - \nabla f_i (\tilde{x})^{\top} x + \nabla F (\tilde{x})^{\top} x$, one can check that $H(x) := \mathbb{E}[h_i(x)] = F(x)$
        - By the conclusion in Exercise 6.2, we have $\mathbb{E}[\Vert \nabla h_i(x_t)\Vert _2^2]\leq 4 L_{\max}[F(x_t) - F(x^{\star})] + 2\mathbb{E}[\Vert \nabla h_i(x^{\star})\Vert ^2_2]$
        - By definition $\nabla h_i(x^{\star}) := (\nabla f_i(x^{\star}) - \nabla f_i(\tilde{x})) + (\nabla F(\tilde{x})- \nabla F(x^{\star}))$
            - Therefore $\mathbb{E}[\Vert \nabla h_i(x^{\star})\Vert ^2_2] = \mathbb{E}[\Vert \nabla f_i(x^{\star}) - \nabla f_i(\tilde{x})\Vert ^2_2] + \Vert \nabla F(\tilde{x})- \nabla F(x^{\star})\Vert ^2_2 + 2\mathbb{E}[\nabla f_i(x^{\star}) - \nabla f_i(\tilde{x})]^{\top}(\nabla F(\tilde{x})- \nabla F(x^{\star}))$
            - $\mathsf{RHS} = \mathbb{E}[\Vert \nabla f_i(x^{\star}) - \nabla f_i(\tilde{x})\Vert ^2_2] - \Vert \nabla F(\tilde{x})- \nabla F(x^{\star})\Vert ^2_2 \leq \mathbb{E}[\Vert \nabla f_i(x^{\star}) - \nabla f_i(\tilde{x})\Vert ^2_2] \leq 2L_{\max}(F(\tilde{x})- F(x^{\star}))$, by Exercise 6.1.
        - Plug this in and we get the result.
- **Theorem 12.11 (Johnson & Zhang, 2013, geometric convergence)** Assume $f_i(x)$ is convex and $L$-smooth and $F(\mathbf{x}):=\frac{1}{n} \sum_{i=1}^{n} f_{i}(\mathbf{x})$ is $\mu$-strongly convex. Let $\mathbf{x}_{*}=\arg \min _{\mathbf{x}} F(\mathbf{x})$. Assume $m$ is sufficiently large (and $\eta < 1/2L$), so that, $\displaystyle \rho=\frac{1}{\mu \eta(1-2 L \eta) m}+\frac{2 L \eta}{1-2 L \eta}<1$,
    - then we have geometric convergence in expectation $\mathbb{E}\left[F\left(\tilde{\mathbf{x}}^{s}\right)-F\left(\mathbf{x}_{*}\right)\right] \leq \rho^{s}\left[F\left(\tilde{\mathbf{x}}^{0}\right)-F\left(\mathbf{x}_{*}\right)\right]$.
    - **Proof**
        - $\mathbb{E}\left[\lVert\mathbf{x}_{t+1}-\mathbf{x}^{*}\rVert _{2}^{2}\right]=\lVert\mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-2 \eta\left(\mathbf{x}_{t}-\mathbf{x}^{*}\right)^{T} \mathbb{E}\left[\mathbf{g}_{t}\right]+\eta^{2} \mathbb{E}\left[\lVert\mathbf{g}_{t}\rVert _{2}^{2}\right]$
        - By convexity and Lemma A $\mathsf{RHS} \leq\lVert\mathbf{x}_{t}-\mathbf{x}^{*}\rVert _{2}^{2}-2 \eta(1-2 L \eta)\left(F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right)+4 L \eta^{2}\left[F(\mathbf{x}_0)-F\left(\mathbf{x}^{*}\right)\right]$
        - The above is for a single step of inner loop, we sum over $t\in[0:m-1]$ and get
        - $\mathbb{E}\left[\lVert\mathbf{x}_{m}-\mathbf{x}^{*}\rVert _{2}^{2}\right] \leq \lVert\mathbf{x}_{0}-\mathbf{x}^{*}\rVert _{2}^{2} - 2 \eta(1-2 L \eta)\sum_{t=0}^{m-1}\mathbb{E}\left[F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right] + 4m L \eta^{2}\left[F(\mathbf{x}_0)-F\left(\mathbf{x}^{*}\right)\right]$
        - By convexity, we have $\sum_{t=0}^{m-1}\left(F\left(\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right) \geq m\left(F\left(\frac{1}{m}\sum_{t=0}^{m-1}\mathbf{x}_{t}\right)-F\left(\mathbf{x}^{*}\right)\right)= m\left(F\left(\tilde{\mathbf{x}}^{s}\right)-F\left(\mathbf{x}^{*}\right)\right)$
        - and by the condition of $\eta < 1/2L$, we have
            - $\mathbb{E}\left[\lVert\mathbf{x}_{m}-\mathbf{x}^{*}\rVert _{2}^{2}\right] \leq \lVert\mathbf{x}_{0}-\mathbf{x}^{*}\rVert _{2}^{2} - 2 \eta m (1-2 L \eta)\mathbb{E}\left[F\left(\tilde{\mathbf{x}}^{s}\right)-F\left(\mathbf{x}^{*}\right)\right] + 4m L \eta^{2}\left[F(\mathbf{x}_0)-F\left(\mathbf{x}^{*}\right)\right]$
        - By definition $\mathbf{x}_0 = \tilde{\mathbf{x}}^{s-1}$, and by $\mu$-strong convexity $\lVert\mathbf{x}_{0}-\mathbf{x}^{*}\rVert _{2}^{2} \leq \frac{2}{\mu} (F(\mathbf{x}_0)-F\left(\mathbf{x}^{*}\right))$
            - $2 \eta m (1-2 L \eta)\mathbb{E}\left[F\left(\tilde{\mathbf{x}}^{s}\right)-F\left(\mathbf{x}^{*}\right)\right] + \underbrace{\mathbb{E}\left[\lVert\mathbf{x}_{m}-\mathbf{x}^{*}\rVert _{2}^{2}\right]}_{\mathrm{omitted}}\leq \left( \frac{2}{\mu} + 4m L \eta^{2}\right)\mathbb{E}\left[F(\tilde{\mathbf{x}}^{s-1})-F\left(\mathbf{x}^{*}\right)\right]$
        - This means $\mathbb{E}\left[F\left(\tilde{\mathbf{x}}^{s}\right)-F\left(\mathbf{x}_{*}\right)\right] \leq\left[\frac{1}{\mu \eta(1-2 L \eta) m}+\frac{2 L \eta}{1-2 L \eta}\right] \mathbb{E}\left[F\left(\tilde{\mathbf{x}}^{s-1}\right)-F\left(\mathbf{x}_{*}\right)\right]$. QED
- **Remark 12.13** Setting $\eta L = \theta$ gives $\rho=\frac{L}{\mu \theta(1-2 \theta) m}+\frac{2 \theta}{1-2 \theta}=O\left(\frac{L}{\mu m}+\text { const. }\right)$, If further set $m=O(L / \mu)$, we get a constant $\rho$.
    - Then the number of epoch for $\varepsilon$ optimal is $O\left(\log \left(\frac{1}{\epsilon}\right)\right)$. Total number of gradient computation required is $\mathcal{O}\left((m+n) \log \left(\frac{1}{\epsilon}\right)\right)=\mathcal{O}\left(\left(n+\frac{L}{\mu}\right) \log \left(\frac{1}{\epsilon}\right)\right)$
- **Remark** 
    - if use importance sampling $\mathbb{P}\left(i_{t}=i\right)=\frac{L_{i}}{\sum L_{i}}$, we get $L_{\mathrm{avg}}$ instead of $L_{\max}$.
    - Incorporating acceleration, can improve to $O\left(\left(n+\sqrt{n \kappa_{\max }}\right) \log \frac{1}{\epsilon}\right)$.
    - Lower complexity bound of $O\left(\left(n+\sqrt{n \kappa_{\max }}\right) \log \frac{1}{\epsilon}\right)$ is proved for strongly-convex and smooth finite-sum problems.

### SPIDER/SARAH/STORM (VR for non-convex $f$)
- If objective satisfies *average-smoothness* $\mathbb{E}_{i}\lVert\nabla f_{i}(\mathbf{x})-\nabla f_{i}(\mathbf{y})\rVert ^{2} \leq L^{2}\Vert \mathbf{x}-\mathbf{y}\Vert ^{2}$, then complexity can be reduced to $O\left(\min \left\{\sqrt{n} / \epsilon^{2}, \epsilon^{-3}\right\}\right)$ instead of $O\left(n / \epsilon^{2}\right)$.
- **Algorithm**
    - **Input** $T$ iteration, $Q$ epoch length, $D$ batch size 1, $D$ batch size 2, $x_0$, $alpha$, $\eta$, $\omega(x)$
    - **For** $t\in[0:T-1]$ **do**
        - **If** $t\equiv 0 (\mathrm{mod} Q)$ **then**
            - compute $\mathbf{g}_{t}=\frac{1}{D} \sum_{i=1}^{D} \nabla f\left(\mathbf{x}_{t} ; \boldsymbol{\xi}_{t}^{i}\right)$
        - **else**
            - compute $\mathbf{g}_{t}=(1-\eta)\left(\mathbf{g}_{t-1}-\frac{1}{S} \sum_{i=1}^{S} \nabla f\left(\mathbf{x}_{t-1} ; \boldsymbol{\xi}_{t}^{i}\right)\right)+\frac{1}{S} \sum_{i=1}^{S} \nabla f\left(\mathbf{x}_{t} ; \boldsymbol{\xi}_{t}^{i}\right)$.
        - **End if**
        - $\mathbf{x}_{t+1}=\operatorname{argmin}_{\mathbf{x} \in X}\left\{\mathbf{g}_{t}^{\top} x+\frac{1}{\alpha} V_{\omega}\left(\mathbf{x}, \mathbf{x}_{t}\right)\right\}$
    - **End for**
    - **Output** $\mathbf{x}_\tau$ with $\tau$ randomly chosen from $[0:T-1]$
- *Complexity* Total time for gradient calculation is $\mathcal{O}(T(2 S+D / Q))$.
- Difference amoung each algorithms
    - STORM usually is the best.


|  Parameters  |                  SPIDER                   |                   SARAH                   |                   STORM                   |                     New 1                     |                   New 2                   |
| :----------: | :---------------------------------------: | :---------------------------------------: | :---------------------------------------: | :-------------------------------------------: | :---------------------------------------: |
|    $T$     | $\mathcal{O}\left(\epsilon^{-2}\right)$ | $\mathcal{O}\left(\epsilon^{-3}\right)$ | $\mathcal{O}\left(\epsilon^{-3}\right)$ | $\mathcal{O}\left(\epsilon^{-5 / 2}\right)$ | $\mathcal{O}\left(\epsilon^{-3}\right)$ |
|   $T/Q$    | $\mathcal{O}\left(\epsilon^{-1}\right)$ | $\mathcal{O}\left(\epsilon^{-1}\right)$ |                   $1$                   |   $\mathcal{O}\left(\epsilon^{-1}\right)$   |                   $1$                   |
|    $D$     | $\mathcal{O}\left(\epsilon^{-2}\right)$ | $\mathcal{O}\left(\epsilon^{-2}\right)$ |            $\mathcal{O}(1)$             |   $\mathcal{O}\left(\epsilon^{-2}\right)$   | $\mathcal{O}\left(\epsilon^{-1}\right)$ |
|    $S$     | $\mathcal{O}\left(\epsilon^{-1}\right)$ |            $\mathcal{O}(1)$             |            $\mathcal{O}(1)$             | $\mathcal{O}\left(\epsilon^{-1 / 2}\right)$ |            $\mathcal{O}(1)$             |
|  $\eta_t$  |                   $0$                   |                   $0$                   |  $\mathcal{O}\left(t^{-2 / 3}\right)$   |                     $0$                     | $\mathcal{O}\left(\epsilon^{2}\right)$  |
| $\alpha_t$ |            $\mathcal{O}(1)$             |         $\mathcal{O}(\epsilon)$         |  $\mathcal{O}\left(t^{-1 / 3}\right)$   | $\mathcal{O}\left(\epsilon^{1 / 2}\right)$  |         $\mathcal{O}(\epsilon)$         |
|  Complexity  | $\mathcal{O}\left(\epsilon^{-3}\right)$ | $\mathcal{O}\left(\epsilon^{-3}\right)$ | $\mathcal{O}\left(\epsilon^{-3}\right)$ |   $\mathcal{O}\left(\epsilon^{-3}\right)$   | $\mathcal{O}\left(\epsilon^{-3}\right)$ |
