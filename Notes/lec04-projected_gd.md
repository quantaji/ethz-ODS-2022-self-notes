# Projected Gradient Descent

## Algorithm
- Gradient Step: $\mathbf{y}_{t+1}:=\mathbf{x}_{t}-\gamma \nabla f\left(\mathbf{x}_{t}\right)$
- Projection Step: $\mathbf{x}_{t+1}:=\Pi_{X}\left(\mathbf{y}_{t+1}\right):=\underset{\mathbf{x} \in X}{\operatorname{argmin}}\lVert\mathbf{x}-\mathbf{y}_{t+1}\rVert^{2}$
  - We assume the minimization in projection step is easy to solve.
  - $d_{\mathbf{y}}(\mathbf{x}):=\lVert \mathbf{x}-\mathbf{y} \rVert^{2}$ is strongly convex -> projection unique.
- **Fact 4.1** Let $X \subseteq \mathbb{R}^{d}$ be closed and convex, $\mathbf{x} \in X, \mathbf{y} \in \mathbb{R}^{d}$. Then
  - (i) $\left(\mathbf{x}-\Pi_{X}(\mathbf{y})\right)^{\top}\left(\mathbf{y}-\Pi_{X}(\mathbf{y})\right) \leq 0$
  - (ii) $\lVert\mathbf{x}-\Pi_{X}(\mathbf{y})\rVert^{2}+\lVert\mathbf{y}-\Pi_{X}(\mathbf{y})\rVert^{2} \leq\Vert \mathbf{x}-\mathbf{y}\Vert ^{2}$ (*Note this is square of distance*)
  - **Proof**
    - (i) $\nabla d_{\mathbf{y}}(\mathbf{x}) = \mathbf{x - y}$, With Lemma 2.27 $\forall \mathbf{x} \in X, \nabla f\left(\mathbf{x}^{\star}\right)^{\top}\left(\mathbf{x}-\mathbf{x}^{\star}\right) \geq 0$ (also holds for closed set) and $\Pi_{X}(\mathbf{y})$ as minimum, we get $(\Pi_{X}(\mathbf{y}) - \mathbf{y})^{\top}(\mathbf{x} - \Pi_{X}(\mathbf{y}))\geq 0$.
    - (i) -> (ii) By $2 \mathbf{v}^{\top} \mathbf{w}=\Vert \mathbf{v}\Vert ^{2}+\Vert \mathbf{w}\Vert ^{2}-\Vert \mathbf{v}-\mathbf{w}\Vert ^{2}$.
- **Lemma (Ex 31)** If $\mathbf{x}_{t+1} = \mathbf{x}_t$, then $\mathbf{x}_t$ is minimizer.
    - **Proof**
        - Let $\mathbf{y} \leftarrow \mathbf{x}_{t} - \gamma \mathbf{g}_{t}$ we have $\Pi_X(\mathbf{y}) := \mathbf{x}_{t+1} = \Pi_X(\mathbf{x}_{t} - \gamma \mathbf{g}_{t}) = \mathbf{x}_t$.
        - By (i) $\left(\mathbf{x}-\Pi_{X}(\mathbf{y})\right)^{\top}\left(\mathbf{y}-\Pi_{X}(\mathbf{y})\right) = \left(\mathbf{x}-\mathbf{x}_t \right)^{\top}\left(-\gamma \mathbf{g}_{t}\right) \leq 0 \Leftrightarrow \nabla f(\mathbf{x}_t)^{\top} \left(\mathbf{x}-\mathbf{x}_t \right) \geq 0$ -> Lemma 2.27 -> minimizer.

