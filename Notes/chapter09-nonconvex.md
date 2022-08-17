# Chapter 9 Nonconvex functions
- **Lemma 9.1** Let $f\in C^2$, with $X\subseteq \mathrm{dom}(f)$ convex, if $\lVert \nabla^{2} f(\mathbf{x})\rVert  \leq L,\forall \mathbf{x}$, where $\Vert \cdot\Vert$ is spectual norm, then $f$ is $L$-smooth.
    - **Proof** similar to Lemma A of Chapter 10.
- **Idea** For non convex function, instead of focusing on $f$, we focus on convergence of $\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2}$ to a critical point.
- **Theorem 9.2** $f\in C^2$ is $L$-smooth with global minimum $\mathbf{x}^{\star}$, then a stepsize of $\gamma=1/L$ gives $\frac{1}{T} \sum_{t=0}^{T-1}\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2} \leq \frac{2 L}{T}\left(f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}^{\star}\right)\right)$, and $\lim _{t \rightarrow \infty}\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2}=0$.
    - **Proof**
        - sufficient descent gives $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L}\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2}$, which means $\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2} \leq 2 L\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}_{t+1}\right)\right)$
        - so summation and we get the first result, also if  $\lim _{t \rightarrow \infty}\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2}=g>0$ will lead to contradiction.
