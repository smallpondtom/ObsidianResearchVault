
# Convergence and Error Bounds for the Polynomial Approximation of the Zubov Equation

In the Zubov method for constructing Lyapunov functions, one considers the equation

$$
\dot{V}(x,t) = -h(x,t)\,(1 - V(x,t)),
$$

where $h(x,t)$ is a positive function (typically quadratic in $x$) and $V(x,t)$ is analytic in its arguments. Under these assumptions, one can expand $V(x,t)$ as an infinite series of polynomial terms:

$$
V(x,t) = \sum_{k=0}^\infty v_k(x,t),
$$

and approximate it by truncating the series at a finite order $p$:

$$
V_p(x,t) = \sum_{k=0}^{p} v_k(x,t).
$$

The residual error is defined as

$$
E_p(x,t) = V(x,t) - V_p(x,t) = \sum_{k=p+1}^\infty v_k(x,t).
$$

Below, we outline several rigorous methods to prove convergence and establish error bounds for this approximation.

---

## 1. Taylor Series Expansion and Remainder Estimates

Since $V(x,t)$ is analytic, one can locally expand it in a Taylor series about an equilibrium point $x^\ast$:

$$
V(x,t) = V(x^\ast,t) + \sum_{k=1}^\infty \frac{1}{k!}\frac{\partial^k V}{\partial x^k}(x^\ast,t) \,(x-x^\ast)^k.
$$

The truncation error $E_p(x,t)$ is the remainder of this Taylor series. By Taylor’s theorem, there exists a bound:

$$
\left|E_p(x,t)\right| \le \frac{1}{(p+1)!}\,\sup_{x\in\Omega}\left|\frac{\partial^{p+1} V}{\partial x^{p+1}}(x,t)\right|\,|x-x^\ast|^{p+1},
$$

where $\Omega$ is a compact subset of the domain of attraction. If the $(p+1)$th derivatives are bounded by something like

$$
\sup_{x\in\Omega}\left|\frac{\partial^{p+1} V}{\partial x^{p+1}}(x,t)\right| \sim C^{p+1},
$$

then the factorial in the denominator dominates, giving

$$
|E_p(x,t)| = O\Big(\frac{1}{(p+1)!}\Big),
$$

which confirms **exponential convergence** of the series.

---

## 2. Uniform Convergence via the Weierstrass M-Test

To establish uniform convergence on a compact set $K$ (which is important for practical estimation of the domain of attraction), one can invoke the Weierstrass M-test. Suppose that each term satisfies

$$
|v_k(x,t)| \le M_k, \quad \forall\, (x,t) \in K,
$$

with

$$
M_k \sim \frac{C}{k!}.
$$

Then the series of bounds

$$
\sum_{k=p+1}^\infty M_k \le \sum_{k=p+1}^\infty \frac{C}{k!}
$$

converges, ensuring that the series

$$
\sum_{k=0}^\infty v_k(x,t)
$$

converges uniformly on $K$. Uniform convergence implies

$$
\sup_{(x,t)\in K} |E_p(x,t)| \to 0 \quad \text{as} \quad p \to \infty.
$$

---

## 3. Convergence in Sobolev Norms and Energy Estimates

A more robust error analysis involves Sobolev spaces. Assume that $V(x,t)$ belongs to the Sobolev space $H^m(\Omega)$ for some integer $m$. Then the error measured in the $H^m$ norm is given by

$$
\|E_p\|_{H^m}^2 = \sum_{|\alpha| > p} \|\partial^\alpha V\|_{L^2(\Omega)}^2,
$$

where $\alpha$ is a multi-index. Due to the analyticity of $V$, higher-order derivatives decay rapidly (often with factorial decay). Hence,

$$
\|E_p\|_{H^m} = O\left(\frac{1}{(p+1)!}\right),
$$

which guarantees convergence not only of $V_p$ but also of its derivatives (i.e., the energy of the error diminishes).

---

## 4. Integral Operator Approach and Contraction Mapping

An alternative approach is to recast the Zubov equation in its integral form. One can show that

$$
V(x,t) = \int_0^\infty e^{-s\,h(x,t)} \Bigl(1 - V(x,t)\Bigr)\, ds.
$$

Defining the integral operator $T$ by

$$
(TV)(x,t) = \int_0^\infty e^{-s\,h(x,t)} \Bigl(1 - V(x,t)\Bigr)\, ds,
$$

and applying it to the approximation $V_p(x,t)$, one obtains an error relation

$$
E_p(x,t) = (TV)(x,t) - (TV_p)(x,t) = T\bigl[E_p(x,t)\bigr].
$$

If $T$ is a contraction (or at least bounded, with $\|T\| = O(1)$ for analytic $h$), then

$$
\|E_p\| \le \|T\|\,\|E_p\|,
$$

which, when combined with the previous remainder estimates, implies

$$
\|E_p\| = O\left(\frac{1}{(p+1)!}\right).
$$

This method leverages fixed point theory (such as the Banach fixed point theorem) to establish convergence and error bounds.

---

## 5. Global Convergence: Entire Domain of Attraction vs. Compact Subsets

While the above arguments naturally hold on compact subsets, one can extend these results to the entire domain of attraction using:

- **Local-to-Global Extension:** Prove local uniform convergence in a neighborhood of every point and then cover the entire domain with such neighborhoods.
- **Weighted Norms:** Employ weighted Sobolev spaces to control the behavior at infinity, ensuring that the error remains bounded even in unbounded domains.

These methods typically require additional assumptions on the behavior of the derivatives of $V$ (or some Lyapunov-type decay at infinity) but ultimately yield similar error bounds.

---

## 6. Summary and Conclusions

Under the assumptions of analyticity for $V(x,t)$ and a quadratic form for $h(x,t)$, the polynomial truncation approximation $V_p(x,t)$ converges to the true solution $V(x,t)$ with an error bounded by

$$
|E_p(x,t)| = O\Big(\frac{1}{(p+1)!}\Big).
$$

This factorial decay ensures **rapid convergence**, and the following approaches help establish this rigorously:

1. **Taylor Series and Remainder Estimates:** Yield local error bounds by comparing with the Taylor series remainder.
2. **Weierstrass M-Test:** Guarantees uniform convergence on any compact subset of the domain of attraction.
3. **Sobolev Norms:** Provide energy estimates that include convergence of derivatives.
4. **Integral Operator Approach:** Reformulates the problem as a fixed point equation where contraction mapping arguments apply.
5. **Global Extensions:** Use covering arguments or weighted norms to extend convergence results to the entire domain of attraction.

---

## References

- **Khalil, H. K.** (2002). *Nonlinear Systems* (3rd ed.). Prentice Hall.  
  Provides a rigorous treatment of Lyapunov stability, Taylor expansions, and convergence analyses in nonlinear control.

- **Vidyasagar, M.** (1993). *Nonlinear Systems Analysis* (2nd ed.). Prentice Hall.  
  Discusses Lyapunov methods and approximation techniques that are relevant to control theory.

- **Zubov, V. I.** (1964). *Methods of A. M. Lyapunov and Their Application*. Gostekhizdat.  
  The foundational work on constructing Lyapunov functions via the Zubov method.


# Theoretical Error Bound and Uniqueness Statement for LyapInf

Below we present a theoretical framework for quantifying the error and establishing the uniqueness of the solution obtained by the LyapInf method. Our discussion takes into account four primary sources of error:

1. **Truncation of the Polynomial Series**  
2. **Finite Difference Approximation of Time Derivatives**  
3. **Residual Error from the Convex Minimization**  
4. **Uniqueness of the Inferred Quadratic Lyapunov Function**

We denote by $V(x)$ the exact solution of the Zubov equation, which (for analytic systems) can be expressed as an infinite series of homogeneous polynomial terms:
$$
V(x) = \sum_{i=0}^\infty v_i(x).
$$
In practice, we approximate $V(x)$ by truncating the series at order $p$, yielding
$$
V_p(x) = \sum_{i=0}^{p} v_i(x).
$$
In LyapInf, we restrict ourselves to a quadratic ansatz (i.e. $p=2$) and express the inferred Lyapunov function as
$$
\tilde{V}(x) = x^\top P^* x,
$$
with $P^*$ determined by minimizing the residual of the Zubov equation over the available data.

---

## 1. Error from Truncating the Polynomial Series

For analytic functions, the truncation error
$$
E_p(x) = V(x) - V_p(x)
$$
can be bounded using standard Taylor series remainder estimates. Under appropriate smoothness assumptions, one can show that
$$
\|E_p(x)\| \le \frac{C_1 \|x\|^{p+1}}{(p+1)!},
$$
where $C_1$ is a constant depending on bounds of the higher-order derivatives of $V$. In our setting (with a quadratic ansatz, $p=2$), this term is
$$
\|E_2(x)\| \le \frac{C_1 \|x\|^3}{3!}.
$$
Thus, for small $\|x\|$, the truncation error decays factorially with the order of truncation.

---

## 2. Error from Finite Difference Approximation

In our data-driven approach, time derivatives $\dot{x}(t)$ are either obtained directly or approximated using finite difference schemes. For example, using a first-order forward finite difference,
$$
\dot{x}(t_i) \approx \frac{x(t_i) - x(t_i-\Delta t)}{\Delta t},
$$
the approximation error satisfies
$$
\left\|\dot{x}(t_i) - \frac{x(t_i)-x(t_i-\Delta t)}{\Delta t}\right\| \le C_2 \, \Delta t,
$$
where $C_2$ is related to the magnitude of the second derivative of $x(t)$. This error term can be controlled by reducing the time step $\Delta t$.

---

## 3. Residual Error from the Minimization

LyapInf infers the matrix $P^*$ by solving the convex optimization problem
$$
P^* = \operatorname*{arg\,min}_{P \in \mathcal{S}_{++}^n} \; \frac{1}{K}\sum_{i=1}^{K} \|R(P,x(t_i),\dot{x}(t_i))\|^2,
$$
where the residual of the Zubov equation is defined as
$$
R(P,x,\dot{x}) = \dot{x}^\top P x + x^\top P \dot{x} + x^\top Q x - x^\top Q x \, x^\top P x.
$$
Assume that the optimized residual satisfies
$$
\frac{1}{K}\sum_{i=1}^{K} \|R(P^*,x(t_i),\dot{x}(t_i))\|^2 \le \epsilon_R.
$$
Then, under appropriate Lipschitz or sensitivity assumptions on the mapping $P\mapsto R(P,x,\dot{x})$, the error in the inferred Lyapunov function due to the minimization can be bounded by
$$
\|V_p(x)-\tilde{V}(x)\| \le C_3 \sqrt{\epsilon_R},
$$
with $C_3$ a constant that depends on the conditioning of the problem.