## Bounded gradients: $\mathcal{O}\left(1 / \varepsilon^{2}\right)$ steps (SAME)
- **Theorem 4.2** Same as unbounded case
    - **Proof**
        - Difference only in gradient step, original procedure gives $\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)=\frac{1}{2 \gamma}\left(\gamma^{2}\lVert\mathbf{g}_{t}\rVert^{2}+\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}^{\star}\rVert^{2}\right)$
        - But we need $\mathbf{x}_{t+1}$ instead of $\mathbf{y}_{t+1}$. By fact 4.1 (ii) setting $\mathbf{x}=\mathbf{x}^{\star}, \mathbf{y}=\mathbf{y}_{t+1}$, we get $\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2} \leq \Vert  \mathbf{y}_{t+1}-\mathbf{x}^{\star} \Vert ^{2}$
            - so we have $\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right) \leq \frac{1}{2 \gamma}\left(\gamma^{2}\lVert\mathbf{g}_{t}\rVert^{2}+\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2}\right)$

## Smooth convex function: $\mathcal{O}\left(1 / \varepsilon\right)$ steps (SAME)
- **Lemma 4.3 (Sufficient descent under constraint)** For $L$-smooth covnex function $f$, a step size of $\gamma = L^{-1}$ gives sufficient descent of $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{1}{2 L}\lVert\nabla f\left(\mathbf{x}_{t}\right)\rVert^{2}+\frac{L}{2}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$.
    - **Proof**
        - $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)+\nabla f\left(\mathbf{x}_{t}\right)^{\top}\left(\mathbf{x}_{t+1}-\mathbf{x}_{t}\right)+\frac{L}{2}\lVert\mathbf{x}_{t}-\mathbf{x}_{t+1}\rVert^{2} = f\left(\mathbf{x}_{t}\right)-L\left(\mathbf{y}_{t+1}-\mathbf{x}_{t}\right)^{\top}\left(\mathbf{x}_{t+1}-\mathbf{x}_{t}\right)+\frac{L}{2}\lVert\mathbf{x}_{t}-\mathbf{x}_{t+1}\rVert^{2}$
        - Use $2 \mathbf{v}^{\top} \mathbf{w}=\Vert \mathbf{v}\Vert ^{2}+\Vert \mathbf{w}\Vert ^{2}-\Vert \mathbf{v}-\mathbf{w}\Vert ^{2}$ on term $\left(\mathbf{y}_{t+1}-\mathbf{x}_{t}\right)^{\top}\left(\mathbf{x}_{t+1}-\mathbf{x}_{t}\right)$ we get
        - $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)-\frac{L}{2}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t}\rVert^{2}+\frac{L}{2}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$ 
        - Then we arrive at our destination
    - **PS (Ex 32):** Since $\mathbf{x}_{t+1}$ is the minimzer of distance to $\mathbf{y}_{t+1}$, we have $\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t}\rVert^{2} \geq \lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$, therefore from the last inequality, $f\left(\mathbf{x}_{t+1}\right) \leq f\left(\mathbf{x}_{t}\right)$

        
- **Lemma 4.4 (Error Bound)** $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{L}{2 T}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert^{2}$ (*same as unbounded case*).
    - **Proof**
        - Since we have an additional term $\frac{1}{2 L}\lVert\nabla f\left(\mathbf{x}_{t}\right)\rVert^{2} \leq f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}_{t+1}\right)+\frac{L}{2}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$ we have to find some compensate in GD algorithm.
        - We have $\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)=\frac{1}{2 \gamma}\left(\gamma^{2}\lVert\mathbf{g}_{t}\rVert^{2}+\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}^{\star}\rVert^{2}\right)$, 
            - since $\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2}+\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2} \leq\lVert\mathbf{y}_{t+1}-\mathbf{x}^{\star}\rVert^{2}$, we can upper bound it by 
            - $\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right) \leq \frac{1}{2 \gamma}\left(\gamma^{2}\lVert\mathbf{g}_{t}\rVert^{2}+\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}\right)$
        - with convexity and sum over all $t$, we get $\sum_{t=0}^{T-1}\left(f\left(\mathbf{x}_{t}\right)-f\left(\mathbf{x}^{\star}\right)\right) \leq \sum_{t=0}^{T-1} \mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right) \leq \frac{1}{2 L} \sum_{t=0}^{T-1}\lVert\mathbf{g}_{t}\rVert^{2}+\frac{L}{2}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert^{2}-\frac{L}{2} \sum_{t=0}^{T-1}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$
        - with new version of sufficient decrease we have $\frac{1}{2 L} \sum_{t=0}^{T-1}\lVert\mathbf{g}_{t}\rVert^{2}=f\left(\mathbf{x}_{0}\right)-f\left(\mathbf{x}_{T}\right)+\frac{L}{2} \sum_{t=0}^{T-1}\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$
        - then we can prove the claim.

