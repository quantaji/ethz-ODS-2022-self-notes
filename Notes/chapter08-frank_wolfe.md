# Chapter 8 The Frank-Wolfe Algorithm

- **Definition (LMO)** *linear minimization oracle* $\operatorname{LMO}_{X}(\mathbf{g}):=\underset{\mathbf{z} \in X}{\operatorname{argmin}} \mathbf{g}^{\top} \mathbf{z}$.
    - This exists when $X$ is bounded and closed.
- **Algo** $\mathbf{s}:=\mathrm{LMO}_{X}\left(\nabla f\left(\mathbf{x}_{t}\right)\right)$ and $\mathbf{x}_{t+1}:=\left(1-\gamma_{t}\right) \mathbf{x}_{t}+\gamma_{t} \mathbf{s}$ and $\gamma_{t} \in[0,1]$.
    - Reduce non-linear to linear problem.
- **Properties**
    - If $X$ convex, then iterates are *always feasible*, $\mathbf{x}_{0}, \mathbf{x}_{1}, \ldots, \mathbf{x}_{t} \in X$.
    - *Projection-free* solving a linear program instead of quadratic program
    - *sparse representation* $\mathbf{x}_t$ is always a convex combination of initial iterate and minimizers used so far.

## Cases when LMO is simple to compute
- FW algo is useful when $X$ can be described as a convex hull of a finite or other nise set of *atom* points $\mathcal{A}$, $X:= \operatorname{conv}(\mathcal{A})$.
    - $\mathbf{s}=\sum_{i=1}^{n} \lambda_{i} \mathbf{a}_{i}$, where $\sum_{i=1}^{n} \lambda_{i}=1$ and all non-negative.
    - Then if $\mathbf{s}$ minimize $\mathbf{g}^{\top} \mathbf{z}$, then there is also an atomic minimizer.
    - $\mathcal{A} = X$ is the trivial case, we are interested in *extreme points* where $\mathbf{x} \notin \operatorname{conv}(X \backslash\{\mathbf{x}\})$.

### LASSO with $\ell_1$-ball
- Problem: $\min _{\mathbf{x} \in \mathbb{R}^{d}}\Vert A \mathbf{x}-\mathbf{b}\Vert ^{2}$ s.t. $\Vert \mathbf{x}\Vert _{1} \leq 1$, we see that $X=\operatorname{conv}\left(\left\{\pm \mathbf{e}_{1}, \ldots, \pm \mathbf{e}_{d}\right\}\right)$.
    - It is easy to show $\mathrm{LMO}_{X}(\mathbf{g}) = -\operatorname{sgn}\left(g_{i}\right) \mathbf{e}_{i} \text { with } i:=\underset{i \in[d]}{\operatorname{argmax}}\vert g_{i}\vert$.


### Semidefinite Programming and the Spectahedron
- Problem: $\arg\min_{Z} G \bullet Z$ s.t. $\operatorname{Tr}(Z)=1$ and $Z \succeq 0$, where $G$ semetric, and $\bullet$ stands for scalor product.
    - Feasible region $X$ is called *Spectahedron*
- Since every semetric matrix can be decomposed into $C^{\top}C = \sum_{i\in[d]}z_{i}z_{i}^{\top}$, natually the atom is $\mathbf{Z} \mathbf{Z}^{\top}$ where $\mathbf{z} \in \mathbb{R}^{d},\Vert \mathbf{z}\Vert =1$.
- **Lemma 8.1** Let $\lambda_1$ be the smallest eigenvalue of $G$, and let $\mathbf{s}_1$ be a corresponding eigenvector of unit length. Then we can choose $\mathrm{LMO}_{X}(G)=\mathbf{s}_{1} \mathbf{s}_{1}^{\top}$.
    - **Proof** $\min _{\operatorname{Tr}(Z)=1, Z \succeq 0} G \bullet Z=\min _{\Vert \mathbf{z}\Vert =1} G \bullet \mathbf{z z}^{\top}=\min _{\Vert \mathbf{z}\Vert =1} \mathbf{z}^{\top} G \mathbf{z}=\lambda_{1}$