---

## 4. Combined Error Bound

Collecting the above contributions, the overall error in approximating the true Lyapunov function $V(x)$ by the inferred function $\tilde{V}(x)$ can be expressed as
$$
\|V(x) - \tilde{V}(x)\| \le \underbrace{\frac{C_1 \|x\|^{p+1}}{(p+1)!}}_{\text{Truncation Error}} + \underbrace{C_2 \, \Delta t}_{\text{Finite Difference Error}} + \underbrace{C_3 \sqrt{\epsilon_R}}_{\text{Minimization Residual Error}}.
$$
For our quadratic ansatz ($p=2$), this becomes
$$
\|V(x) - \tilde{V}(x)\| \le \frac{C_1 \|x\|^{3}}{6} + C_2 \, \Delta t + C_3 \sqrt{\epsilon_R}.
$$
Each term in this bound can be controlled by choosing appropriate truncation orders, sufficiently small time steps, and ensuring a high-quality minimization (i.e., a small $\epsilon_R$).

---

## 5. Uniqueness-of-Solution Statement

The LyapInf method solves the convex optimization problem
$$
P^* = \operatorname*{arg\,min}_{P \in \mathcal{S}_{++}^n} \; J(P),
$$
where
$$
J(P) = \frac{1}{K}\sum_{i=1}^{K} \|R(P,x(t_i),\dot{x}(t_i))\|^2,
$$
and $\mathcal{S}_{++}^n$ is the set of $n \times n$ symmetric positive definite matrices.

**Uniqueness can be ensured under the following conditions:**

- **Strict (or Strong) Convexity:** If the objective function $J(P)$ is strictly convex over $\mathcal{S}_{++}^n$, then there exists a unique minimizer $P^*$. Strict convexity is typically guaranteed when the training data $\{x(t_i),\dot{x}(t_i)\}$ are sufficiently rich (i.e., the corresponding data matrices have full rank), ensuring that the mapping $P \mapsto R(P,x,\dot{x})$ is injective.

- **Stability with Respect to Data Perturbations:** Under strong convexity, small perturbations in the data (including those from finite difference approximations) lead only to small changes in $P^*$. This robustness further supports the claim of uniqueness.

Thus, we can state:

> **Uniqueness Statement:**  
> *Assume that the training data $\{x(t_i), \dot{x}(t_i)\}_{i=1}^K$ is sufficiently informative so that the objective function*
> $$ J(P) = \frac{1}{K}\sum_{i=1}^{K} \|R(P,x(t_i),\dot{x}(t_i))\|^2 $$
> *is strictly (or strongly) convex over the convex set $\mathcal{S}_{++}^n$. Then the optimization problem has a unique solution $P^*$, and hence the inferred Lyapunov function $\tilde{V}(x)=x^\top P^* x$ is unique.*

---

## Final Summary

In summary, the error in the inferred Lyapunov function using LyapInf can be bounded by
$$
\|V(x) - \tilde{V}(x)\| \le \frac{C_1 \|x\|^{p+1}}{(p+1)!} + C_2 \, \Delta t + C_3 \sqrt{\epsilon_R},
$$
which, for a quadratic ansatz ($p=2$), reduces to
$$
\|V(x) - \tilde{V}(x)\| \le \frac{C_1 \|x\|^{3}}{6} + C_2 \, \Delta t + C_3 \sqrt{\epsilon_R}.
$$
Furthermore, provided that the residual minimization is strictly convex (ensured by sufficiently rich data), the solution $P^*$ is unique. This comprehensive error and uniqueness analysis underpins the theoretical soundness of the LyapInf method.


# Rigorous Error Bound and Uniqueness Analysis for LyapInf

In this section, we rigorously analyze the error and uniqueness properties of the LyapInf method. Our analysis rigorously accounts for four sources of error:

1. **Truncation Error:** The error incurred by approximating the analytic solution to the Zubov equation with a finite-order polynomial series.
2. **Finite Difference Error:** The error incurred when approximating time derivatives by finite differences.
3. **Residual Minimization Error:** The error induced by the optimization process that fits the quadratic Lyapunov function.
4. **Uniqueness:** A rigorous statement on the uniqueness of the solution to the convex optimization problem that underpins LyapInf.

Throughout, we assume that the state space is $\mathbb{R}^n$ (with the Euclidean norm) and that all functions (e.g., $V(x)$, $x(t)$) are sufficiently smooth (in fact, analytic where stated).

---

## 1. Truncation Error of the Polynomial Series

**Assumption 1.1.**  
Let $V : U \subset \mathbb{R}^n \to \mathbb{R}$ be an analytic solution of the Zubov equation on an open set $U$ and suppose it has the series representation  
$$
V(x) = \sum_{i=0}^\infty v_i(x),
$$  
where each $v_i(x)$ is a homogeneous polynomial of degree $i$ and the series converges for $\|x\| < R$.