## Smooth and strongly convex $f$: $\mathcal{O}(\log (1 / \varepsilon))$ steps (SAME)
- **Theorem 4.5** (*similar to theorem 4.3*)
    - (i) Geometric decrease for $\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}$, $\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2} \leq\left(1-\frac{\mu}{L}\right)\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}$
    - (ii) Exponential decrease for absoulute error $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \frac{L}{2}\left(1-\frac{\mu}{L}\right)^{T}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert^{2} + \lVert\nabla f\left(\mathbf{x}^{\star}\right)\rVert\left(1-\frac{\mu}{L}\right)^{T / 2}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert$
    - **Proof**
        - with strong convexity, we can bound gradient to $\mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right) \leq \frac{1}{2 \gamma}\left(\gamma^{2}\lVert\nabla f\left(\mathbf{x}_{t}\right)\rVert^{2}+\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}\right)-\frac{\mu}{2}\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2}$
        - with convexity $f(\mathbf{x}_t) - f(\mathbf{x}^{\star}) \leq \mathbf{g}_{t}^{\top}\left(\mathbf{x}_{t}-\mathbf{x}^{\star}\right)$ we can bound on $\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2}$
        - $\lVert\mathbf{x}_{t+1}-\mathbf{x}^{\star}\rVert^{2} \leq (1-\mu \gamma)\lVert\mathbf{x}_{t}-\mathbf{x}^{\star}\rVert^{2} + 2 \gamma\left(f\left(\mathbf{x}^{\star}\right)-f\left(\mathbf{x}_{t}\right)\right)+\gamma^{2}\lVert\nabla f\left(\mathbf{x}_{t}\right)\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2}$ (*geometric decrease with some noise*)
            - The additional term is bound to be non-positive by the adapted version of sufficient descent (Lemma 4.3) $\frac{2}{L}\left(f\left(\mathbf{x}^{\star}\right)-f\left(\mathbf{x}_{t}\right)\right)+\frac{1}{L^{2}}\lVert\nabla f\left(\mathbf{x}_{t}\right)\rVert^{2}-\lVert\mathbf{y}_{t+1}-\mathbf{x}_{t+1}\rVert^{2} \leq 0 .$
            - Then we get (i)
        - (ii) is attained by smoothness, but the gradient term does not vanish, $f\left(\mathbf{x}_{T}\right)-f\left(\mathbf{x}^{\star}\right) \leq \nabla f\left(\mathbf{x}^{\star}\right)^{\top}\left(\mathbf{x}_{T}-\mathbf{x}^{\star}\right)+\frac{L}{2}\lVert\mathbf{x}^{\star}-\mathbf{x}_{T}\rVert^{2} \leq\lVert\nabla f\left(\mathbf{x}^{\star}\right)\rVert\lVert\mathbf{x}_{T}-\mathbf{x}^{\star}\rVert+\frac{L}{2}\lVert\mathbf{x}^{\star}-\mathbf{x}_{T}\rVert^{2}\leq\lVert\nabla f\left(\mathbf{x}^{\star}\right)\rVert\left(1-\frac{\mu}{L}\right)^{T / 2}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert+\frac{L}{2}\left(1-\frac{\mu}{L}\right)^{T}\lVert\mathbf{x}_{0}-\mathbf{x}^{\star}\rVert^{2}$