- **Lemma 9.3(with stepsize 1/L, it cannot overshoot.)** $f\in C^2$ is $L$-smooth, if $\nabla f(\mathbf{x}) \neq \mathbf{0}$, then update with $\gamma=1 / L^{\prime}<1 / L$ will never give a critical point $\nabla f(\mathbf{x}^{\prime}=\mathbf{x}-\gamma \nabla f(\mathbf{x}))\neq 0$.
    - **Proof**
        - By smoothness we have $L$-Lipschitz  $\Vert \nabla f(\mathbf{x}) - \nabla f(\mathbf{x}')\Vert  \leq L\Vert \mathbf{x}- \mathbf{x}'\Vert  = \frac{L}{L'}\Vert \nabla f(\mathbf{x})\Vert  < \Vert \nabla f(\mathbf{x})\Vert$
        - This means $\Vert \nabla f(\mathbf{x}')\Vert  \geq\Vert \nabla f(\mathbf{x})\Vert  - \Vert \nabla f(\mathbf{x}') - \nabla f(\mathbf{x})\Vert  > 0$.

## Trajectory analysis
- Some times we can prove GD avoids saddle points and converge to global optimal

### Deep Linear Neural Networks
- Objective $\lVert W_{\ell} W_{\ell-1} \cdots W_{1} X-Y\rVert _{F}^{2}$

### Width-1 DLNN
- We want when $x=1$ then $y=1$, this gives an objective of $f(\mathbf{x}):=\frac{1}{2}\left(\prod_{k=1}^{d} x_{k}-1\right)^{2}$
- The gradient gives $\nabla f(\mathbf{x})=\left(\prod_{k} x_{k}-1\right)\left(\prod_{k \neq 1} x_{k}, \ldots, \prod_{k \neq d} x_{k}\right)^{\top}$
    - global minimum when $\prod_{k} x_{k}=1$
    - other critical point when at least *two* $x_k$ is zero, thay give non-minimum of $f=1/2$.
- We want to show that from anyhwere in $X=\left\{\mathbf{x}: \mathbf{x}>\mathbf{0}, \prod_{k} \mathbf{x}_{k} \leq 1\right\}$, GD converge to global minimum. However, $f$ is not smooth in $X$.
- But we can later show $f$ smooth along trajectory, then with sufficient descent, we know $f$ always decreasing, and the starting point, we have $f<1/2$, then never to a saddle point.
- Even in this, we still cannot prove global minimum, since $X$ is unbounded, GD may make $\mathbf{x}$ to infinity.
- **Definition 9.4** If $\mathbf{x}>0$ componentwise, let $c\geq 1$, $\mathbf{x}$ is called $c$-balanced if $x_{i} \leq c x_{j}$ for all $1 \leq i, j \leq d$
- **Lemma 9.5** If $\mathbf{x}>0$ be $c$-balanced with $\prod_{k} x_{k} \leq 1$, then for any stepsize $\gamma>0, \mathbf{x}^{\prime}:=\mathbf{x}-\gamma \nabla f(\mathbf{x})$ satisfies (i) $\mathbf{x}^{\prime} \geq \mathbf{x}$ componentwise and (ii) $\mathbf{x}'$ is also $c$-balanced.
    - **Proof**
        - Set $\Delta:=-\gamma\left(\prod_{k} x_{k}-1\right)\left(\prod_{k} x_{k}\right) \geq 0$, then gradient descent gives $x_{k}^{\prime}=x_{k}+\frac{\Delta}{x_{k}} \geq x_{k}$
        - We havee $x_{i} \leq c x_{j}$ and $x_{j} \leq c x_{i}\Leftrightarrow 1 / x_{i} \leq c / x_{j}$, so $x_{i}^{\prime}=x_{i}+\frac{\Delta}{x_{i}} \leq c x_{j}+\frac{\Delta c}{x_{j}}=c x_{j}^{\prime}$
    - If we define $c\leq1$-co-balanced as $x_{i} \geq c x_{j}$ for all $1 \leq i, j \leq d$ 
    - then when $\prod_{k} x_{k} \geq 1$, then $\mathbf{x}' < \mathbf{x}$, while still $\mathbf{x}'$ is $c$-co-balanced.
#### Smoothness along the trajectory
- We can derive smoothness from bounded Hessian
- The hessian is $\nabla^{2} f(\mathbf{x})_{i j}= \begin{cases}\left(\prod_{k \neq i} x_{i}\right)^{2}, & j=i \\ 2 \prod_{k \neq i} x_{k} \prod_{k \neq j} x_{k}-\prod_{k \neq i, j} x_{k}, & j \neq i\end{cases}$
- **Lemma 9.6** If $\mathbf{x}>0$ is $c$-balanced, then for any subset $I \subseteq\{1, \ldots, d\}$, $\left(\frac{1}{c}\right)^{\vert I\vert }\left(\prod_{k} x_{k}\right)^{1-\vert I\vert  / d} \leq \prod_{k \notin I} x_{k} \leq c^{\vert I\vert }\left(\prod_{k} x_{k}\right)^{1-\vert I\vert  / d}$
    - **Proof**
        - For any $i$, we have $x_{i}^{d} \geq(1 / c)^{d} \prod_{k} x_{k}$ so $x_{i} \geq (1 / c)\left(\prod_{k} x_{k}\right)^{1 / d}$, similarly $x_{i}^{d} \leq c^{d} \prod_{k} x_{k}$ so $x_{i} \leq c \left(\prod_{k} x_{k}\right)^{1/d}$
        - Plug in this and we get the result.
    - If $c$-co-balanced, $I \subseteq\{1, \ldots, d\}$, $\left(\frac{1}{c}\right)^{\vert I\vert }\left(\prod_{k} x_{k}\right)^{1-\vert I\vert  / d} \geq \prod_{k \notin I} x_{k} \geq c^{\vert I\vert }\left(\prod_{k} x_{k}\right)^{1-\vert I\vert  / d}$
- **Lemma 9.7** If $\mathbf{x}>0$ be $c$-balanced with $\prod_{k} x_{k} \leq 1$, then $\lVert \nabla^{2} f(\mathbf{x})\rVert  \leq\lVert \nabla^{2} f(\mathbf{x})\rVert _{F} \leq 3 d c^{2}$
    - **Proof**
        - For any matrix $A$, $\Vert Ax\Vert ^2 = \sum_{i}(a_i^{\top}x)^2 \leq \sum_{i} (\sum_{j} a_{ij})^2(\sum_{j} x_{j})^2 = \Vert A\Vert _F^2\Vert x\Vert ^2_2$, then $\Vert A\Vert  \leq \Vert A\Vert _F$
        - To bound $\nabla ^2 f$, first we bound on diagnal term $\lvert \nabla^{2} f(\mathbf{x})_{i i}\rvert =\lvert \left(\prod_{k \neq i} x_{i}\right)^{2}\rvert  \leq c^{2}$
        - then for off-diagnal term $\lvert \nabla^{2} f(\mathbf{x})_{i j}\rvert  \leq\lvert 2 \prod_{k \neq i} x_{k} \prod_{k \neq j} x_{k}\rvert +\lvert \prod_{k \neq i, j} x_{k}\rvert  \leq 3 c^{2}$
        - sum together we get $\Vert \nabla ^2 f\Vert _F^2 \leq d c^4 + 9 d(d-1)c^4 \leq 9 d^2 c^4$, QED.
    - If $c$-co-balanced, we can prove $\lVert \nabla^{2} f(\mathbf{x})\rVert  \leq\lVert \nabla^{2} f(\mathbf{x})\rVert _{F} \leq 3 d \frac{1}{c^{2}}$
- **Lemma 9.8 (Summary of previous)** If $\mathbf{x}>0$ be $c$-balanced with $\prod_{k} x_{k} \leq 1$, $L=3 d c^{2}$, Let $\gamma:=1 / L$, then GD with this lr gives $\mathbf{x}_{t}$ always $c$-balanced, and $f$ is $L$-smooth along the line segment of trajectory.
    - **Proof** key is that smooth function never pass critical point, so every iterate, we have $\prod_{k} x_{k} \leq 1$.
    - We can prove similar result for $\prod_{k} x_{k} \geq 1$ case with similar definition of $c$-co-balance (Exercise 58).

- **Exercise 59** there are starting point $\mathbf{x}_0$ not critical that does not converge to global minimum. 
    - When $\prod_{k} x_{k} \geq 1$ and $\Delta \leq 0$, then update $x_k' = x_k + \Delta / x_k$ will lead to zero for some large learning rate.

#### Convergence
- **Theorem 9.9** Let $c>1$ and $\delta>0$ such that $\mathbf{x}_0 > 0$ is $c$-balanced with $\delta \leq \prod_{k}\left(\mathbf{x}_{0}\right)_{k}<1$, choosing stepsize $\gamma=\frac{1}{3 d c^{2}}$, then GD satisfies $f\left(\mathbf{x}_{T}\right) \leq\left(1-\frac{\delta^{2}}{3 c^{4}}\right)^{T} f\left(\mathbf{x}_{0}\right)$.
    - **Proof**
        - By sufficient decrease $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{6 d c^{2}}\lVert \nabla f\left(\mathbf{x}_{t}\right)\rVert ^{2}$
        - while $\Vert \nabla f(\mathbf{x})\Vert ^{2}=2 f(\mathbf{x}) \sum_{i=1}^{d}\left(\prod_{k \neq i} x_{k}\right)^{2} \geq  2 f(\mathbf{x}) \frac{d}{c^{2}}\left(\prod_{k} x_{k}\right)^{2-2 / d} \geq  2 f(\mathbf{x}) \frac{d}{c^{2}}\left(\prod_{k} x_{k}\right)^{2}\geq 2 f(\mathbf{x}) \frac{d}{c^{2}} \delta^{2}$
        - then $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{6 d c^{2}} 2 f\left(\mathbf{x}_{t}\right) \frac{d}{c^{2}} \delta^{2}=f\left(\mathbf{x}_{t}\right)\left(1-\frac{\delta^{2}}{3 c^{4}}\right)$
- **Exercise 61** Sequence $\left(\mathbf{x}_{T}\right)_{T \geq 0}$ in above update converge to an optimal solution $\mathbf{x}^{\star}$
    - **Proof** 
        - Since $0<x_i \leq c\left(\prod_{k} x_{k}\right)^{1 / d} \leq c$, sequence is always bounded, then it has a converging subsequence $\{\mathbf{x}_{t_k}\}$.
        - By Young's inequality $(\sum_i a_i)^2 = \sum_{ij} a_i a_j \leq \sum_{ij} \frac{1}{2}(a_i^2 + a_j^2) = n \sum_i a_i^2$, this also fits for vector case, $\Vert \sum_i \mathbf{a}_i\Vert ^2 \leq n\sum_i \Vert \mathbf{a}_i\Vert ^2_2$
        - Let $\mathbf{a}_t = \mathbf{x}_{t_k} - \mathbf{x}_{t}$, then we have $\Vert \mathbf{x}_{t_k} - \mathbf{x}_{T}\Vert ^2 \leq (T-t_k)\sum_{t} \Vert \gamma \nabla f(x_{t})\Vert ^2 \leq C\cdot(T-t_k)\cdot (f(x_{t_k}) - f(x_T))$, $f(x_{t_k}) - f(x_T)$ converge exponentially w.r.t $t_k$, so this term $\Vert \mathbf{x}_{t_k} - \mathbf{x}_{T}\Vert ^2$ converge to zero.
    - We can also prove from the fact of $x_{k,t}$ is monotone....way much easier.
