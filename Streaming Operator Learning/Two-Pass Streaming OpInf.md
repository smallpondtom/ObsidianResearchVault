# Two-Pass Streaming-OpInf

## Todos
- [ ] Change the Tikhonov regularization to accept general regularizations in matrix form 📅 2024-12-11  ⏫ 
- [ ] Complete the experiments including the 3D Channel flow by the end of this year 📅 2024-12-31
- [ ] Complete most of the write up except for the intro, discussion, and conclusion of the Journal 📅 2024-12-31
- [ ] Complete the bullet points for the intro, discussion, and conclusion for the journal  📅 2024-12-31

## Randomized Eigenspace Merging

Below is a step-by-step randomized version of the baseline merging algorithm. This algorithm replaces the exact QR and SVD steps with randomized sketches, reducing computational cost while producing a good low-rank approximation with high probability. The key idea is to use a random projection to first reduce the problem size before performing expensive decompositions.

**Setup:**  
You have two partial SVDs (or truncated eigenspaces),

- $P_1 = (U_1 \in \mathbb{R}^{m \times k_1} , S_1 \in \mathbb{R}^{k_1 \times k_1})$ 
- $P_2 = (U_2 \in \mathbb{R}^{m \times k_2}, S_2 \in \mathbb{R}^{k_2 \times k_2})$   

and parameters:

- Truncation factor $k$  (desired rank, with $k \leq k_1+k_2$ )
- Decay factor $\gamma$
- An oversampling parameter $p$ (a small integer like 5 or 10 to improve approximation quality)
- Define $\ell = k + p$ for the sketch dimension.

**Goal:** Merge $P_1$ and $P_2$ into a new truncated SVD $P = (U, S)$ that approximates the combined subspace spanned by $[\gamma U_1 S_1, U_2 S_2]$ , while keeping only the top $k$ singular components.

---

**Randomized Baseline Merge Algorithm:**

**Input:**

- $U_1, S_1, U_2, S_2$
- Truncation factor $k$
- Decay factor $\gamma$
- Oversampling $p$ (with $\ell = k+p$)
- A random test matrix $\Omega \in \mathbb{R}^{(k_1+k_2) \times \ell}$, typically with i.i.d. Gaussian entries.

**Output:**

- $U \in \mathbb{R}^{m \times k}$
- $S \in \mathbb{R}^{k \times k}$

**Steps:**

1. **Form the merged matrix A:**  
    Construct the combined matrix:
	$$ 
		A = [\gamma U_1 S_1 \;\; U_2 S_2] \in \mathbb{R}^{m \times (k_1+k_2)}.
	 $$
    This matrix represents the combined subspace you want to approximate.
    
2. **Sketch the subspace with random projection:**  
    Multiply $A$ by the random test matrix $\Omega$:
	$$    
	Y = A \Omega \in \mathbb{R}^{m \times \ell}.
	$$  
    The matrix $Y$ captures the dominant column space of $A$ with high probability.
    
3. **Orthonormalize the sketch:**  
    Compute a QR factorization of $Y$ to obtain an orthonormal basis $Q_\Omega$:
	$$ 
    Q_\Omega, \_ = \text{qr}(Y).
	$$  
    Now $Q_\Omega \in \mathbb{R}^{m \times \ell}$ is an orthonormal matrix whose columns form a good approximation to the range of $A$.
    
4. **Reduce the problem size:**  
    Project $A$ onto the subspace spanned by $Q_\Omega$ to get a much smaller matrix $B$:
	$$ 
	    B = Q_\Omega^T A \in \mathbb{R}^{\ell \times (k_1+k_2)}.
	$$    
    Now you only need to deal with $B$, which is significantly smaller than $A$.
    
5. **Compute a truncated SVD of $B$:**  
    Compute the (exact) SVD of $B$:
   $$ 
    B = U_B S_B V_B^T.
	$$ 
    Then truncate to the top $k$ singular values and vectors:
    - $U_B' = U_B[:,1:k]$, $S = S_B[1:k]$, and $V_B' = V_B[:,1:k]$.
6. **Map back to original space and finalize $U$ and $S$:**  
    The approximation to $A$ can be written as:
	$$ 
	    A \approx Q_\Omega U_B' S (V_B')^T.
	$$  
    The truncated SVD of $A$ is then given by:
	$$ 
	    U = Q_\Omega U_B' \in \mathbb{R}^{m \times k}, \quad S = \text{diag}(S[1:k]) \in \mathbb{R}^{k \times k}.
	$$  
    Thus, the merged subspace is represented by $(U, S)$, where $U$ contains the top kk left singular vectors and $S$ the top $k$ singular values.
    

---

**Remarks:**

- If $(k_1 + k_2)$ is large, this approach avoids forming and factorizing a large matrix directly. Instead, it uses a random sketch $Y = A\Omega$ to quickly approximate the range of $A$.
- The oversampling parameter pp ensures with high probability that the approximation is close to what you would get with a deterministic SVD. Common choices are $p = 5$ or $p = 10$.
- If needed, you can improve the approximation quality by applying power iterations or leverage block randNLA techniques. These details are covered in the literature by Halko, Martinsson, and Tropp (2011).

This step-by-step algorithm completely replaces the deterministic QR and SVD steps in the baseline merge with a fully randomized procedure, yielding a randomized approximation to the merged eigenspace.