### Matrix completion (Exercise 54)
- Problem: $\min _{Y \in X \subseteq \mathbb{R}^{n \times m}} \sum_{(i, j) \in \Omega}\left(Z_{i j}-Y_{i j}\right)^{2}$ where $X:=\operatorname{conv}(\mathcal{A})$ with $\mathcal{A}:=\left\{\mathbf{u} \mathbf{v}^{\top} \mid \mathbf{u} \in \mathbb{R}^{n},\Vert \mathbf{u}\Vert _{2}=1 , \mathbf{v} \in \mathbb{R}^{m},\Vert \mathbf{v}\Vert _{2}=1\right\}$
- F-W step: $\partial_{Y_{ij}} = Y_{ij}-Z_{ij}$, $\mathrm{LMO}_X = \arg\min_{X} \sum_{ij} (Y_{ij}-Z_{ij})(\mathbf{uv}^{\top})_{i,j} = \mathbf{u}^{\top} (Y-Z) \mathbf{v}$
    - consider the SVD of $Y-Z$, $Y-Z = U\Sigma V^{\top}$, where $\Sigma\in\mathbb{R}^{n\times m}$ is diagnal.
    - Then $U^{\top}u$ and $V^{\top} v$ also norm-$1$. This gives solution of $k:= \arg \max_{i\in[\min\{n,m\}]} \sigma(\Sigma)_i$, and $u=U\mathbf{e}_{k}, v=-V\mathbf{e}_{k}$
    - $\mathbf{uv}^{\top} = - U\mathbf{E}_{k k}V^{\top}$
- PS: Matrix completion has been removed from this course, so I don't know the normal procedure for projection...

## Duality gap, A certificate for optimization quality
- **Definition (Duality gap)** Given $\mathbf{x}\in X$ the *duality gap(Hearn gap)* is $g(\mathbf{x}):=\nabla f(\mathbf{x})^{\top}(\mathbf{x}-\mathbf{s})$ where $\mathbf{s}:=\mathrm{LMO}_{X}(\nabla f(\mathbf{x}))$.
- **Lemma 8.2** Suppose there is a minimizer for F-W algo,$\mathbf{x}^{\star}$, $f$-convex. Let $\mathbf{x}\in X$. Then $g(\mathbf{x}) \geq f(\mathbf{x})-f\left(\mathbf{x}^{\star}\right)$.
    - **Proof** $g(\mathbf{x}) =\nabla f(\mathbf{x})^{\top}(\mathbf{x}-\mathbf{s}) \geq \nabla f(\mathbf{x})^{\top}\left(\mathbf{x}-\mathbf{x}^{\star}\right)  \geq f(\mathbf{x})-f\left(\mathbf{x}^{\star}\right) \geq 0$
    - $g(\mathbf{x}^{\star}) = 0$ since $\nabla f\left(\mathbf{x}^{\star}\right)^{\top}\left(\mathbf{x}-\mathbf{x}^{\star}\right) \geq 0$, $\forall \mathbf{x} \in X$ and this means $g(\mathbf{x}^{\star}) \leq 0$.
    - Note that $g(\mathbf{x})\leq \Vert \nabla f(\mathbf{x})\Vert _{a*} \Vert \mathbf{x}-\mathbf{s}\Vert _{a}$.

## Convegence in $\mathcal{O}(1 / \varepsilon)$ Steps
- Interestingly, step size can be set to be unrelated to smooth constant.

### Case for $\gamma_{t}=2 /(t+2)$
- **Lemma 8.4 (Descent Lemma)** For a step $\operatorname{step} \mathbf{x}_{t+1}:=\mathbf{x}_{t}+\gamma_{t}\left(\mathbf{s}-\mathbf{x}_{t}\right)$ with $\gamma_{t} \in[0,1]$, we have $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\gamma_{t} g\left(\mathbf{x}_{t}\right)+\gamma_{t}^{2} \frac{L}{2}\lVert\mathbf{s}-\mathbf{x}_{t}\rVert ^{2}$, where $\mathbf{s}=\mathrm{LMO}_{X}\left(\nabla f\left(\mathbf{x}_{t}\right)\right)$.
    - **Proof**
        - Smoothness $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right) + \gamma_{t} \nabla f\left(\mathbf{x}_{t}\right)^{\top}\left(\mathbf{s}-\mathbf{x}_{t}\right) + \frac{L}{2}\gamma_{t}^2 \Vert \mathbf{s}-\mathbf{x}_{t}\Vert _{a}^2 = f\left(\mathbf{x}_{t}\right) - \gamma_{t} g(\mathbf{x}_{t}) + \frac{L}{2}\gamma_{t}^2 \Vert \mathbf{s}-\mathbf{x}_{t}\Vert _{a}^2$.