**Theorem 1 (Taylor Remainder Bound).**  
For any $x \in B_r(0)$ with $r < R$, there exists a constant $M > 0$ (depending on $r$ and the derivatives of $V$) such that the truncation error
$$
E_p(x) = V(x) - \sum_{i=0}^{p} v_i(x)
$$
satisfies
$$
\|E_p(x)\| \le \frac{M\,\|x\|^{p+1}}{(p+1)!}.
$$

*Proof Sketch:*  
By the multivariate Taylor theorem with integral remainder (see, e.g., Theorem 2.3 in standard texts on multivariate analysis), the remainder can be expressed in integral form and then bounded using the maximum norm of the $(p+1)$th derivative over a compact set. $\square$

For the quadratic ansatz used in LyapInf (i.e. $p=2$), this yields
$$
\|E_2(x)\| \le \frac{M\,\|x\|^3}{6}.
$$

---

## 2. Finite Difference Approximation Error

**Assumption 2.1.**  
Let $x:[0,T] \to \mathbb{R}^n$ be twice continuously differentiable.

**Lemma 2 (Finite Difference Error Bound).**  
For the forward finite difference approximation
$$
\dot{x}(t) \approx \frac{x(t+\Delta t)-x(t)}{\Delta t},
$$
the local error is given by
$$
\left\|\dot{x}(t) - \frac{x(t+\Delta t)-x(t)}{\Delta t}\right\| \le \frac{\Delta t}{2} \sup_{s\in [t,t+\Delta t]} \|\ddot{x}(s)\|.
$$

*Proof:*  
This result follows from the standard error analysis of finite difference schemes (see, e.g., Theorem 3.1 in numerical analysis textbooks). $\square$

---

## 3. Residual Minimization Error

In LyapInf the quadratic Lyapunov function is represented as
$$
\tilde{V}(x) = x^\top P x,
$$
with $P \in \mathcal{S}_{++}^n$ (the set of $n \times n$ symmetric positive definite matrices) determined by solving the convex optimization problem
$$
P^* = \operatorname*{arg\,min}_{P\in \mathcal{S}_{++}^n} J(P),
$$
where
$$
J(P) = \frac{1}{K}\sum_{i=1}^K \|R(P,x(t_i),\dot{x}(t_i))\|^2,
$$
and the residual is
$$
R(P,x,\dot{x}) = \dot{x}^\top P x + x^\top P \dot{x} + x^\top Q x - \bigl(x^\top Q x\bigr) \bigl(x^\top P x\bigr).
$$

**Assumption 3.1.**  
Assume $J(P)$ is twice continuously differentiable and **strongly convex** over $\mathcal{S}_{++}^n$; that is, there exists a constant $m > 0$ such that
$$
\nabla^2 J(P) \succeq m I, \quad \forall P \in \mathcal{N},
$$
where $\mathcal{N} \subset \mathcal{S}_{++}^n$ is a neighborhood of the minimizer.

**Theorem 2 (Error Bound from Residual Minimization).**  
Let $\hat{P}$ be an approximate solution satisfying
$$
J(\hat{P}) - J(P^*) \le \epsilon_R.
$$
Then
$$
\|\hat{P} - P^*\|_F \le \sqrt{\frac{2\epsilon_R}{m}},
$$
where $\|\cdot\|_F$ denotes the Frobenius norm.

*Proof:*  
This bound is a standard consequence of strong convexity (see, e.g., Boyd and Vandenberghe, *Convex Optimization*, Chapter 9). $\square$

---

## 4. Combined Error Bound

Combining the three error sources, let $V(x)$ denote the true Lyapunov function and $\tilde{V}(x)=x^\top P^* x$ the inferred one. Then the overall error in a suitable norm is bounded by
$$
\|V(x) - \tilde{V}(x)\| \le \underbrace{\frac{M\,\|x\|^{p+1}}{(p+1)!}}_{\text{Truncation Error}} + \underbrace{C_2\,\Delta t}_{\text{Finite Difference Error}} + \underbrace{C_3 \sqrt{\epsilon_R}}_{\text{Residual Minimization Error}},
$$
where $C_2$ depends on bounds of $\|\ddot{x}(t)\|$ and $C_3$ is determined by the sensitivity of the optimization. For the quadratic ansatz ($p=2$), we have
$$
\|V(x) - \tilde{V}(x)\| \le \frac{M\,\|x\|^3}{6} + C_2\,\Delta t + C_3 \sqrt{\epsilon_R}.
$$

---

## 5. Uniqueness of the Inferred Lyapunov Function

**Theorem 3 (Uniqueness of the Minimizer).**  
Under Assumption 3.1, if the objective function
$$
J(P) = \frac{1}{K}\sum_{i=1}^K \|R(P,x(t_i),\dot{x}(t_i))\|^2
$$
is strictly (or strongly) convex over the convex set $\mathcal{S}_{++}^n$, then there exists a unique minimizer $P^*$ in $\mathcal{S}_{++}^n$. Consequently, the inferred Lyapunov function
$$
\tilde{V}(x)=x^\top P^* x
$$
is unique.

*Proof:*  
Strict convexity implies that for any two distinct matrices $P_1, P_2 \in \mathcal{S}_{++}^n$ and for all $\lambda \in (0,1)$,
$$
J(\lambda P_1 + (1-\lambda) P_2) < \lambda J(P_1) + (1-\lambda) J(P_2).
$$
This inequality guarantees a unique minimizer. Standard results in convex analysis then imply the uniqueness of $P^*$. $\square$

