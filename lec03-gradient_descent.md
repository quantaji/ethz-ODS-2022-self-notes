# lecture 03 Gradient descent
- We assume existance of minimizer ``\mathbf{x}^{\star}``.
- The goal is to find approximation ``\mathbf{x}``, s.t. ``f(\mathbf{x})-f\left(\mathbf{x}^{\star}\right)<\varepsilon``.
    - no need for ``\|\mathbf{x} - \mathbf{x}^{\star}\|`` to be close

### Notation on Norm
- ``\|\cdot\|_{a*}`` is the dual norm, ``\|u\|_{a*}=\sup _{v \neq 0} \frac{u^{T} v}{\|v\|_a}=\sup _{\|v\|_a=1} u^{T} v``
   
 - This implies generalized Cauchy-Schwarz inequality ``\left|u^{T} v\right| \leq\|u\|_{a*}\|v\|_a, \forall u,v``.
- For Eculidiean norm ``\|\cdot\|=\|\cdot\|_{*}=\|\cdot\|_{2}``
- the parameter ``L`` depends on choice of norm
- ``\|x\|_{2} \leq\|x\|_{1} \leq \sqrt{n}\|x\|_{2}``, ``\frac{1}{\sqrt{n}}\|x\|_{2} \leq\|x\|_{\infty} \leq\|x\|_{2}``.

## Smoothness and Lipschitz
### Lipschitz
- **Definition (Lipschitz)** The gradient of ``f`` is *Lipschitz continuous* with parameter ``L > 0`` w.r.t. norm ``\|\cdot\|_a``, if ``\|\nabla f(x)-\nabla f(y)\|_{a*} \leq L\|x-y\|_a``.
- **Lemma A (generalized Cauchy–Schwarz inequality)** ``\nabla f`` is ``L``-Lipschitz ``\Rightarrow`` ``\forall \mathbf{x,y}\in\mathbf{dom}(f)``, ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \leq L\|x-y\|^{2}_{a}``
    - **Proof**
        - ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \leq \|\nabla f(x)-\nabla f(y)\|_{a*}\|x-y\|_{a} \leq L\|x-y\|_a^2``
    
### Smoothness
- **Definition 3.2 (Smoothness)** Let ``f: \operatorname{dom}(f) \rightarrow \mathbb{R}`` be a differentiable function, ``X \subset \operatorname{dom}(f)`` convex and ``L \in \mathbb{R}_{+}``. Function ``f`` is called *smooth with parameter ``L``* over ``X`` (over norm ``\|\cdot\|_a``) if ``f(\mathbf{y}) \leq f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})+\frac{L}{2}\|\mathbf{x}-\mathbf{y}\|_{a}^{2}``
    - PS: **NO** need to assume ``f`` to be convex
- **Lemma 3.3 (equivalance of smoothness in 2-norm)** ``f`` is ``L``-smooth over 2-norm  ``\Leftrightarrow`` ``g(\mathbf{x})=\frac{L}{2} \mathbf{x}^{\top} \mathbf{x}-f(\mathbf{x})`` is convex over ``\operatorname{dom}(f)``.
    - **Proof** 
        - ``\nabla g(\mathbf{x}) = L\mathbf{x} - \nabla f(\mathbf{x})``
        - ``g`` convex ``\Leftrightarrow`` ``\forall \mathbf{x,y}, g(\mathbf{y}) \geq g(\mathbf{x}) + \nabla g(\mathbf{x})^{\top}(\mathbf{y-x})`` ``\Leftrightarrow`` ``\frac{L}{2} \mathbf{y}^{\top} \mathbf{y}-f(\mathbf{y}) \geq \frac{L}{2} \mathbf{x}^{\top} \mathbf{x}-f(\mathbf{x}) + \left[ L\mathbf{x} - \nabla f(\mathbf{x}) \right]^{\top}(\mathbf{y-x})`` ``\Leftrightarrow`` ``f(\mathbf{y}) \leq f(\mathbf{x})+\nabla f(\mathbf{x})^{\top}(\mathbf{y}-\mathbf{x})+\frac{L}{2}\|\mathbf{x}-\mathbf{y}\|_{2}^{2}``
    - If ``f`` twice differentiable, Hessian of ``g`` is ``LI-\nabla^2 f \succeq 0``, this means ``\forall x, \lambda_{\max }\left(\nabla^{2} f(x)\right) \leq L``.
