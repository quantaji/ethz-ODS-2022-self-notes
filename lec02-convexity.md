# Lecture 02 Convexity

## Notation
- ``\|\mathbf{x}\|``: Euclidian norm, ``\ell``-2 norm.
- Cauchy-Schwarz ineq: ``|\mathbf{u}^{\top} \mathbf{v}| \leqslant \|\mathbf{u}\| \cdot \|\mathbf{v}\|``
- spectual norm (2-norm) of matrix ``A\in \mathbb{R}^{m\times n}``: ``\|A\|:=\max _{\mathbf{v} \in \mathbb{R}^{d}, \mathbf{v} \neq 0} \dfrac{\|A \mathbf{v}\|}{\|\mathbf{v}\|}=\max _{\|\mathbf{v}\|=1}\|A \mathbf{v}\|``
    - ``\|A \mathbf{v}\| \leq\|A\|\|\mathbf{v}\|``

## Convex Sets
- **Defnition 2.7** A set ``C \subseteq \mathbb{R}^{d}`` is convex if ``\forall \mathbf{x}, \mathbf{y} \in C``, ``\forall \lambda \in[0,1]``, ``\lambda \mathbf{x}+(1-\lambda) \mathbf{y} \in C``.
- **Observation 2.8 (Intersection)** Let ``C_{i}, i \in I`` be convex sets, then ``C=\bigcap_{i \in I} C_{i}`` is a convex set.
- **Theorem 2.9 (B-Lipschitz equivalence to Derivative)** If (1) ``f: \operatorname{dom}(f) \rightarrow \mathbb{R}^{m}`` is differentiable, (2) ``X \subseteq \operatorname{dom}(f)`` is a non-empety and open convex set, then following equivalent:
    - (1) ``f`` is ``B``-Lipschitz, ``\|f(\mathbf{x})-f(\mathbf{y})\| \leq B\|\mathbf{x}-\mathbf{y}\|, \forall \mathbf{x}, \mathbf{y} \in X``.
    - (2) ``f``'s Jacobian is bounded by ``B`` in spectual norm, ``\|D f(\mathbf{x})\| \leq B,  \forall \mathbf{x} \in X``.
    - Further, if ``X`` not open, then (2) implies (1).
    - **Proof**
        - (1)``\to``(2)
            - By openness, ``\forall \mathbf{x} \in X``, ``\exists l`` s.t. ball ``B(\mathbf{x},l) \in X``,
            - By differentiability,  ``\forall \mathbf{x} \in X, \mathbf{v} \in B(\mathbf{x},l)``, ``f(\mathbf{x+v}) = f(\mathbf{x}) + Df(\mathbf{x}) \mathbf{v}+ r(\mathbf{v})``, where `` \lim_{\|\mathbf{v}\| \to 0}\dfrac{\|r(\mathbf{v})\|}{\|\mathbf{v}\|} = 0``
            - By ``B``-Lipschitz, ``B\|\mathbf{v}\| \geq \|f(\mathbf{x+v}) - f(\mathbf{x})\| = \|Df(\mathbf{x}) \mathbf{v}+ r(\mathbf{v})\| \geq \|Df(\mathbf{x}) \mathbf{v}\| - \|r(\mathbf{v})\|``
            - Therefore``\dfrac{\|Df(\mathbf{x}) \mathbf{v}\|}{\|\mathbf{v}\|} \leq B + \dfrac{\|r(\mathbf{v})\|}{\|\mathbf{v}\|}, \forall \mathbf{v} \in B(\mathbf{x}, l)``,
            - let ``\mathbf{v}`` be the optimal direction where ``\dfrac{\|Df(\mathbf{x}) \mathbf{v}\|}{\|\mathbf{v}\|} = \|Df(\mathbf{x})\|``, and let its magnitude towards zero, then ``\|D f(\mathbf{x})\| \leq B``.
        - (2)``\to``(1), no need to assume open
            - For arbitary ``\mathbf{x}, \mathbf{y} \in X \subseteq \operatorname{dom}(f), \mathbf{x} \neq \mathbf{y}``, define a scalar function for arbitary ``\mathbf{z}``, ``h(t)=\mathbf{z}^{\top} f(\mathbf{x}+t(\mathbf{y}-\mathbf{x}))``, ``h'(t) = \mathbf{z}^{\top} Df(t) \times (\mathbf{y-x})``
            - by mean value theorem, ``\exists c\in (0, 1)`` s.t. ``h'(c) = h(1) - h(0) ``, so ``\mathbf{z}^{\top} Df(c) \times (\mathbf{y-x}) = \mathbf{z}^{\top} (f(\mathbf{y}) - f(\mathbf{x}))``,
            -  By Cauch-Schwarz ineq ``\left\|\mathbf{z}^{\top}(f(\mathbf{y})-f(\mathbf{x}))\right\| = \mathbf{z}^{\top} D f(c)(\mathbf{y}-\mathbf{x}) \leq \|\mathbf{z}\|\|D f(c)(\mathbf{y}-\mathbf{x})\| ``
            - By spec-norm ``\mathsf{RHS} \leq |\mathbf{z}\|\|D f(c)\|\|(\mathbf{y}-\mathbf{x})\| ``
            - By ``B``-boundness of ``\|Df\|``, `` \mathsf{RHS} \leq B\|\mathbf{z}\|\|(\mathbf{y}-\mathbf{x})\|``
            - Taking ``\mathbf{z}=\dfrac{f(\mathbf{y})-f(\mathbf{x})}{\|f(\mathbf{y})-f(\mathbf{x})\|}``, ``\mathsf{LHS} = \|f(\mathbf{y})-f(\mathbf{x})\| \leq \mathsf{RHS} = B\|(\mathbf{y}-\mathbf{x})\|``
            
          