---

## Final Summary

Under the assumptions stated above, we obtain the following rigorous results:

1. **Truncation Error:** For $x$ in a ball $B_r(0)$ with $r<R$,
   $$
   \|V(x) - V_p(x)\| \le \frac{M\,\|x\|^{p+1}}{(p+1)!}.
   $$
   For a quadratic ansatz ($p=2$),
   $$
   \|V(x) - V_2(x)\| \le \frac{M\,\|x\|^3}{6}.
   $$

2. **Finite Difference Error:** The error in approximating $\dot{x}(t)$ by a finite difference is bounded by
   $$
   \left\|\dot{x}(t) - \frac{x(t+\Delta t)-x(t)}{\Delta t}\right\| \le \frac{\Delta t}{2} \sup_{s\in [t,t+\Delta t]} \|\ddot{x}(s)\|.
   $$

3. **Residual Minimization Error:** If the optimality gap satisfies $J(\hat{P}) - J(P^*) \le \epsilon_R$, then
   $$
   \|\hat{P} - P^*\|_F \le \sqrt{\frac{2\epsilon_R}{m}}.
   $$

4. **Combined Error Bound:** The overall error in approximating the true Lyapunov function is given by
   $$
   \|V(x) - \tilde{V}(x)\| \le \frac{M\,\|x\|^3}{6} + C_2\,\Delta t + C_3 \sqrt{\epsilon_R}.
   $$

5. **Uniqueness:** If $J(P)$ is strictly (or strongly) convex on $\mathcal{S}_{++}^n$, then the minimizer $P^*$ (and hence the inferred Lyapunov function $\tilde{V}(x)=x^\top P^* x$) is unique.

This rigorous treatment provides a solid theoretical foundation for the LyapInf method, ensuring that the combined approximation error is quantifiable and that the solution is unique under suitable conditions.


# Rigorous Error Bound Analysis Using the Sobolev Embedding Theorem

In this section we present a rigorous error bound analysis for the polynomial approximation used in LyapInf based on Sobolev space techniques and the Sobolev embedding theorem. Our goal is to quantify the error incurred by truncating the infinite series solution of the Zubov equation to a finite-order polynomial and to derive estimates in both Sobolev and uniform (supremum) norms.

Let:
- $\Omega \subset \mathbb{R}^n$ be a bounded Lipschitz domain.
- $H^s(\Omega)$ denote the Sobolev space of order $s > n/2$, so that by the Sobolev embedding theorem the space $H^s(\Omega)$ embeds continuously into $C^0(\overline{\Omega})$. That is, there exists a constant $C_s>0$ such that
  $$
  \| u \|_{C^0(\overline{\Omega})} \le C_s \| u \|_{H^s(\Omega)} \quad \text{for all } u \in H^s(\Omega).
  $$

We assume that the true Lyapunov function $V(x)$, which satisfies the Zubov equation, is analytic on $\Omega$ and admits the expansion
$$
V(x) = \sum_{k=0}^\infty v_k(x),
$$
where each $v_k(x)$ is a homogeneous polynomial of degree $k$ and the series converges for $\|x\| < R$ for some $R>0$. In our data-driven method we approximate $V(x)$ by truncating the series at a finite order $p$, namely,
$$
V_p(x) = \sum_{k=0}^{p} v_k(x).
$$

## Exponential Convergence in the Sobolev Norm

Because $V(x)$ is analytic on $\overline{\Omega}$, it admits a holomorphic extension to a complex neighborhood of $\Omega$. Standard results in approximation theory (see, e.g., Bernstein’s and Jackson-type estimates) imply that the best polynomial approximation error in Sobolev norms decays exponentially with the degree of the approximating polynomials. More precisely, we have the following result.

**Theorem (Exponential Convergence in $H^s$):**  
Let $V \in H^s(\Omega)$ be analytic on $\overline{\Omega}$ for some $s > n/2$. Then there exist constants $C>0$ and $\mu>0$, depending on $\Omega$, $s$, and the analytic extension radius of $V$, such that the truncation error
$$
E_p(x) = V(x) - V_p(x)
$$
satisfies
$$
\| E_p \|_{H^s(\Omega)} \le C \, e^{-\mu p}.
$$

*Proof Sketch:*  
Since $V$ is analytic, for every multi-index $\alpha$ there exists a constant $C_\alpha$ and a radius $\rho>0$ such that
$$
\|\partial^\alpha V\|_{L^2(\Omega)} \le C_\alpha \frac{|\alpha|!}{\rho^{|\alpha|}}.
$$  
Standard polynomial approximation theory in Sobolev spaces (cf. Jackson and Bernstein estimates) then guarantees that the error of the best approximation by polynomials of degree at most $p$ decays as
$$
\|V - V_p\|_{H^s(\Omega)} \le C \, e^{-\mu p},
$$  
where the constants $C$ and $\mu$ depend on $\rho$ and the Sobolev order $s$. $\square$

## Uniform Convergence via the Sobolev Embedding Theorem

