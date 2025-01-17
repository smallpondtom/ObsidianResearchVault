
EMOTIONAL DAMAGEEEEEEEEEEEEEEEEEEEEEEEEEEE!!!!!!

# Reviewer 1 

## Summary 

The paper introduces some preliminary results on solving the Zubov equation using a quadratic ansatz, which is intended for easier formal verification compared to a neural network ansatz. The idea is clear and well-articulated, making it suitable for a conference paper. However, I would encourage the authors to address the following comments if they intend to extend this paper for a
journal version.

## Comments

1.  Please verify whether exponential stability is needed instead of just asymptotic stability when studying Zubov’s equation.
2. The notion $\mathcal{D}$ in the statement of Theorem 2 comes out of nowhere
3. Please indicate the truncation error when using Eq. (6).
4. I doubt that the error would be small when $p = 2$. It seems like the solution is being forced into a projection onto a low-resolution basis, which may not cause the residual error to reduce.
5. In Section III.A, merely minimizing the residual error may not yield a correct approximation of the true solution. It is necessary to formally argue for the uniqueness of the solution under specific conditions. It is also worth noting that there may be unbounded solutions for Zubov’s equation. Particularly when using a quadratic ansatz, you might approximate an incorrect solution. Additional considerations, such as boundary conditions or the evolution speed at large domains, should be taken into account.
6. Counterexample: Consider ODE $\dot{x} = −x + x^3$. One can demonstrate that $V (x) = x^2$ is always a viscosity solution to $V'(x)(−x + x^3) = −2x^2(1 − V (x))$ with condition $V (0) = 0$. However, what we really want is a continuous function such that $V (x) = x^2$ for $x ∈ (−1, 1)$ and $V (x) ≡ 1$ elsewhere. To make Theorem 2 work, I wonder what extra efforts you have made to avoid unbounded solutions.
7. In Section IV.B, I have some concerns about the theoretical guarantees. Intuitively, one might use a large number of finite elements or neural networks to approximate a solution. However, your solution ansatz has a very low resolution. In this case, it’s unclear what exactly is being approximated. Even though quadratic ansatzes can always work within a local region, my intuition suggests that you should sample points within a small range and proceed accordingly, which would likely be fine. The way you approach this is to attempt finding a semi-global solution first, then restrict it to a small region and check if it meets those conditions. The gap lies in how to transition from a possibly inaccurate approximation to a locally verifiable Lyapunov function. Does this method always work?
8. The discussion in Section IV.C is fine. However, data efficiency is the only factor considered here, given the type of basis functions you are comparing.
9. In Section V, with respect to solving Zubov’s equation, many related works sample initial conditions without considering the stability of the trajectories. Poor estimates might result from the lack of physical information involved in the algorithm.

Additional tools/references
[1] Edwards, Alec, Andrea Peruffo, and Alessandro Abate. “Fossil 2.0: Formal Certificate Synthesis for the Verification and Control of Dynamical Models.” In Proceedings of the 27th ACM International Conference on Hybrid Systems: Computation and Control, pp. 1-10. 2024.
[2] Liu, Jun, Yiming Meng, Maxwell Fitzsimmons, and Ruikun Zhou. “TOOL LyZNet: ALightweight Python Tool for Learning and Verifying Neural Lyapunov Functions and Regions of Attraction.” In Proceedings of the 27th ACM International Conference on Hybrid Systems: Computation and Control, pp. 1-8. 2024.
[3] Alfarano, Alberto, Franc¸ois Charton, and Amaury Hayat. “Global Lyapunov functions: a long-standing open problem in mathematics, with symbolic transformers.” arXiv preprint arXiv:2410.08304 (2024).



# Reviewer 2

## Summary

The authors propose a method for Lyapunov function identification based on Zubov's method, based on direct estimation from system trajectory data using a quadratic form.

The authors have developed a blackbox method that takes an ensemble of system trajectories and produces a quadratic Lyapunov function, but it is unclear what the minimum requirements on the number of trajectories and the length of each of the system runs is. While the optimization problem is a convex second-order cone program, egregious amounts of required data to arrive at a correct result quickly draw the approach's applicability into question.

## Comments 

The reviewer's main gripe with the manuscript lies in the restrictive nature of ellipsoids (quadratic forms) in identifying the domain of attraction of general nonlinear functions—this restrictive nature is clearly reflected in test problem 1, where a large part of the domain of attraction is lost because of the assumption of a zero-centered ellipsoid. The results could be obtained by a now-classical approach based on system identification followed by the solution of a sum-of-squares problem to obtain a quadratic Lyapunov function [1]. For this very reason, the reviewer sees little apparent novelty in the proposed approach.

