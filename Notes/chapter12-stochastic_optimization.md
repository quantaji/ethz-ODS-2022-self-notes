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
