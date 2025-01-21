# Making SketchySVD more efficient

By combining the Randomized Eigenspace Merging (REM) algorithm with SketchySVD we can avoid working in the supposedly large dimension $n$ which emanates from the large data matrix $\mathbf{A}\in\mathbb{R}^{m\times n}$ . This new algorithm, which I call the **MergingSketchySVD** is essentially a divide-and-conquer approach of the original algorithm.

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
    Q_\Omega, \_\_ = \text{qr}(Y).
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
- The oversampling parameter $p$ ensures with high probability that the approximation is close to what you would get with a deterministic SVD. Common choices are $p = 5$ or $p = 10$. For details on the selection of the oversampling parameter $p$ refer to "Randomized Numerical Linear Algebra..." by Martinsson and Tropp (2020).
- If needed, you can improve the approximation quality by applying power iterations or leverage block randNLA techniques. These details are covered in the literature by Halko, Martinsson, and Tropp (2011).

> This step-by-step algorithm completely replaces the deterministic QR and SVD steps in the baseline merge with a fully randomized procedure, yielding a randomized approximation to the merged eigenspace.


# Optimized Matvec for outer product structure

The key observation is that if

$$H \;=\; a \; e_i^\top$$

then $H$ is a rank-1 matrix whose only nonzero column is the $i$-th column (and that column equals aa). Multiplying by $H$ on the right (i.e., $\cdot \,H$) selects or updates only the $i$-th column of whatever matrix it multiplies; multiplying by $H$ on the left (i.e., $H \,\cdot$) selects or updates only the $i$-th row.

Hence, rather than doing a “full” matrix multiplication with $H$—most of whose columns/rows are zero—you can directly update the single affected column/row in-place. Below is a conceptual breakdown of each term in your snippet.

---

The original update scheme in Tropp et al. [2019] is 

$$
\begin{aligned}
	\mathbf{X} &\leftarrow \eta\mathbf{X} + \nu\boldsymbol{\Xi}\mathbf{H} \\
	\mathbf{Y} &\leftarrow \eta\mathbf{Y} + \nu\mathbf{H}\boldsymbol{\Omega}^\top\\
	\mathbf{Z} &\leftarrow \eta\mathbf{Z} + \nu\boldsymbol{\Phi}\mathbf{H}\boldsymbol{\Psi}^\top \\
	\mathbf{E} &\leftarrow \eta\mathbf{E} + \nu\boldsymbol{\Theta}\mathbf{H} \\
\end{aligned}
$$
 where $\eta$ and $\nu$ are weighting factors. This can be naively be implemented in Julia as

```Julia
sketchy.X *= η 
sketchy.X += ν * (sketchy.Ξ * H) 
sketchy.Y *= η 
sketchy.Y += ν * (H * sketchy.Ω') 
sketchy.Z *= η 
sketchy.Z += ν * (sketchy.Φ * H * sketchy.Ψ') 
sketchy.E *= η 
sketchy.E += ν * (sketchy.Θ * H)
```
 
---

## 1. `sketchy.X += ν * (sketchy.Ξ * H)`

Here,

$$\text{sketchy}.\Xi * H \;=\; \text{sketchy}.\Xi \;\bigl(a\, e_i^\top\bigr) \;=\; (\text{sketchy}.\Xi \, a)\;e_i^\top.$$