- **Lemma B (quadratic upper bound)** If ``\operatorname{dom} f=\mathbf{R}^{n}`` and ``f`` is ``L``-smooth over norm ``a``, and has minimizer ``\mathbf{x}^{\star}``, then ``\forall z``, ``\frac{1}{2 L}\|\nabla f(z)\|_{a*}^{2} \leq f(z)-f\left(x^{\star}\right) \leq \frac{L}{2}\left\|z-x^{\star}\right\|_{a}^{2}``
    - **Proof**
        - Right-hand inequality by smoothness setting ``y=z, x=x^{\star}``, ``f(\mathbf{z}) \leq f(\mathbf{x}^{\star})+ {\underbrace{\nabla f(\mathbf{x}^{\star})}_{=0}}^{\top}(\mathbf{z}-\mathbf{x}^{\star})+\frac{L}{2}\|\mathbf{x}^{\star}-\mathbf{z}\|_{a}^{2}``
        - Left-hand ineuqality, by smoothness ``\inf _{y} f(y) \leq \inf _{y}\left(f(z)+\nabla f(z)^{T}(y-z)+\frac{L}{2}\|y-z\|^{2}\right)``
            - by seperation of direction and magnitude ``\mathsf{LHS }\leq =\inf _{\|v\|=1} \inf _{t}\left(f(z)+t \nabla f(z)^{T} v+\frac{L t^{2}}{2}\right) = \inf _{\|v\|=1}\left(f(z)-\frac{1}{2 L}\left(\nabla f(z)^{T} v\right)^{2}\right)`` by maximum of quadratic function.
            - By the definition of dual norm, we have ``\inf _{y} f(y) = f(z^{\star}) \leq f(z)-\frac{1}{2 L}\|\nabla f(z)\|_{*}^{2}``

- **Lemma 3.4 (quadratic function)** quadratic form ``f(\mathbf{x})=\mathbf{x}^{\top} Q \mathbf{x}+\mathbf{b}^{\top} \mathbf{x}+c`` is ``2\|Q\|_{\mathrm{spec}}``-smooth over 2-norm.
- Example of Non-Smooth func: ``f(x) = x^4`` since ``\forall L, y^{4} \leq \frac{L}{2} y^{2}`` does not hold for all ``y`` near ``x=0``.

- **Lemma 3.6 (Operation that preserves smoothness)** 
    - (i) *Linear combination* ``f:=\sum_{i=1}^{m} \lambda_{i} f_{i}``, where ``\lambda_i > 0``.
    - (ii) *Affine Transform* ``f(A \mathbf{x}+\mathbf{b}))``.
    - **Proof** Straightforward

### Equivalence of Smoothness and Lipschitz under Convexity
- **Definition (co-coercivity)** The property of *co-coercivity* of ``\nabla f`` with parameter ``L`` over norm ``a`` is ``\forall x,y``, ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \geq \frac{1}{L}\|\nabla f(x)-\nabla f(y)\|_{*}^{2}``
- **Lemma 3.5 (Equivalence of Smoothness and Lipschitz under convexity)** If (i) ``\mathbf{dom}(f) = \mathbb{R}^{d}`` (ii) ``f`` convex and differentiable. Then ``f`` is smooth with parameter ``L`` under norm ``a`` ``\Leftrightarrow`` ``\nabla f`` is Lipschitz continuous with parameter ``L`` under norm ``a``.
    - *Proof Route* **Lipschitzness** -(*Lemma A*)-> **Smoothness** -(*Lemma B*)-> **Co-coercivity** -> **Lipschitzness**
    - **Proof**
        - **Lipschitzness** -(*Lemma A*)-> **Smoothness**
            - Define ``g(t\in[0,1]) = f(x+ t(y-x))``, (well-defined because of convexity of ``\mathbf{dom}(f)``)
            - ``g^{\prime}(t)-g^{\prime}(0)=(\nabla f(x+t(y-x))-\nabla f(x))^{T}(y-x)``
            - Since ``f`` ``L``-Lipschitz, by Lemma A, we have  ``\forall \mathbf{x,y}\in\mathbf{dom}(f)``, ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \leq L\|x-y\|^{2}_{a}`` 
                - so that ``g^{\prime}(t)-g^{\prime}(0) \leq t L \|x-y\|_{a}^2``
            - ``f(y)=g(1)=g(0)+\int_{0}^{1} g^{\prime}(t) d t \leq g(0)+g^{\prime}(0)+\frac{L}{2}\|x-y\|^{2} =f(x)+\nabla f(x)^{T}(y-x)+\frac{L}{2}\|x-y\|^{2}``
            - We have smoothness
        - **Smoothness** -(*Lemma B*)-> **Co-coercivity**
            - Define two function ``f_{x}(z)=f(z)-\nabla f(x)^{T} z, f_{y}(z)=f(z)-\nabla f(y)^{T} z``
            - Since we only add first-order term, ``f_x{z}, f_y(z)`` are still convex.
            - Since ``\nabla f_x(z_1) - \nabla f_x(z_2) = \nabla f(z_1) - \nabla f(z_2)``, ``f_x{z}, f_y(z)`` are still ``L``-smooth.
            - Also ``\nabla f_x(z=x) = 0``, by convexity, it reaches it minimum, using Smoothness and left-hand inequality of Lemma B, and taking ``f = f_x, z=y``, we get ``f(y)-f(x)-\nabla f(x)^{T}(y-x) \geq \frac{1}{2 L}\|\nabla f(y)-\nabla f(x)\|_{a*}^{2}``
            - Similarly ``f(x)-f(y)-\nabla f(y)^{T}(x-y) \geq \frac{1}{2 L}\|\nabla f(y)-\nabla f(x)\|_{a*}^{2}``
            - Adding these together and we get ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \geq \frac{1}{L}\|\nabla f(x)-\nabla f(y)\|_{a*}^{2}``, co-coercivity
        - **Co-coercivity** -> **Lipschitzness**
            - ``\frac{1}{L}\|\nabla f(x)-\nabla f(y)\|_{a*}^{2} \leq (\nabla f(x)-\nabla f(y))^{T}(x-y) \leq \|\nabla f(x)-\nabla f(y)\|_{a*}\cdot \|x-y\|_{a}``
            - ``\Rightarrow \|\nabla f(x)-\nabla f(y)\|_{a*} \leq L  \|x-y\|_{a}``

