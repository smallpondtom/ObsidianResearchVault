# Two-Pass Streaming-OpInf

## Todos
- [ ] Change the Tikhonov regularization to accept general regularizations in matrix form 📅 2024-12-11  ⏫ 
- [ ] Complete the experiments including the 3D Channel flow by the end of this year 📅 2024-12-31
- [ ] Complete most of the write up except for the intro, discussion, and conclusion of the Journal 📅 2024-12-31
- [ ] Complete the bullet points for the intro, discussion, and conclusion for the journal  📅 2024-12-31


## RLS and Gradient Descent

They are _related_, but RLS is _more_ than just a simple gradient descent. In particular:

- **Ordinary Gradient Descent** repeatedly updates the parameter estimate based on the local gradient of the cost function (often requiring a step size). Each new step basically says
    $$x_{\text{new}} = x_{\text{old}} \;-\; \alpha\,\nabla \! J(x_{\text{old}})$$
    
- **Recursive Least Squares** (RLS) uses a _matrix‐based_ update rule derived from the exact normal equations for least squares. It continually _adapts the inverse_ of the Hessian (the matrix A⊤AA^\top A in linear regression) via the _matrix‐inversion lemma_. This lets you jump directly to a solution _as though_ you were doing a “Newton‐type” update in each iteration.
    
From an “optimization” perspective, you can see RLS as a specialized form of _Newton’s method_ (or Gauss–Newton in the linear‐in‐the‐parameters case) applied recursively to the least‐squares cost. Meanwhile, _plain_ gradient descent is typically a first‐order method without directly estimating or inverting the Hessian.

Hence:

1. **RLS does descend** the squared‐error cost in a streaming/recursive way.
2. **It resembles** a Newton‐type method (since it effectively keeps track of the Hessian inverse).
3. **It usually converges much faster** than pure gradient descent because it uses that matrix‐inversion lemma to make _exact_ second‐order steps for the least‐squares problem.

Therefore, you can say RLS is somewhat _analogous_ to gradient descent, but _it is actually closer to a full second‐order update_ for the least‐squares cost, rather than a simple first‐order gradient step.

### 1. Gradient Descent vs. Newton’s Method

Consider a cost function $J(\theta)$. The two fundamental update schemes are:

1. **(First-order) Gradient Descent**
    $$\theta_{k+1} \;=\; \theta_k \;-\; \alpha\,\nabla J(\theta_k).$$
    
    This uses only the _first derivative_ (gradient) and a step size $\alpha$.
    
2. **(Second-order) Newton’s Method**
    $$\theta_{k+1}  =  \theta_k  −  \mathbf{H}^{-1}(\theta_k) \nabla J(\theta_k),$$
    
    where $\mathbf{H}(\theta)$ is the _Hessian_ (matrix of second derivatives). Thus Newton’s method uses _both_ the gradient and the Hessian (or its inverse) to potentially converge _much faster_.
    

---

### 2. Least-Squares Problem and Its Hessian

Consider a linear (or linear-in-the-parameters) least-squares problem. Suppose at each step $k$ we get a data vector $\phi_k$ and a target $y_k$ and want to solve:

$$\min_{\theta}\; \sum_{i=1}^{k} \bigl\|y_i - \phi_i^{\top}\theta\bigr\|^2.$$

1. The _gradient_ of the cost w.r.t. $\theta$ is $\nabla J(\theta) \;=\; -2 \sum_{i=1}^k \phi_i\,\bigl(y_i - \phi_i^{\top}\theta\bigr)$.
2. The _Hessian_ is (assuming a uniform weighting) $\mathbf{H} \;=\; 2\sum_{i=1}^{k}\phi_i\,\phi_i^{\top} \;=\; 2\,\Phi^\top \Phi$, where $\Phi$ collects the feature vectors $\phi_i$.

In a batch setting, a _Newton update_ for the least-squares problem would do

$$\theta_{\text{newton}}^{(k)} \;=\; \theta_{\text{old}} \;-\; \bigl(\Phi^\top\Phi\bigr)^{-1}\, \bigl(\textstyle\sum_{i=1}^k \phi_i\,\bigl(y_i - \phi_i^\top \theta_{\text{old}}\bigr)\bigr).$$

Since $\Phi^\top\Phi$ is the Hessian (up to a factor 2), this is effectively a second-order method.

---

### 3. RLS = Recursive Inversion of the Hessian

**Recursive Least Squares** maintains and _updates_ an estimate of $\bigl(\Phi^\top\Phi\bigr)^{-1}$ in real time. Specifically, if $P_{k-1}\approx(\Phi_{k-1}^\top \Phi_{k-1})^{-1}$, then upon arrival of the new data $(\phi_k, y_k)$, RLS applies the _matrix-inversion lemma_ (Sherman–Morrison update) to get $P_k\approx(\Phi_k^\top \Phi_k)^{-1}$. Then the update to $\theta_k$ is:

$$\theta_k = \theta_{k-1} \;+\; P_{k-1}\,\phi_k \,\Bigl[y_k \;-\;\phi_k^\top \theta_{k-1}\Bigr],$$

where

$$P_k \;=\; P_{k-1} \;-\; \frac{P_{k-1}\,\phi_k\,\phi_k^\top\,P_{k-1}}{1 + \phi_k^\top\,P_{k-1}\,\phi_k}.$$

Observe how $P_{k-1}$ acts like $\bigl(\Phi_{k-1}^\top \Phi_{k-1}\bigr)^{-1}$. Thus the gain term

$$\mathbf{g}_k = P_{k-1}\,\phi_k$$

is effectively $\mathbf{H}^{-1}\nabla J(\theta)$ in the local least-squares sense, _not just_ a simple gradient direction. That is _inherently second-order_.

---

### 4. Conclusion: RLS Is Like a Newton Update

Gradient descent would do something like

$$\theta_k = \theta_{k-1} - \alpha \sum_{i=1}^k \phi_i \bigl(y_i - \phi_i^\top \theta_{k-1}\bigr),$$

which is purely first-order and typically requires tuning $\alpha$ for stability/speed.

**RLS** automatically adjusts the step direction and scale as if it _knew_ (or updated knowledge of) the Hessian inverse $\bigl(\Phi^\top\Phi\bigr)^{-1}$. In that sense, _on each new sample,_ it is doing a local second-order (Newton-like) step _without needing a separate line search or step-size parameter_. This is exactly what sets RLS apart from standard first-order gradient updates.

Hence, we say:

> **RLS can be viewed as a second-order update** for a linear least-squares cost, because it _recursively_ maintains $\mathbf{H}^{-1}$ (the inverse Hessian) and uses it in each parameter update.