## Convex Functions
- **Definition 2.10** A function ``f``: ``\operatorname{dom}(f) \rightarrow \mathbb{R}`` is *convex* if 
    - (i) ``\operatorname{dom}(f)`` is convex and 
    - (ii) ``\forall \mathbf{x}, \mathbf{y} \in \mathbf{d o m}(f), \lambda \in[0,1]``, ``f(\lambda \mathbf{x}+(1-\lambda) \mathbf{y}) \leq \lambda f(\mathbf{x})+(1-\lambda) f(\mathbf{y})``
- **Definition of** *Epigraph* ``\operatorname{epi}(f):=\left\{(\mathbf{x}, \alpha) \in \mathbb{R}^{d+1}: \mathbf{x} \in \operatorname{dom}(f), \alpha \geq f(\mathbf{x})\right\}``
- **Observation 2.11** ``f`` is a convex function if and only if ``\operatorname{epi}(f)`` is a convex set.
    - **Proof**
        - ``f\to\mathrm{epi}(f)``
            - ``\forall (\mathbf{x}, \alpha), (\mathbf{y}, \beta) \in \mathrm{epi} f``, we know ``\alpha \geq f(\mathrm{x}), \beta \geq f(\mathrm{y})``, 
            - then ``\forall \lambda\in [0,1]``, ``\lambda \alpha + (1-\lambda)\beta \geq \lambda f(\mathrm{x}) + (1-\lambda)f(\mathrm{y}) \geq f(\lambda\mathrm{x} + (1-\lambda)\mathrm{y})``
            - then point ``(\lambda\mathrm{x} + (1-\lambda)\mathrm{y}, \lambda \alpha + (1-\lambda)\beta) \in \mathrm{epi}(f)``
            - ``\mathrm{epi}(f)`` convex
        - ``\mathrm{epi}(f)\to f``
            - let ``\alpha = f(\mathbf{x}), \beta = f(\mathbf{y})``, ``\mathrm{epi}(f)`` convex means points ``(\lambda\mathrm{x} + (1-\lambda)\mathrm{y}, \lambda f(\mathrm{x}) + (1-\lambda)f(\mathrm{y})) \in \mathrm{epi}(f)``,
            - be def of ``\mathrm{epi}(f)``, we have ``\lambda f(\mathrm{x}) + (1-\lambda)f(\mathrm{y})) \geq f(\lambda\mathrm{x} + (1-\lambda)\mathrm{y})``
- **Lemma 2.12 (Jensen’s inequality)** Let ``f: \mathbb{R}^{d} \rightarrow \mathbb{R}`` be a convex function, ``\mathbf{x}_{1}, \ldots, \mathbf{x}_{m} \in \operatorname{dom}(f)``, and ``\lambda_{1}, \ldots, \lambda_{m} \in \mathbb{R}_{+}`` s.t. ``\sum_{i=1}^{m} \lambda_{i}=1``, ``f\left(\sum_{i=1}^{m} \lambda_{i} \mathbf{x}_{i}\right) \leq \sum_{i=1}^{m} \lambda_{i} f\left(\mathbf{x}_{i}\right)``
    - **Proof** Assume this holds for ``m-1``, then ``f\left(\sum_{i=1}^{m} \lambda_{i} \mathbf{x}_{i}\right) = f\left(\sum_{i=3}^{m} \lambda_{i} \mathbf{x}_{i} + (\lambda_1 + \lambda_2)\frac{\lambda_1 \mathbf{x}_1 + \lambda_2 \mathbf{x}_2 }{\lambda_1 + \lambda_2}\right) \leq (\lambda_1 + \lambda_2) f(\frac{\lambda_1 \mathbf{x}_1 + \lambda_2 \mathbf{x}_2 }{\lambda_1 + \lambda_2}) + \sum_{i=3}^{m} \lambda_{i} f(\mathbf{x}_{i}) \leq \sum_{i=1}^{m} \lambda_{i} f(\mathbf{x}_{i}) ``. ``m=2`` holds as normal def of convexity
