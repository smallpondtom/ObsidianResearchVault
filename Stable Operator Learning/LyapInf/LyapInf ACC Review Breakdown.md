
## Easy Fixes
- Clarify the notion/domain $\mathcal{D}$ in Theorem 2 before using it
- Indicate the truncation error in Theorem 6
- "egregious amount of data" even though its a convex optimization --> I have already verified that this method works for significantly less data for simple 2D examples. Two reviewers mentioned that the amount of data is not significantly less than the neural network methods and thus is not a strong argument
- lack review on recent work of determining Lyapunov functions from Koopman operators (Mauroy and Mezic, 2016) (Tang 2024)
- Comparison with the Lyapunov function obtained from the linearized system is missing
- Comparison with other SOTA methods such as neural-network based methods (not really easy but doable)
- the title seems unfit since it is a bit too broad by claiming it works for the whole "nonlinear dynamical systems"
- Clarify the domain of attraction estimation problem (the optimization problem in equation (22))
- Zubov's theorem seems to be missing an additional condition (? not sure about this one)

## Requires Deeper Dive
- Verify whether exponential stability is necessary instead of just asymptotic stability when studying the Zubov equation.
- For a quadratic ansatz, it seems like the solution is being forced into a projection onto a low-resolution basis, which may not cause the residual error to reduce. 
- Does merely minimizing the residual error yield a correct approximation of the true solution? (**theoretical guarantees**)
- How can you formally argue the uniqueness of the solution under specific conditions? (**theoretical guarantees**)
- Have you considered unbounded solutions of the Zubov's equation? -- Additional considerations, such as boundary conditions or evolution speed at large domains should be taken into account.
- What are the effects of sampling unstable data points? Any theoretical analyses?
- Using a Monte-Carlo based method to compute the domain of attraction
	- *NOTE*: I have implemented a convex optimization based approach to compute this which works for quadratic and cubic systems (however, only for quadratic Lyapunov functions)
	- However, considerations of a new method might be helpful to boost the novelty
- A more thorough discussion of the domain of interest and how the data sampling is affected based on which domain the data is sampled from
- The "unusual" positive-definiteness constraint disables the paper from providing an analytical solution, which in turn prohibits theoretical guarantees of the solution
- Quantify the errors (related the theoretical guarantees)
	- the error coming from the data itself, namely the finite difference
	- does the optimization find the exact solution?
	- quantify in what sense the solution is "close to Lyapunov" by minimizing the residual of the Zubov equation
- requirements on the data sampling interval since you are using finite differencing? (**clarify the assumptions of the data**)
- The claim "near-maximal" is not convincing --> this is related to the fact that LyapInf gives a quadratic ansatz and for unbounded DoAs the estimate may not be the best


## Show-Stoppers


## Ideas provided by the reviewers
- it would be interesting to see whether this paper's entire contribution could be replaced by just sampling a large number of different matrices P instead of trying to find a good guess using the heuristic presented before