The authors' work lacks a review on recent advances in determining Lyapunov functions directly from Koopman operators, namely by considering the intersection of zero level sets of Koopman eigenfunctions [2]. Koopman operators admit many data-driven approaches to their approximation directly from trajectory data, or they can be obtained analytically by an appropriately parameterized fitted dynamics model (see, e.g., [3]); see [4] for a very recent development of this approach along with references to related work on estimating Zubov functions. What is most noteworthy is that such approaches do not impose restrictions on the Lyapunov function structure (ansatz), unlike the quadratic structure that the authors impose in this manuscript.

For the reasons stated above, the reviewer deems the manuscript, in its present form, unfit for
publication in and presentation at ACC 2025.

Additional references:
[1] A. Papachristodoulou and S. Prajna, "On the construction of Lyapunov functions using the sum of squares decomposition," Proceedings of the 41st IEEE Conference on Decision and Control, 2002., Las Vegas, NV, USA, 2002, pp. 3482-3487 vol.3, doi: 10.1109/CDC.2002.1184414.
[2] A. Mauroy and I. Mezić, "Global Stability Analysis Using the Eigenfunctions of the Koopman Operator," in IEEE Transactions on Automatic Control, vol. 61, no. 11, pp. 3356-3369, Nov. 2016, doi: 10.1109/TAC.2016.2518918.
[3] Williams, Matthew O., Ioannis G. Kevrekidis, and Clarence W. Rowley. "A data–driven approximation of the Koopman operator: Extending dynamic mode decomposition." Journal of Nonlinear Science 25
[3] Tang, Wentao. "Koopman Operator in the Weighted Function Spaces and its Learning for the Estimation of Lyapunov and Zubov Functions." arXiv preprint arXiv:2410.00223 (2024).


# Reviewer 3

This paper deals with the estimation of Lyapunov functions from trajectory data. The paper is well-written, however, I find the the novelty and contribution of this work, due to the simplicity of the approach proposed to obtain the shape of the Lyapunov sub-level set via trajectory data, rather low. The author restricts the problem to quadratic Lyapunov functions and the sub-level set is determined using Monte Carlo. Since the approach determines a quadratic Lyapunov function, a simple comparison to a linear system which is obtained with the data to get a possible Lyapunov function is missing and it would be relevant to understanding the advantages of the proposed method. I consider the paper weak in its theoretical contribution, but from a practical perspective, it might be useful.
## Summary

This is a review of the paper “LyapInf: Data-Driven Lyapunov Function Inference for Stability Analysis of Nonlinear Dynamical Systems” by Tomoki Koike and Elizabeth Qian submitted to the 2025 American Control Conference.

The paper presents a data-driven approach to infer Lyapunov functions for nonlinear dynamical systems. The authors propose a method to estimate the ROA by minimizing the error on the Zubov’s PDE on the trajectory data of the system with appropriate simplifications and by making an aposteriori maximization of the sublevel set of the Lyapunov function through a Monte-Carlo search.


## Comment

I recommend this paper for approval, however, I have some doubts on the relevance of the contributions

The approach taken by the author is to minimize the error on Zubov’s PDE on the trajectory data of the system. In order to make this minimization tractable, the author simplified the problem by imposing quadratic prototypes for the Lyapunov function and selecting apriori static functions, namely h(x) in Eq. (4) (Similar relaxations have been used in the literature). In addition, the author establishes a somewhat arbitrary domain of interest and uses trajectory data that start from this domain, although some might not belong to the region of attraction and therefore affect the performance of the optimisation negatively. Hence, the choice of the domain of interest is crucial and should be discussed in more detail, which is not the case in the paper. Due to the simplicity of the author’s method to obtain a Lyapunov function candidate, I was expecting a clever way to deal with the determination of the sublevel set of the Lyapunov function. However, the authors simply use a typical Monte-Carlo search, which is not very innovative and lacks theoretical guarantees. The paper is well written and the results are interesting, but considering the huge amount of approaches using Zubov’s method since the 70s, I would like to see a more detailed comparison with the state-of-the-art methods and not only with the approach that leverages NNs. This latter comparison is not fair, since the NNs are known to be universal approximators and the author is using a very simple architecture, it is expected that the NNs will outperform the proposed method while requiring more data.


# Reviewer 4


## Summary

This paper aims to find a "good guess" of a Lyapunov function for a nonlinear system based on finite, imperfect data about its vector field. The guess -- Lyapunov ansatz -- is assumed to be quadratic, and the paper's proposed method is to find the ansatz parameters which best fit the equality that a Lyapunov function is known to satisfy from the Zubov's theorem.