- **Lemma 2.13 (Convexity and Continuity)** If ``f`` is convex and suppose that ``\mathrm{dom}(f) \subseteq \mathbb{R}^{d}`` is open, then ``f`` is continuous.
    - **Proof** (By hint from Homework)
        - Since ``\mathrm{dom}(f)`` open, ``\forall \mathbf{x}``, we can always find an open ball, and further a closed cube that ``\mathrm{x} \in \otimes_{i=1}^{d} [l_i, r_i]`` and ``\mathbf{x}`` being its center,
        - Any point ``\mathbf{x}'`` in the cube can be written as normalized linear combination of corner. E.g. ``\mathbf{x}' = (x_n, x_{-n}) = \dfrac{r_n - x_n}{r_n - l_n}(l_n, x_{-n}) + \dfrac{x_n - l_n}{r_n - l_n}(r_n, x_{-n})``
            - we can iteratively do this to every coordinate of ``\mathbf{x}'``, until we get a normalized combination of ``2^d`` corner
            - Therefore, ``\forall \mathbf{x}' \in \otimes_{i=1}^{d} [l_i, r_i], f(\mathbf{x}') \leq \sum_{i=1}^{2^d} \lambda_i' f(\mathbf{x}_i) \leq \max_{i} f(\mathbf{x}_i)``, ``\mathbf{x}_i`` is corner.
        - If we do shrinkage over the cube by factor ``\alpha_t``, then each corner ``\mathbf{x}_i`` becomes ``(1-\alpha_t)\mathbf{x} + \alpha_t \mathbf{x}_i``
            - then ``f((1-\alpha_t)\mathbf{x} + \alpha_t \mathbf{x}_i) \leq (1-\alpha_t) f(\mathbf{x} ) + \alpha_t f(\mathbf{x}_i) \to f(\mathbf{x} )`` when ``\alpha_t\to 0``
            - Therefore, we upper bound the value in the cube. ``\forall \epsilon``, ``\exists \alpha_t`` s.t. ``\max_{i} f(\mathbf{x}_{\alpha_t, i}) -f(\mathbf{x}) \leq \epsilon``
        - Now need to lower bound it, if we already have ``\forall \mathbf{x}' \in \otimes_{i=1}^{d} [l_{i, \alpha_t}, r_{i, \alpha_t}]``, ``f(\mathbf{x}')-f(\mathbf{x}) \leq \epsilon``.
            - If lower not bounded, e.g. ``\exists \mathbf{y} \in \otimes_{i=1}^{d} [l_{i, \alpha_t}, r_{i, \alpha_t}]`` s.t. ``f(\mathbf{y}) < f(\mathbf{x}) - \epsilon``,
            - by convexity ``f(\mathbf{x}) \leq (f(\mathbf{y}) + f(2\mathbf{y} - \mathbf{x}))/2``, then ``f(2\mathbf{y} - \mathbf{x}) \geq 2f(\mathbf{x}) - f(\mathbf{y}) > f(\mathbf{x}) + \epsilon``.
            - contradictory, then in the cude, value also lower bounded by ``\epsilon``
        - We can also find a ball inside a cude, therefore ``f`` continuous.