## Strong convexity
- **Definition and Lemma C (Strong Convexity)** A differentiable function ``f`` is strongly convex with parameter ``\mu`` over norm ``\|\cdot\|_{a}`` if ``\mathbf{dom}(f)`` is convex and (for following are equivalent)
    - (i) ``f(y) \geq f(x)+\nabla f(x)^{T}(y-x)+\frac{\mu}{2}\|y-x\|_{a}^{2}, \forall x, y`` (This is also used in text book)
    - (ii) ``f(\theta x+(1-\theta) y) \leq \theta f(x)+(1-\theta) f(y)-\frac{\mu}{2} \theta(1-\theta)\|x-y\|_{a}^{2}``, ``\theta \in [0,1]``
    - (iii) ``\forall x,y\in\mathbf{dom}(f), g(t) :=f(x+t(y-x))-\frac{m}{2} t^{2}\|x-y\|^{2}`` is convex over ``t``
    - (iiii) ``(\nabla f(x)-\nabla f(y))^{T}(x-y) \geq \mu\|x-y\|_{a}^{2}`` (also known as *strong monotonicity* or *coercivity* of ``\nabla f``)
    - **Proof of Equivalence**
        - (i) -> (ii)
            - set ``y=\theta x+(1-\theta) y, x= x`` and ``y=\theta x+(1-\theta) y, x= y``, we get 
                - (a) ``f(\theta x+(1-\theta) y) \geq f(x)+(1-\theta)\nabla f(x)^{T}(y-x)+(1-\theta)^2\frac{\mu}{2}\|y-x\|_{a}^{2}``  and 
                - (b) ``f(\theta x+(1-\theta) y) \geq f(y)-\theta\nabla f(y)^{T}(y-x)+\theta^2\frac{\mu}{2}\|y-x\|_{a}^{2}`` 
            - ``\theta\times``(a)``+(1-\theta)\times``(b) we get (ii)
        - (ii) -> (iii)
            - Consider function ``h(t) := f(x + t(y-x))``, by (ii) we have ``h(\theta t_1 + (1-\theta) t_2) \leq \theta h(t_1) + (1-\theta)h(t_2) - \frac{\mu}{2} \theta (1-\theta) (t_1-t_2)^2 \|y-x\|_{a}^{2}``
            - Since ``\theta (1-\theta) (t_1-t_2)^2 = \theta t_1 ^2 + (1-\theta) t_2^2 - (\theta t_1 + (1-\theta) t_2)^2``, we have equivalently
                - ``h(\theta t_1 + (1-\theta) t_2  \frac{\mu}{2} \|y-x\|_{a}^{2}) - (\theta t_1 + (1-\theta) t_2)^2\leq \theta (h(t_1)- t_1 ^2 \frac{\mu}{2} \|y-x\|_{a}^{2}) + (1-\theta)(h(t_2)-t_2^2\frac{\mu}{2} \|y-x\|_{a}^{2})``
            - or equivalently, define ``g(t):= h(t) - t^2\frac{\mu}{2} \|y-x\|_{a}^{2}``, ``g`` is a convex function (along this domain of finite line between ``x,y``)
                - if ``f`` differentiable, then ``g`` also
        - PS: (iii) -> (ii) straightforward by setting ``t=\theta``, and compare with ``t=0,1``.
        - (iii) -> (i)
            - The first order condition of ``g`` convex gives ``g(1) \geq g(0) + g'(0)``
                - ``\Rightarrow f(y) - \frac{\mu}{2} \|y-x\|_{a}^{2} \geq f(x) + \nabla f(x)^{\top}(y-x) - \mu \|y-x\|_{a}^{2}``, this is (i)
        - (i) -> (iiii) switch between ``x,y`` and add them together.
        - (iiii) -> (iii)
            - let ``x=x, y=x+t(y-x)`` in (iiii), and we have ``(\nabla f(x + t (y-x) )-\nabla f(x))^{T}(y-x) \geq \mu t \|x-y\|^{2}``
            - now we compute ``g'(t)`` in (iii), we have ``g'(t) = \nabla f(x + t(y-x))^{\top}(y-x) - \mu t \|y-x\|_{a}^2``
            - assume ``t_1 \geq t_2``, then ``g(t_1) - g(t_2) = \nabla (f(x + t_1(y-x)) - f(x + t_2(y-x)))^{\top}(y-x) - \mu (t_1 - t_2) \|y-x\|_{a}^2 \geq \mu (t1 - t2) \|x-y\|_{a}^2 - \mu (t_1 - t_2) \|y-x\|_{a}^2 = 0``
            - ``g(t)`` nondecreasing, convex -> (iii).