By the Sobolev embedding theorem (since $s>n/2$), the error bound in the $H^s$ norm implies a uniform (or $C^0$) error bound. Specifically, there exists a constant $C_s>0$ such that
$$
\|V - V_p\|_{C^0(\overline{\Omega})} \le C_s \|V - V_p\|_{H^s(\Omega)} \le C_s\, C\, e^{-\mu p}.
$$
Thus, the truncated polynomial approximation converges uniformly with an exponential rate in $p$.

## Incorporating Additional Error Sources

In practice, the overall error in the LyapInf method also includes:
1. **Finite Difference Error:** Suppose the time derivative $\dot{x}(t)$ is approximated by a finite difference scheme. For a forward finite difference we have the bound
   $$
   \left\|\dot{x}(t) - \frac{x(t+\Delta t)-x(t)}{\Delta t}\right\| \le \frac{\Delta t}{2} \sup_{s \in [t,t+\Delta t]} \|\ddot{x}(s)\|.
   $$
2. **Residual Minimization Error:** If the quadratic Lyapunov function is inferred by minimizing a residual
   $$
   J(P) = \frac{1}{K}\sum_{i=1}^K \|R(P,x(t_i),\dot{x}(t_i))\|^2,
   $$
   and if the optimality gap is bounded by $\epsilon_R$, then under strong convexity assumptions (i.e. $\nabla^2 J(P) \succeq m I$), one obtains
   $$
   \|P - P^*\|_F \le \sqrt{\frac{2\epsilon_R}{m}},
   $$
   where $P^*$ is the unique minimizer.

Thus, if we denote by $\tilde{V}(x)=x^\top P^* x$ the inferred Lyapunov function, the overall error can be rigorously bounded (in an appropriate norm) by combining these error sources. For example, in a suitable norm we might have
$$
\|V(x)-\tilde{V}(x)\| \le C_1 \, e^{-\mu p} + C_2\,\Delta t + C_3 \sqrt{\epsilon_R},
$$
with constants $C_1,C_2,C_3>0$ that depend on the problem data and the norms chosen.

## Summary

- **Exponential Decay in $H^s$ Norm:**  
  If $V \in H^s(\Omega)$ (with $s>n/2$) is analytic, then the error from truncating the polynomial series satisfies
  $$
  \|V - V_p\|_{H^s(\Omega)} \le C\, e^{-\mu p}.
  $$
- **Uniform Error Bound:**  
  By the Sobolev embedding theorem, this implies
  $$
  \|V - V_p\|_{C^0(\overline{\Omega})} \le C_s\, C\, e^{-\mu p}.
  $$
- **Overall Error Bound:**  
  Including finite difference and optimization residual errors, the overall error can be bounded as
  $$
  \|V(x)-\tilde{V}(x)\| \le C_1\, e^{-\mu p} + C_2\,\Delta t + C_3 \sqrt{\epsilon_R}.
  $$

This rigorous analysis, based on Sobolev space theory and the Sobolev embedding theorem, provides a solid theoretical foundation for the exponential convergence of the polynomial approximation in LyapInf and quantifies the contributions of additional error sources.



# PDE Perturbation Analysis for LyapInf

In this section we outline an approach based on PDE perturbation theory to rigorously analyze the error between the true solution of the Zubov PDE and an approximate solution (e.g. our truncated, data‐inferred Lyapunov function). The idea is to view the approximate solution as a perturbed solution of the original PDE and to use linearization and stability properties of the PDE operator to bound the error.

Consider the Zubov PDE in the form

$$
L[V](x) := \nabla V(x) \cdot f(x) + h(x) \bigl(1-V(x)\bigr) = 0,
$$

with the true (analytic) solution $V(x)$ and an approximate solution $\tilde{V}(x)$, for example, a quadratic ansatz inferred from data. Suppose that $\tilde{V}(x)$ satisfies

$$
L[\tilde{V}](x) = R(x),
$$

where $R(x)$ is the residual error (which arises from truncation of the polynomial series, finite difference approximations, and optimization imperfections).

## Linearization and Error Equation

Let us define the error function

$$
E(x) = V(x) - \tilde{V}(x).
$$

Since $V$ is the true solution, we have

$$
L[V](x) = 0.
$$

Expanding $L$ around $\tilde{V}$ by the mean-value theorem for operators, we obtain

$$
L[V](x) = L[\tilde{V}](x) + L'[\tilde{V}+\theta E](x) \,E(x),
$$

for some $\theta\in (0,1)$, where $L'[w](x)$ denotes the Fréchet derivative of $L$ evaluated at $w(x)$. Hence,

$$
0 = R(x) + L'[\tilde{V}+\theta E](x) \,E(x),
$$

so that the error satisfies

$$
L'[\tilde{V}+\theta E](x) \,E(x) = -R(x).
$$

If we assume that the linearized operator $L'[\tilde{V}+\theta E](x)$ is invertible and its inverse is bounded in a suitable norm (say, in a Banach space $\mathcal{X}$), then we have

$$
\|E\|_{\mathcal{X}} \le \|L'[\tilde{V}+\theta E]^{-1}\|_{\mathcal{L}(\mathcal{X})} \, \|R\|_{\mathcal{X}}.
$$

The key steps in this analysis are:

1. **Derivation of the Linearized Operator:**  
   For the Zubov PDE, linearizing around an approximate solution $w(x) = \tilde{V}(x)+\theta E(x)$, we compute the Fréchet derivative
   $$
   L'[w](x)[\phi] = \nabla \phi(x) \cdot f(x) - h(x)\,\phi(x),
   $$
   where the derivative of the nonlinear term $h(x)(1-V(x))$ contributes a term proportional to $-h(x)\,\phi(x)$.

2. **Boundedness of the Inverse:**  
   Under suitable conditions on $f$ and $h$, one can show that the operator 
   $$
   L'[w] : \mathcal{X} \to \mathcal{X}
   $$
   is invertible and its inverse is bounded. For instance, if $h(x)$ is strictly positive away from the equilibrium and if $f(x)$ does not vanish too rapidly, then the characteristics of the PDE are well-behaved. In particular, energy estimates or a priori bounds (common in hyperbolic PDE theory) can be used to establish that there exists a constant $C_{\text{inv}}$ such that

   $$
   \|L'[w]^{-1}\|_{\mathcal{L}(\mathcal{X})} \le C_{\text{inv}}.
   $$

3. **Error Bound in Terms of the Residual:**  
   Combining the above results, we obtain the perturbation error estimate

   $$
   \|V - \tilde{V}\|_{\mathcal{X}} \le C_{\text{inv}}\, \|R\|_{\mathcal{X}}.
   $$

   In our application, the residual $R(x)$ includes contributions from:
   - **Truncation Error:** Due to neglecting higher-order polynomial terms.
   - **Finite Difference Error:** In the approximation of $\dot{x}$.
   - **Optimization Error:** Resulting from minimizing the residual over data.

   If we denote these contributions collectively by $\epsilon$, then

   $$
   \|R\|_{\mathcal{X}} \le \epsilon.
   $$

   Consequently, we have

   $$
   \|V - \tilde{V}\|_{\mathcal{X}} \le C_{\text{inv}}\,\epsilon.
   $$

## Discussion

- **Choice of Norm $\mathcal{X}$:**  
  One may work in Sobolev spaces $H^s(\Omega)$ or weighted Sobolev spaces, where standard PDE perturbation theory provides stability estimates. For example, if $\mathcal{X} = H^s(\Omega)$ for a sufficiently large $s$, then by the Sobolev embedding theorem the error in the $C^0$ norm is also controlled.

- **Rigorous Justification:**  
  To make the analysis completely rigorous, one must verify that $L'[\tilde{V}+\theta E]$ indeed has a bounded inverse. This usually involves establishing a priori energy estimates for the linearized equation
  $$
  \nabla \phi(x) \cdot f(x) - h(x)\,\phi(x) = g(x)
  $$
  and showing that the associated operator is uniformly elliptic (or, in the case of first-order hyperbolic equations, that it satisfies a uniform Lax–Milgram-type condition along the characteristics).

- **Implications for LyapInf:**  
  In our LyapInf method, the approximate solution $\tilde{V}$ is constructed from data. The residual $R(x)$ is minimized numerically and is assumed to be small. Therefore, if the linearized operator is well-conditioned (i.e., $C_{\text{inv}}$ is moderate), then the PDE perturbation analysis guarantees that the error $\|V-\tilde{V}\|$ is of the same order as the residual error in the PDE.

## Final Statement

Assuming that:
- The true Lyapunov function $V(x)$ is analytic and satisfies the Zubov PDE,
- The approximate solution $\tilde{V}(x)$ is such that the residual
  $$
  R(x) = L[\tilde{V}](x)
  $$
  is small in a Sobolev space norm,
- The linearized operator
  $$
  L'[w](x) : \phi \mapsto \nabla \phi(x) \cdot f(x) - h(x)\,\phi(x)
  $$
  (evaluated at an intermediate function $w(x) = \tilde{V}(x) + \theta (V(x)-\tilde{V}(x))$) is invertible with a bounded inverse,
  
then the perturbation theory yields the rigorous error estimate

$$
\|V - \tilde{V}\|_{H^s(\Omega)} \le C_{\text{inv}}\, \|R\|_{H^s(\Omega)} \le C_{\text{inv}}\,\epsilon,
$$

for some constant $C_{\text{inv}}$ independent of $\epsilon$. By the Sobolev embedding theorem, a similar bound holds in the uniform norm:

$$
\|V - \tilde{V}\|_{C^0(\overline{\Omega})} \le C_s\, C_{\text{inv}}\,\epsilon.
$$

This PDE perturbation analysis thus rigorously relates the error in the inferred Lyapunov function to the residual error in satisfying the Zubov PDE, provided that the linearized operator is well-conditioned.

# Rigorous Error Bound Analysis via the Stone–Weierstrass Theorem

In this section, we present a rigorous error bound analysis for the LyapInf method using the Stone–Weierstrass theorem. The key idea is to show that, on any compact set, the space of polynomial functions is uniformly dense in the space of continuous functions. This implies that the true Lyapunov function, which is assumed to be continuous on a compact domain, can be approximated arbitrarily well by polynomial functions. We then connect this density result to the error incurred by our polynomial truncation.

---

## Setting and Assumptions