- **Theorem 8.3** If $f$ convex and $L$-smooth, $X$ convex and bounded. With any start $\mathbf{x}_0\in X$ and stepsize $\gamma_{t}=2 /(t+2)$ F-W algo gives $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{2 L \operatorname{diam}(X)^{2}}{T+1}$ where $\operatorname{diam}(X):=\max _{\mathbf{x}, \mathbf{y} \in X}\Vert \mathbf{x}-\mathbf{y}\Vert$.
    - **Proof**
        - By certificate property, $g\left(\mathbf{x}_{t}\right) \geq f\left(\mathbf{x}_{t}\right) - f\left(\mathbf{x}^{\star}\right)$, so $\Delta f\left(\mathbf{x}_{t+1}\right) \leq (1-\gamma_t) \Delta f\left(\mathbf{x}_{t}\right) + \gamma_{t}^{2} C$, where $C:= \frac{L}{2}\lVert\mathbf{s}-\mathbf{x}_{t}\rVert ^{2}$.
        - We assme $\Delta f\left(\mathbf{x}_{t}\right) \leq \frac{4C}{t+1}$, this is true for $t=0$, since by smoothness $f\left(\mathbf{x}_{0}\right) \leq f\left(\mathbf{x}^{\star}\right) + \frac{L}{2}\Vert \mathbf{x}^{\star}-\mathbf{x}_{0}\Vert _a^2$.
        - By spritis of induction, we assmue this holds for $t$ and smaller, then for $t+1$ we have $\Delta f\left(\mathbf{x}_{t+1}\right) \leq (1-\frac{2}{t+2}) \frac{4C}{t+1} + \frac{4}{(t+2)^2} C = \frac{4C}{t+2}\frac{t(t+2) + (t+1)}{(t+2)(t+1)} = \frac{4C}{t+2}\frac{t^2 + 3t + 1}{t^2 + 3t+2} \leq \frac{4C}{t+2}$

### Other step size
- *Line search* $\gamma_{t}:=\underset{\gamma \in[0,1]}{\operatorname{argmin}} f\left((1-\gamma) \mathbf{x}_{t}+\gamma \mathbf{s}\right)$. This can be guranteed to be faster than previous step size.
- *Gap-based* $\gamma_{t}:=\min \left(\frac{g\left(\mathbf{x}_{t}\right)}{L\lVert\mathbf{s}-\mathbf{x}_{t}\rVert ^{2}}, 1\right)$, this is the quadratic function minimizer for $\gamma_t\in[0, 1]$, so definitely, it is better than $2/(t+2)$
    - $h\left(\mathbf{x}_{t+1}\right) \leq \begin{cases}h\left(\mathbf{x}_{t}\right)\left(1-\frac{\gamma_{t}}{2}\right), & \gamma_{t}<1 \\ h\left(\mathbf{x}_{t}\right), & \gamma_{t}=1\end{cases}$. (This can be proved easily)

    
### Affine invariance
- The upper bound seems to depends on the coordinate and changes under affine transform, but in reality, the algorithm objective $\nabla f^{\prime}\left(\mathbf{x}^{\prime}\right)^{\top} \mathbf{z}^{\prime}$ is unchanged under affine transform.
- This contradiction can be solved by defining a new *curvature constant* $C_{(f, X)}:=\sup _{\substack{\mathbf{x}, \mathbf{s} \in X, \gamma \in(0,1] \\ \mathbf{y}=(1-\gamma) \mathbf{x}+\gamma \mathbf{s}}} \frac{1}{\gamma^{2}}\left(f(\mathbf{y})-f(\mathbf{x})-\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})\right)$ which is affine invariant. (PS: this is similar to Bregman div)
    - By this definition, we have $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\nabla f\left(\mathbf{x}_{t}\right)^{\top} \gamma_{t}\left(\mathbf{x}_{t}-\mathbf{s}\right)+\gamma_{t}^{2} C_{(f, X)}$