- **Lemma D (boundedness and global minimum)** If ``f`` is differentiable and ``\mu``-strongly convex, then ``f`` have bounded sublevel.
    - **Proof** 
        - Since ``f`` ``\mu``-strongly convex, given any ``x,y``, value along this line will go to ``+\infty`` by quadratic function.
        - suppose ``f^{\leq \alpha}`` non-empty, since ``f``convex -> continuous, then ``\exists y`` s.t. ``f(y) = \alpha``.
        - Then  ``\forall x, f(x) \geq f(y) + \nabla f(y)^{\top} (y-x) + \frac{\mu}{2}\|y-x\|^2_{a} \geq f(y) - \|\nabla f(y)\|_{a*}\|y-x\| + \frac{\mu}{2}\|y-x\|^2_{a} \geq ``,
        - If ``\|y-x\|_{a} \geq 2 \|\nabla f(y)\|_{a*} / \mu``, then ``f(x) \geq f(y) = \alpha``, then ``f^{\leq \alpha}``  bounded.
    - This means ``\inf_{x\in\mathbf{dom}(f)} f > -\infty``, if further one of ``f^{\leq \alpha}`` is closed, then ``f`` have a global minimum.
    - The global minimum is also unique (trivial).

-  **Lemma E (quadratic lower bound)** If ``f`` is closed (means has closed sublevel sets), and ``\mathbf{x}^{\star}`` is its unique minimizer, then ``\forall z\in \mathbf{dom}(f), \frac{m}{2}\left\|z-x^{\star}\right\|^{2} \leq f(z)-f\left(x^{\star}\right) \leq \frac{1}{2 m}\|\nabla f(z)\|_{*}^{2}.``
    -  **Proof** similar to smoothness case.

- **Lemma 3.11 (equivalance of strong convexity in 2-norm)** ``f`` ``\mu``-strong convex over 2-norm iff ``k(x)=f(x)-\frac{\mu}{2}\|x\|^{2}_2`` is convex
    - **Proof** similar to smoothness case.
    -  If ``f`` twice differentiable, Hessian of ``h`` is ``\nabla^2 f - \mu I \succeq 0``, this means ``\forall x, \lambda_{\min }\left(\nabla^{2} f(x)\right) \geq \mu``.
        -  Strictly Convex.

### Smooth and Strong convexity
- **Lemma F (Extension of co-coercivity)** Suppose ``f`` is ``\mu``-strongly convex and ``L``-smooth for 2-norm, and ``\mathbf{dom}(f) = \mathbb{R}^n``
    - then function ``h(x)=f(x)-\frac{\mu}{2}\|x\|_{2}^{2}`` is ``L-\mu``-smooth
    - co-coercivity of ``\nabla h`` gives ``\forall x,y, (\nabla f(x)-\nabla f(y))^{T}(x-y) \geq \frac{\mu L}{\mu+L}\|x-y\|_{2}^{2}+\frac{1}{\mu+L}\|\nabla f(x)-\nabla f(y)\|_{2}^{2}``