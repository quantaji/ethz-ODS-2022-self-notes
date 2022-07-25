## Speed metric
- ``\lim _{k \rightarrow \infty} \frac{f\left(w^{k+1}\right)-f\left(w^{*}\right)}{f\left(w^{k}\right)-f\left(w^{*}\right)}=\rho``
    - ``\rho=1`` sublinear rate
    - ``\rho\in(0,1)`` linear rate
    - ``\rho = 0``super linear reate

## Vanilla GD
- Update: ``\mathbf{x}_{t+1}:=\mathbf{x}_{t}-\gamma \mathbf{g}_{t}``, `` \mathbf{g}_{t}:=\nabla f\left(\mathbf{x}_{t}\right)``
- Consider quantity ``\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)=\frac{1}{\gamma}\left(\mathbf{x}_{t}-\mathbf{x}_{t+1}\right)^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right) = \frac{1}{2 \gamma}\left(\left\|\mathbf{x}_{t}-\mathbf{x}_{t+1}\right\|^{2}+\left\|\mathbf{x}_{t}-\mathbf{x}^{\star}\right\|^{2}-\left\|\mathbf{x}_{t+1}-\mathbf{x}^{\star}\right\|^{2}\right) = \frac{\gamma}{2}\left\|\mathbf{g}_{t}\right\|^{2}+\frac{1}{2 \gamma}\left(\left\|\mathbf{x}_{t}-\mathbf{x}^{\star}\right\|^{2}-\left\|\mathbf{x}_{t+1}-\mathbf{x}^{\star}\right\|^{2}\right)``
- sum over ``t=0,\ldots,T-1``, we have ``\sum_{t=0}^{T-1} \mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)=\frac{\gamma}{2} \sum_{t=0}^{T-1}\left\|\mathbf{g}_{t}\right\|^{2}+\frac{1}{2 \gamma}\left(\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}-\left\|\mathbf{x}_{T}-\mathbf{x}^{\star}\right\|^{2}\right) \leq =\frac{\gamma}{2} \sum_{t=0}^{T-1}\left\|\mathbf{g}_{t}\right\|^{2}+\frac{1}{2 \gamma}\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}``
- By convexity ``f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right) \leq \mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)``, we have ``\sum_{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \frac{\gamma}{2} \sum_{t=0}^{T-1}\left\|\mathbf{g}_{t}\right\|^{2}+\frac{1}{2 \gamma}\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}``
    - This gives upper bound on average error ``\mathbb{E}_{t} [f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)]``
    - note that last iterate is not necessarily the best one. Some algorithms guarantee last iterate convergence.
    - Bad result since we cannot control ``\|\mathbf{g}_t\|``

## Improvement Case 1: Lipschitz ``f``: ``\mathcal{O}\left(1 / \varepsilon^{2}\right)`` Steps
- By Theorem 2.9, if we assume convex function ``f`` is ``B``-Lipschitz, then ``\|D f(x)\| \leq B``, the spectual norm in Thm 2.9 is Euclidian norm when ``D f \in \mathbb{R}^{1\times d}``
    - then ``\mathbf{g}_t \leq B`` -> Theorem 3.1
- **Theorem 3.1** Suppose ``f \in C^1(x)`` convex, with global minimum ``\mathbf{x}^{\star}``. If (1) ``\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\| \leq R`` (2) ``\forall \mathbf{x}, \|\nabla f(\mathbf{x})\| \leq B``, when with step size ``\gamma:=\frac{R}{B \sqrt{T}}``, we have average error bounded by ``\frac{1}{T} \sum_{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \frac{R B}{\sqrt{T}}``
    - **Proof** Straighforward from vanilla case, ``\sum_{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \frac{\gamma}{2} B^{2} T+\frac{1}{2 \gamma} R^{2}``
    - **Time Complexity** To achieve ``\min _{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \varepsilon``, we need ``T \geq \frac{R^{2} B^{2}}{\varepsilon^{2}}`` iterations, ``\mathcal{O}\left(1 / \varepsilon^{2}\right)``.
    - but no dependence on dimension ``d``.

## Improvement Case 2: Smooth Convex ``f``: ``\mathcal{O}\left(1 / \varepsilon\right)`` Steps
- **Lemma 3.7 (Sufficient Descent)** Suppose ``f: \mathbb{R}^{d} \rightarrow \mathbb{R} \in C^{1}`` is ``L``-smooth. Setting ``\gamma := L^{-1}``, then gradient descent gives ``f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L}\left\|\nabla f\left(\mathbf{x}_{t}\right)\right\|^{2}``. (**Proof** Straightforward)
- **Lemma 3.8 (Last Iteration Bound for smooth convex funciton)** Suppose ``f: \mathbb{R}^{d} \rightarrow \mathbb{R} \in C^{1}`` is ``L``-smooth with global minimum ``\mathbf{x}^{\star}``. With step size ``\gamma := L^{-1}`` gradient descent gives ``f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{L}{2 T}\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}``.
    - **Proof** 
        - Sum *sufficient descent* over ``t=0:T-1``, we get ``\frac{1}{2 L} \sum_{t=0}^{T-1}\left\|\nabla f\left(\mathbf{x}_{t}\right)\right\|^{2} \leq \sum_{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}_{t+1}\right)\right)=f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}_{T}\right)``
        - Take this into vanilla GD bound, we get ``\sum_{t=1}^{T}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \frac{L}{2}\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}``.
        - By sufficient descent, monotocity of ``f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)`` is guaranteed, so ``f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{1}{T} \sum_{t=1}^{T}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \frac{L}{2 T}\left\|\mathbf{x}_{0}-\mathbf{x}^{\star}\right\|^{2}``
    - **Time Complexity** ``T \geq \frac{R^{2} L}{2 \varepsilon} = \mathcal{O}\left(1 / \varepsilon\right)``

## Improvement Case 3: Acceleration for smooth ``f``: ``\mathcal{O}(1 / \sqrt{\varepsilon})`` Steps

## Improvement Case 4: Smooth and Stongly convex ``f``: ``\mathcal{O}(\log (1 / \varepsilon))`` Steps, linear rate