- **Theorem 8.5** (*proof is similar*) $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{4 C_{(f, X)}}{T+1}$.
    - no smoothness assumed.
- **Lemma 8.6 (Exercise 52)** Let $f$ convex and $L$-smooth, then $C_{(f,X)}$ is a tighter constant, $C_{(f, X)} \leq \frac{L}{2} \operatorname{diam}(X)^{2}$.
    - **Proof** $f(\mathbf{x} + \gamma(\mathbf{s}-\mathbf{x})) \leq f(\mathbf{x}) + \gamma \nabla f(\mathbf{x})^{\top}(\mathbf{s}-\mathbf{x}) + \frac{\gamma^2L}{2} \Vert \mathbf{s}-\mathbf{x}\Vert _{a}^2$
- All of the stepsize holds the following inequality $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\nabla f\left(\mathbf{x}_{t}\right)^{\top} \mu_{t}\left(\mathbf{x}_{t}-\mathbf{s}\right)+\mu_{t}^{2} C_{(f, X)}$ where $\mu_{t} := 2/(t+2)$

### Convergence of duality gap
- **Theorem 8.7** $f$ convex and $L$-smooth, then choosing any of stepsize in $2/(t+2)$, line search or gap-based stepsize, F-W algo gives duality gap minimum such that $\exists t\in[1:T]$ s.t. $g\left(\mathbf{x}_{t}\right) \leq \frac{27 / 2 \cdot C_{(f, X)}}{T+1}$.
    - **Proof** See [Revisiting Frank-Wolfe: Projection-Free Sparse Convex Optimization](https://os.zhdk.cloud.switch.ch/tind-tmp-epfl/868923b6-e0a1-44c1-81de-1007d0b72fb4?response-content-disposition=attachment%3B%20filename%2A%3DUTF-8%27%27jaggi13-supp.pdf&response-content-type=application%2Fpdf&AWSAccessKeyId=ded3589a13b4450889b2f728d54861a6&Expires=1660427390&Signature=nqJz3xkkHQYhywJhz6SuhIbbbj4%3D) Appendix B for detail.
    - Looser Proof:
        - By descent lemma $\mu_{t}g(x_t) \leq f\left(\mathbf{x}_{t}\right) - f\left(\mathbf{x}_{t+1}\right)  + \mu_{t}^2 C_{(f, X)}$
        - sum over $t\in[\lfloor T/2 \rfloor:T]$, by the fact of $2(\ln (t+2)-\ln (t+3))\leq \mu_t = \frac{2}{t+2} \leq (\ln (t+1)-\ln (t+2))$
            - $\sum_{t=\lfloor T/2 \rfloor}^{T}\mu_t \geq 2\ln\frac{T+2}{\lfloor T/2 \rfloor +3}$
        - And the fact of $\mu_t^2 \leq \frac{4}{(t+2)(t+1)} = \frac{4}{t+1} - \frac{4}{t+2}$, so that $\sum_{t=\lfloor T/2 \rfloor}^{T}\mu_t^2 \leq \frac{4}{T+1} - \frac{4}{\lfloor T/2 \rfloor+2}$
        - Also $f\left(\mathbf{x}_{\lfloor T/2 \rfloor}\right) - f\left(\mathbf{x}_{T}\right) \leq f\left(\mathbf{x}_{\lfloor T/2 \rfloor}\right) - f\left(\mathbf{x}^{\star}\right) \leq 4C_{(f,X)}/ (\lfloor T/2 \rfloor+1)$ 
        - we have $\min_{t\in[\lfloor T/2 \rfloor, T]} g(x_t) \leq \ldots \leq \frac{2C_{(f,X)}}{\ln\frac{T+2}{\lfloor T/2 \rfloor +3}}\left( \frac{1}{\lfloor T/2 \rfloor+1} + \frac{1}{T+1} - \frac{1}{\lfloor T/2 \rfloor+2}\right) = \frac{2C_{(f,X)}}{T+1} \dfrac{1 + \frac{T+1}{(\lfloor T/2 \rfloor+1)(\lfloor T/2 \rfloor+2)}}{\ln\frac{T+2}{\lfloor T/2 \rfloor +3}}$
        - We get a similar conclusion, but loser, the constant is about, when $T>3$, coefficient $\leq 15$.