- $\text{sketchy.Ξ} \, a$ is just a vector (let’s call it `z = sketchy.Ξ * a`).
- Multiplying by $e_i^\top$ on the right places `z` into the $i$-th column (and zeros elsewhere).
- Note: $\nu$ is a weighting factor (*it's not the alphabet "v")

Hence, instead of a full matrix-matrix multiply, you can directly do:

```julia
# Scale X by η
sketchy.X .*= η

# Compute the update vector once
z = sketchy.Ξ * a

# Add that vector to the i-th column of X
@views sketchy.X[:, i] .+= ν .* z
```

That single line `sketchy.X[:, i] .+= ν .* z` replicates what multiplying by the rank-1 matrix $a\,e_i^\top$ would have done, but only touches the $i$-th column in-place.

---

## 2. `sketchy.Y += ν * (H * sketchy.Ω')`

Now we have

$$H\,\text{sketchy.Ω}' \;=\; \bigl(a\, e_i^\top\bigr)\,\text{sketchy.Ω}' \;=\; a\,\bigl(e_i^\top\,\text{sketchy.Ω}'\bigr).$$

- $e_i^\top \,\text{sketchy.Ω}'$ picks out the $i$-th row of $\text{sketchy.Ω}'$. Let’s call that row vector `row = sketchy.Ω'[i, :]`.
- Then we multiply $a$ (a column vector) by that row, giving a rank-1 update `a * row`.
- Note: $\nu$ is a weighting factor (*it's not the alphabet "v")

Hence a more efficient update is:

```julia
sketchy.Y .*= η

# Extract the i-th row from Ω'
row = sketchy.Ω'[i, :]   # this is a 1 x (something) row vector
# The vector a we already have (it’s the same as in H = a*e_iᵀ)

# Now do the rank-1 update: for each column j, add ν*a*(row[j]) to Y[:, j].
# In Julia, you can just write:
sketchy.Y .+= ν .* (a * row)
```

---

## 3. `sketchy.Z += ν * (sketchy.Φ * H * sketchy.Ψ')`

We can group this as:

$$\text{sketchy.Φ} \,\bigl(\,a\, e_i^\top\bigr) \,\text{sketchy.Ψ}' \;=\; \bigl(\text{sketchy.Φ}\,a\bigr) \;\bigl(e_i^\top\, \text{sketchy.Ψ}'\bigr).$$

- First compute `z = sketchy.Φ * a`.
- Then compute `row = e_i' * sketchy.Ψ'`, which again picks out row `row = sketchy.Ψ'[i, :]`.
- Finally do a rank-1 update:

```julia
sketchy.Z .*= η

z   = sketchy.Φ * a               # column vector
row = sketchy.Ψ'[i, :]            # row vector

sketchy.Z .+= ν .* (z * row)
```

This replaces the expensive multiply $\text{sketchy.Φ} \; H \; \text{sketchy.Ψ}'$ by two small vector multiplications and a rank-1 update.

---

## 4. `sketchy.E += ν * (sketchy.Θ * H)`

Similarly,

$$\text{sketchy.Θ}\;\bigl(a\,e_i^\top\bigr) \;=\; (\text{sketchy.Θ} \, a)\; e_i^\top.$$

- Compute `v = sketchy.Θ * a`.
- Then place `v` into the $i$-th column:

```julia
sketchy.E .*= η

v = sketchy.Θ * a
@views sketchy.E[:, i] .+= ν .* v
```

---

## Putting it all together

A sketch of the fully “rank-1-aware” update might look like:

```julia
function update_sketchy!(sketchy, a, i, η, ν)
    # 1) X update
    sketchy.X .*= η
    @views sketchy.X[:, i] .+= ν .* (sketchy.Ξ * a)
    
    # 2) Y update
    sketchy.Y .*= η
    row_Ω = sketchy.Ω'[i, :]   # picks out i-th row
    sketchy.Y .+= ν .* (a * row_Ω)  # rank-1 update

    # 3) Z update
    sketchy.Z .*= η
    v   = sketchy.Φ * a
    row_Ψ = sketchy.Ψ'[i, :]
    sketchy.Z .+= ν .* (v * row_Ψ)

    # 4) E update
    sketchy.E .*= η
    @views sketchy.E[:, i] .+= ν .* (sketchy.Θ * a)
end
```

> Each of those rank-1 updates avoids multiplying by columns (or rows) that would have been zero anyway in the outer product $a \, e_i^\top$. This will be much more efficient if your matrices are large and $H$ is truly an outer product of two vectors, since you no longer do full matrix–matrix multiply operations but just the parts that are nonzero.