- **Lemma 2.14 (Counter Example in Infinite Dimension)** ``\exists`` vector space ``V`` and linear function ``f`` s.t. ``\forall \mathbf{v}\in V``, ``f`` is discontinuous.
    - **Example** ``V`` be the polynomial function in ``x\in[-1,1]``, distance measured by *supreme norm* ``\|h\|_{\infty}:=\sup _{x \in[-1,1]}|h(x)|``
        - consider the linear function mapping ``p`` to its derivative at ``x=0``, ``f: p(x) \to p'(0)``
        - consider zero polynomial ``p_0(x) \equiv 0``, with ``p_0'(0) = 0``,
        - consider its neighbor ``p_{n, k}(x)=\frac{1}{n} \sum_{i=0}^{k}(-1)^{i} \frac{(n x)^{2 i+1}}{(2 i+1) !}`` which is a finite expansion of ``s_{n}(x)=\frac{1}{n} \sin (n x)=\frac{1}{n} \sum_{i=0}^{\infty}(-1)^{i} \frac{(n x)^{2 i+1}}{(2 i+1) !}``
        - since ``\left\|p_{n, k}-s_{n}\right\|_{\infty} \rightarrow 0`` as ``k\to\infty`` and ``\left\|s_{n}\right\|_{\infty} \rightarrow 0`` as ``n\to\infty``, this means ``\left\|p_{n, k}\right\| \rightarrow 0 `` as ``n, k \rightarrow \infty``, on the other hand ``f\left(p_{n, k}\right)=p_{n, k}^{\prime}(0)=1``

## Convexity Characterization
### First Order

- **Lemma 2.15** Suppose ``\mathrm{dom}(f)`` open, ``f`` differentiable and ``\forall \mathbf{x}``m ``\nabla f(\mathbf{x})`` exists, then ``f`` is convex iff  ``\mathrm{dom}(f)`` convex, and ``f(\mathbf{y}) \geq f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})``
    - **Proof**
        - (``\rightarrow``), by convexity, for ``t\in(0,1)``, ``f(\mathbf{x} + t(\mathbf{y-x}))\leq f(\mathbf{x})+t(f(\mathbf{y})-f(\mathbf{x}))``
            - ``f(\mathbf{y}) \geq f(\mathbf{x})+\frac{f(\mathbf{x}+t(\mathbf{y}-\mathbf{x}))-f(\mathbf{x})}{t} = f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})+\frac{r(t(\mathbf{y}-\mathbf{x}))}{t}``
            - Taking ``t\to 0`` we have  ``f(\mathbf{y}) \geq f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})``
        - (``\leftarrow)``, for ``\mathbf{z}:=\lambda \mathbf{x}+(1-\lambda) \mathbf{y} \in \operatorname{dom}(f)`` we have ``f(\mathbf{x}) \geq f(\mathbf{z})+\nabla f(\mathbf{z})^{\top}(\mathbf{x}-\mathbf{z})``, and ``f(\mathbf{y}) \geq f(\mathbf{z})+\nabla f(\mathbf{z})^{\top}(\mathbf{y}-\mathbf{z})``
            - Weighted addition of two ineqs by factor ``\{\lambda, 1-\lambda\}``, we get convexity ``\lambda f(\mathbf{x})+(1-\lambda) f(\mathbf{y}) \geq f(\mathbf{z})=f(\lambda \mathbf{x}+(1-\lambda) \mathbf{y})``
- **Lemma 2.16 (monotonicity of the gradient)** Suppose ``\mathrm{dom}(f)`` open, ``f`` differentiable. Then ``f`` is convex iff if dom(f) is convex and ``\forall \mathbf{x,y} \in \mathrm{dom}(f),(\nabla f(\mathbf{y})-\nabla f(\mathbf{x}))^{\top}(\mathbf{y}-\mathbf{x}) \geq 0``
    - **Proof**
        - (``\rightarrow``) If ``f`` convex, apply **2.15** to ``\mathbf{x, y}``, and get ``f(\mathbf{y}) \geq f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})`` and ``f(\mathbf{x}) \geq f(\mathbf{y})+\nabla f(\mathbf{y})^{\top}(\mathbf{x}-\mathbf{y})``
            - add them togethor and get ``0 \geq (\nabla f(\mathbf{y})-\nabla f(\mathbf{x}))^{\top}(\mathbf{x}-\mathbf{y})``
        - (``\leftarrow``) Denote scalar function ``h(t) := f(\mathbf{x} + t(\mathbf{y-x})), t\in[0,1]``, 
            - so ``h'(t) = \nabla f(\mathbf{x} + t(\mathbf{y-x}))^{\top}(\mathbf{y-x})``
            - setting ``\mathbf{y} \leftarrow \mathbf{x} + t(\mathbf{y-x})`` in monotonicity inequality, we have ``h'(t) \geq h(0) \forall t\in[0,1]``
            - By mean value theorem, ``\exists c`` s.t. ``f(\mathbf{y}) = h(1) = h(0) + h'(c) \geq h(0) + h'(0) = f(\mathbf{x}) + \nabla f(\mathbf{y})^{\top}(\mathbf{y}-\mathbf{x})``, convexity


            
### Second Order 