## Comments

The paper is well-written, easy to read, honest about its contributions (perhaps an increasingly uncommon feature), and certainly seems to be technically correct. My only "complaint", and it's really a major one, is that I'm not sure about the wealth of its contribution.

Simply put, the paper says "we don't know what a true Lyapunov function of a system could be, so we will try to guess a quadratic function". Of course, quadratic forms of Lyapunov are popular guesses (if nothing else, because they fit linear systems), so there is nothing new in this idea. Naturally, quadratic functions are often insufficient for any kind of global results for the obvious reasons that the system might not admit a Lyapunov function even close to quadratic.

Now, the claimed technical novelty of this paper is to say "our best guess for the parameters of the Lyapunov function will be to try to find the parameters that best fit the equality from Zubov's theorem". I am willing to believe that this is formally novel, although inferring other classes of Lyapunov functions through Zubov's theorem has been done previously. After this idea is introduced, the technical innovation in the paper really stops -- the data is obtained through a standard finite difference scheme, and the optimization problem is essentially least squares, with unusual constraints which disables the paper from providing an analytical solution.

(By the way, I suspect there is a mistake in either eq. (9) or eq. (13), as (9) is linear in P and its supposed equivalent in (13) is quadratic; perhaps there are squares missing from (9).)

There are no theoretical guarantees -- which is natural because (i) the data itself, obtained using finite differences, contains errors which have not been quantified, (ii) it is possible that the optimization solver does not find an exact solution, and, of course most importantly (iii) there is nothing more than an intuitive explanation for why "being close" to satisfying Zubov's theorem equation should also mean that the function is "close to Lyapunov" (and in what sense). The numerical experiments confirm this lack of guarantees: while the results are certainly what one would expect from this method, they show that the true regions of attraction might be significantly larger than those developed in this paper. In fact, since the numerical experiments already rely on solving an additional optimization problem using Monte Carlo sampling, it would be interesting to see whether this paper's entire contribution could be replaced by just sampling a large number of different matrices P instead of trying to find a good guess using the heuristic presented before.

Altogether, I think this paper is a valuable first step in exploring computationally simple, data-driven Lyapunov synthesis, but I encourage the authors to consider the substantial (not just formal) novelty of their approach compared to previous work, and possible theoretical guarantees that could be obtained for the proposed method, at least for a particular (if "academic" or "toy") subclass of nonlinear systems.


# Reviewer 5

## Summary

The paper proposes a data-driven inference method for the Lyapunov function which is further used for finding the subset of the region of attraction. The method is based on Zubov’s theorem and with a simplification of only inferring a quadratic Lyapunov function with higher orders neglected because of using data. The authors also provide insights into practical implementation and input considerations. The results and discussion are sufficient and comprehensive and the comparison with the other works is strong.

## Comments

1. The reviewer feels that the title may be too broad since a nonlinear autonomous system is studied whereas the title claims the whole ‘nonlinear dynamical systems.’ Authors may provide more justification if they want to keep the title.
2. In Theorem 2, it may explain partial $\mathcal{D}$.
3. In Section III B, the word ‘respectively’ may not be needed.
4. In Section III C, 1) Training Data, is there a requirement on data sampling interval if the data are generated using Equation (7)?
5. The reviewer finds that Equation (22) is difficult to understand as it is different from its description: we seek the maximum $c$ such that … $\dot{\tilde{V}}<0$ region. Authors may provide more explanation.
6. In Section IV C, what is the ‘direct numerical evaluation’? A reference may be provided. It would provide a significant tutorial benefit to readers.

# Reviewer 6

## Summary

This paper addresses the problem of identifying Lyapunov functions for black-box nonlinear dynamical systems using a data-driven approach, which is an important topic. However, it lacks crucial details about the proposed approach, and the advantages over existing literature are not convincingly demonstrated. Specific comments are as follows:

## Comments

1. The statement of Zubov’s Theorem in Theorem 2 appears to be missing an additional condition.
2. In Section III.B, it is stated that "To enforce the positive definiteness of P, diagonal entries of P are constrained to be larger than a small positive constant." However, positive diagonal entries alone are not sufficient for P to be positive definite.
3. It is unclear whether the resulting Lyapunov function candidates obtained by solving optimization problems (13) or (9) are guaranteed to satisfy the conditions of Zubov’s Theorem. If not, how can it be verified that they are valid Lyapunov functions?
4. The simulation results do not convincingly support the claim that the proposed method can find "near-maximal ellipsoid estimates of the system's domain of attraction." Additionally, the data requirement does not appear to be significantly reduced compared to neural network-based approaches.