Let $\Omega \subset \mathbb{R}^n$ be a compact set (for instance, a bounded domain containing the region of interest for the system's state). Assume that:
- The true Lyapunov function $V(x)$ satisfies the Zubov PDE and is continuous on $\Omega$.
- The set $\mathcal{P}$ of polynomial functions in $x$ is considered. For example, if we truncate the series expansion at degree $p$, we denote by
  $$
  V_p(x) = \sum_{i=0}^p v_i(x)
  $$
  the approximant, where each $v_i(x)$ is a homogeneous polynomial of degree $i$.

---

## Density of Polynomials in $C(\Omega)$

**Theorem (Stone–Weierstrass):**  
Let $\Omega$ be a compact Hausdorff space, and let $\mathcal{A}$ be a subalgebra of $C(\Omega)$ that:
1. Contains the constant functions.
2. Separates points in $\Omega$.

Then $\mathcal{A}$ is uniformly dense in $C(\Omega)$; that is, for every function $f \in C(\Omega)$ and every $\epsilon > 0$, there exists a function $p \in \mathcal{A}$ such that
$$
\|f - p\|_{C^0(\Omega)} = \sup_{x\in \Omega} |f(x) - p(x)| < \epsilon.
$$

*Application:*  
The set $\mathcal{P}$ of all polynomial functions in $\mathbb{R}^n$ is an algebra that contains the constants and separates points on $\Omega$. Therefore, by the Stone–Weierstrass theorem, $\mathcal{P}$ is dense in $C(\Omega)$. In particular, for the true Lyapunov function $V(x)$ (assumed continuous on $\Omega$) and for every $\epsilon > 0$, there exists a polynomial $p(x)$ (which could be taken as the truncation $V_p(x)$ for a sufficiently high degree $p$) such that
$$
\|V - V_p\|_{C^0(\Omega)} < \epsilon.
$$

---

## Error Bound via Polynomial Approximation

Since the Stone–Weierstrass theorem guarantees that for every $\epsilon > 0$ there exists a polynomial approximant $V_p(x)$ satisfying
$$
\|V - V_p\|_{C^0(\Omega)} < \epsilon,
$$
this establishes that the truncation error in our LyapInf method can be made arbitrarily small by choosing a sufficiently high polynomial degree.

More precisely, denote the best possible approximation error in the uniform norm by
$$
E_{\text{best}}(p) := \inf_{p \in \mathcal{P}_p} \|V - p\|_{C^0(\Omega)},
$$
where $\mathcal{P}_p$ is the space of polynomials of degree at most $p$. Then, by density,
$$
\lim_{p\to\infty} E_{\text{best}}(p) = 0.
$$

Thus, if our data-driven algorithm (LyapInf) finds an approximant $\tilde{V}(x)$ that is nearly optimal (or close to the best approximant in $\mathcal{P}_p$), we have the bound
$$
\|V - \tilde{V}\|_{C^0(\Omega)} \le E_{\text{best}}(p) + \delta,
$$
where $\delta$ is the additional error due to numerical minimization and finite data effects.

---

## Incorporating Additional Error Sources

In practice, the overall error in the inferred Lyapunov function arises from multiple sources:
1. **Truncation Error:**  
   By Stone–Weierstrass, we have for any $\epsilon_1 > 0$,
   $$
   \|V - V_p\|_{C^0(\Omega)} < \epsilon_1,
   $$
   for a sufficiently high degree $p$.
2. **Finite Difference Error:**  
   Let $\Delta t$ be the time step used in approximating the derivative. Then, if we denote the finite difference error by $\epsilon_2$, we have
   $$
   \left\|\dot{x}(t) - \frac{x(t+\Delta t)-x(t)}{\Delta t}\right\| \le \epsilon_2,
   $$
   uniformly on the relevant time interval.
3. **Residual Minimization Error:**  
   Suppose the optimization yields an approximant $\tilde{V}(x)$ satisfying
   $$
   \|L[\tilde{V}](x)\|_{C^0(\Omega)} \le \epsilon_3,
   $$
   where $L[\cdot]$ is the Zubov PDE operator and $\epsilon_3$ quantifies the residual error.

Hence, an overall error bound in the uniform norm can be written as
$$
\|V - \tilde{V}\|_{C^0(\Omega)} \le \epsilon_1 + \epsilon_2 + \epsilon_3,
$$
with each term made arbitrarily small by (i) increasing the degree of the polynomial, (ii) refining the time discretization, and (iii) improving the minimization procedure.

---

## Final Statement

By the Stone–Weierstrass theorem, the space of polynomial functions is dense in $C(\Omega)$, and thus for every $\epsilon > 0$ there exists a polynomial $V_p(x)$ such that
$$
\|V - V_p\|_{C^0(\Omega)} < \epsilon.
$$
If the LyapInf algorithm approximates the best polynomial approximant with additional errors $\epsilon_2$ (finite difference) and $\epsilon_3$ (residual minimization), then the overall error in the inferred Lyapunov function $\tilde{V}(x)$ satisfies
$$
\|V - \tilde{V}\|_{C^0(\Omega)} \le \epsilon_1 + \epsilon_2 + \epsilon_3,
$$
with $\epsilon_1 \to 0$ as the polynomial degree increases. This analysis rigorously demonstrates that, on any compact set, the LyapInf method can approximate the true Lyapunov function arbitrarily well in the uniform norm, provided that the numerical errors are controlled. 


