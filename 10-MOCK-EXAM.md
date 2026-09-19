# MOCK EXAM — Sunday evening, timed

**Fresh numbers, same structure as Assignment 1.** Do it on paper, closed-book, **60
minutes**. Then mark yourself honestly against the solutions below. Wherever you lose
marks, reread that one topic file — not all of them.

Write steps, not just answers. Write one sentence of interpretation after each result.

---

## Q1 — Norms and distance [4]
Let `x = (2, −3, 6)`.
1. Compute `‖x‖₁`, `‖x‖₂`, `‖x‖∞`.
2. Relative to `q = (0,0)`, compare `p = (5,0)` and `r = (3,3)`. Which is closer under
   L1? Under L2?
3. In one or two sentences, explain why changing the norm can change "nearest".

## Q2 — Matrix as a function, column picture, rank [5]
`A = [[2,1,3],[1,0,1]]`, `z = (1, 2, −1)`.
1. Compute `Az` row-by-column.
2. Recompute as a linear combination of A's columns.
3. Find rank(A) and state one dependence relation among the columns.
4. Is A rank-deficient in the `rank < min(m,n)` sense? Does it have full column rank?
5. Without a determinant, is `AᵀA` invertible?

## Q3 — Dot product and cosine similarity [4]
`u = (2, 1)`, `w = (−3, 1)`.
1. Compute `u·w`.
2. Compute the cosine similarity and the angle.
3. What does the sign tell you about the angle?
4. Can you conclude the vectors point in exactly opposite directions? Explain.
5. Replace `u` by `10u`. What changes in the raw dot product? What is unchanged in cosine
   similarity, and why?

## Q4 — Projection and residual [5]
`a = (1, 3)`, `b = (5, 5)`.
1. Derive the scalar coefficient `c`.
2. Find `projₐ(b)`.
3. Find the residual `r`.
4. Verify numerically that `r ⊥ a`.
5. Explain geometrically why this is the closest point on the line spanned by `a`.

## Q5 — Determinant, trace, eigenvalues [6]
`B = [[5, 2], [2, 2]]`.
1. `det(B)` and `tr(B)`.
2. Characteristic equation and both eigenvalues.
3. One eigenvector for each eigenvalue.
4. Verify one eigenpair by direct multiplication.
5. Explain why B is invertible.
6. Confirm the sum and product of eigenvalues against trace and determinant.

## Q6 — Diagonalization [4]
Using Q5's eigenpairs:
1. Construct P and D with `B = PDP⁻¹`.
2. Why does this make `B¹⁰` easier? (Do not compute it.)
3. **Is it valid to replace `P⁻¹` with `Pᵀ` here?** State the condition and test it on
   this matrix. *(Careful — think before you answer. Compare with Assignment Q6.)*
4. Give one reason SVD is more generally applicable than eigendecomposition.

## Q7 — Reading an SVD [5]
A matrix `C ∈ ℝ⁸ˣ⁵` (8 documents, 5 terms) has rank-3 approximation `C₃ = U₃Σ₃V₃ᵀ`.
1. Dimensions of `U₃`, `Σ₃`, `V₃ᵀ`. Verify they multiply to C's shape.
2. Which singular vectors are directions in document space? In term space?
3. Does SVD exist even though C isn't square? Justify with the actual reason.
4. A library returns `u₂` and `v₂` both with every sign reversed. Error or not? Explain.
5. Why can a rank-3 representation expose latent topics with no labels supplied?

## Q8 — Truncated SVD and energy [5]
Singular values `σ = (6, 4, 2, 1)`.
1. Total energy.
2. Proportion retained at rank 1, 2 and 3.
3. Frobenius reconstruction error at rank 2.
4. Smallest k retaining ≥95% of the energy.
5. State precisely what Eckart–Young guarantees.

## Q9 — PCA via SVD [5]
    X = [ 1  1 ]
        [ 2  4 ]
        [ 4  2 ]
        [ 5  5 ]
1. Compute the row mean and construct `Xc`.
2. Compute `XcᵀXc`; find its eigenvalues and eigenvectors.
3. State PC1. Does it match the layout of the four points?
4. Proportion of variance explained by each component.
5. How do the eigenvectors/eigenvalues of `Σcov = (1/n)XcᵀXc` relate to those of `XcᵀXc`?
6. What plays the role here that the squared singular values played in Q8?

## Q10 — Reflection [2]
In 2–3 sentences: the course argues that reliable, scalable data systems require formal
mathematical tools; Tukey argued that data analysis is a broader empirical discipline in
which practical judgement matters as much as formal technique. How do these two views
relate? Support your answer with one concrete example from this module.

---
---

# SOLUTIONS

## Q1
1. `‖x‖₁ = 2+3+6 = **11**`. `‖x‖₂ = √(4+9+36) = √49 = **7**`. `‖x‖∞ = **6**`.
   *(Check 11 ≥ 7 ≥ 6 ✓)*
2. **L1:** `‖p‖₁ = 5`, `‖r‖₁ = 6` → **p is closer.**
   **L2:** `‖p‖₂ = 5`, `‖r‖₂ = √18 ≈ 4.24` → **r is closer.**
3. L2 squares each coordinate difference, so a single large deviation (p's 5) is penalised
   far more heavily than several moderate ones summing to more (r's 3 and 3). "Nearest" is
   therefore not a property of the data alone — it depends on which ruler you chose.

## Q2
1. Row 1: `2(1) + 1(2) + 3(−1) = 2 + 2 − 3 = 1`. Row 2: `1(1) + 0(2) + 1(−1) = 0`.
   → **`Az = (1, 0)`**
2. `1·(2,1) + 2·(1,0) + (−1)·(3,1) = (2,1) + (2,0) + (−3,−1) = (1,0)` ✓ same.
3. Columns `(2,1)`, `(1,0)`, `(3,1)`. The first two are independent, and
   **`col₃ = col₁ + col₂`**. So **rank(A) = 2**.
4. A is 2×3 → `min(m,n) = 2`, and rank = 2, so **not rank-deficient — it is full rank.**
   But it has **3 columns with rank 2**, so it does **not** have full column rank.
   *(A wide matrix can never have full column rank.)*
5. `rank(AᵀA) = rank(A) = 2`, but `AᵀA` is 3×3. Invertibility requires full column rank,
   which A lacks. **`AᵀA` is singular — not invertible.**

## Q3
1. `u·w = 2(−3) + 1(1) = −6 + 1 = **−5**`
2. `‖u‖ = √5`, `‖w‖ = √10`. `cos θ = −5/√50 = −5/(5√2) = −1/√2 ≈ **−0.707** → θ = **135°**`
3. Negative ⇒ the angle is **greater than 90°** (obtuse). That is the entire content of the
   sign.
4. **No.** Exactly opposite requires `cos θ = −1`; here `cos θ ≈ −0.707`, giving 135°.
   Negative cosine covers the whole range 90°–180°; 180° is just one extreme point inside it.
5. Raw dot product scales by 10 → **−50**. Cosine similarity is **unchanged at −0.707**,
   because the factor of 10 appears in the numerator and again in `‖10u‖ = 10‖u‖`, so it
   cancels. Cosine measures direction only, not magnitude.

## Q4
1. Closest point is `c·a`, and the residual must be perpendicular to `a`:
   `(b − ca)·a = 0` → `b·a − c(a·a) = 0` → `c = (a·b)/(a·a)`.
   `a·b = 1(5) + 3(5) = 20`, `a·a = 1 + 9 = 10` → **`c = 2`**
2. `projₐ(b) = 2·(1,3) = **(2, 6)**`
3. `r = (5,5) − (2,6) = **(3, −1)**`
4. `r·a = 3(1) + (−1)(3) = 3 − 3 = **0** ✓`
5. `b`, its projection, and any other point on the line form a right triangle in which the
   segment from `b` to that other point is the hypotenuse. The hypotenuse is always the
   longest side, so every other point on the line is strictly farther from `b` than the foot
   of the perpendicular.

## Q5
1. `det(B) = (5)(2) − (2)(2) = 10 − 4 = **6**`. `tr(B) = 5 + 2 = **7**`.
2. `λ² − 7λ + 6 = 0` → `(λ−6)(λ−1) = 0` → **λ = 6 and λ = 1**
3. λ=6: `B − 6I = [[−1,2],[2,−4]]` → `−v₁ + 2v₂ = 0` → `v₁ = 2v₂` → **v = (2, 1)**
   λ=1: `B − I = [[4,2],[2,1]]` → `4v₁ + 2v₂ = 0` → `v₂ = −2v₁` → **v = (1, −2)**
4. `B(2,1) = (5·2 + 2·1, 2·2 + 2·1) = (12, 6) = 6·(2,1)` ✓
5. `det(B) = 6 ≠ 0`, so B is invertible. Equivalently neither eigenvalue is zero, and
   equivalently B is full rank (rank 2) — its columns are independent, so no dimension is
   collapsed and the map can be undone.
6. `6 + 1 = 7 = tr(B)` ✓  and  `6 × 1 = 6 = det(B)` ✓

## Q6
1. `P = [[2, 1], [1, −2]]`, `D = [[6, 0], [0, 1]]` (column order matched to the diagonal).
2. `B¹⁰ = P D¹⁰ P⁻¹`, and `D¹⁰ = diag(6¹⁰, 1¹⁰)` — raising a diagonal matrix to a power is
   just raising each diagonal entry, replacing ten matrix multiplications with two scalar
   powers and two matrix products. It also shows the long-run behaviour immediately: the
   `λ = 6` direction dominates completely while the `λ = 1` direction stays fixed.
3. **Yes, valid here — but only after normalising P.**
   The condition is that P must be **orthogonal** (columns mutually orthogonal *and* of unit
   length), which is guaranteed when **B is symmetric**.
   Test: `B = Bᵀ` ✓ (entry (1,2) = entry (2,1) = 2). And directly,
   `(2,1)·(1,−2) = 2 − 2 = 0` ✓ — the eigenvectors are orthogonal.
   But `‖(2,1)‖ = √5 ≠ 1`, so you must first divide each column by `√5`. With
   `P = (1/√5)[[2,1],[1,−2]]`, then `P⁻¹ = Pᵀ`.
   ⚠ **Contrast with Assignment Q6**, where `B = [[4,1],[2,3]]` is *not* symmetric, its
   eigenvectors are *not* orthogonal, and the answer there is **no**. The answer depends
   entirely on the matrix — never memorise one or the other.
4. SVD exists for **every** matrix with no exceptions — including non-square matrices (a
   768×1024 image has no eigenvectors at all) and square matrices that aren't diagonalizable
   (a shear with a repeated eigenvalue has too few independent eigenvectors). Eigendecomposition
   requires a square matrix with a full independent eigenbasis.

## Q7
1. `U₃`: **8 × 3**. `Σ₃`: **3 × 3**. `V₃ᵀ`: **3 × 5**.
   Check: `(8×3)(3×3)(3×5) → 8×5` ✓ — C's shape.
2. **U (left singular vectors) → document space** (8 rows = 8 documents).
   **V (right singular vectors) → term space** (5 columns = 5 terms).
3. **Yes.** V's columns are eigenvectors of `AᵀA` and U's of `AAᵀ`; both of those are
   **symmetric for any A** (since `(AᵀA)ᵀ = AᵀA`), and the **spectral theorem** guarantees
   every symmetric matrix has a full set of real orthogonal eigenvectors. Diagonalization can
   fail on A itself, but never on `AᵀA` or `AAᵀ` — that is the whole guarantee, and it does
   not care whether A is square.
4. **Not an error.** Singular vectors are unique only up to sign: flipping `u₂` and `v₂`
   **together** leaves `σ₂u₂v₂ᵀ` unchanged, so the reconstruction `C₃` is identical. Only the
   grouping of points is meaningful, never which side of zero they fall on.
5. Documents on the same subject reuse the same vocabulary, so C's rows are close to linear
   combinations of a few underlying patterns — the matrix has low **effective rank**. SVD
   finds the directions carrying the most variation, and those directions *are* the shared
   vocabulary patterns. The structure was already present in the co-occurrence counts; SVD
   only exposed it, which is why no labels were needed.

## Q8
1. `36 + 16 + 4 + 1 = **57**`
2. rank-1: `36/57 ≈ **63.16%**`; rank-2: `52/57 ≈ **91.23%**`; rank-3: `56/57 ≈ **98.25%**`
3. `‖A − A₂‖_F = √(σ₃² + σ₄²) = √(4 + 1) = √5 ≈ **2.236**`
4. k=2 gives 91.23% ✗; k=3 gives 98.25% ✓ → **k = 3**
5. Among **all** matrices of rank k — not only truncated SVDs — the truncated SVD `Aₖ`
   minimises the distance to A, in **both** the Frobenius and spectral norms, and the
   remaining error is determined exactly by the singular values that were discarded.

## Q9
1. Column means: `(1+2+4+5)/4 = 3` and `(1+4+2+5)/4 = 3`. Mean `= (3, 3)`.
       Xc = [ −2  −2 ]
            [ −1   1 ]
            [  1  −1 ]
            [  2   2 ]
   *(Each column sums to 0 ✓)*
2. `Σx² = 4+1+1+4 = 10`; `Σy² = 4+1+1+4 = 10`; `Σxy = 4−1−1+4 = 6`.
       XcᵀXc = [ 10   6 ]
               [  6  10 ]
   `tr = 20`, `det = 100 − 36 = 64`. `λ² − 20λ + 64 = 0` → `(λ−16)(λ−4) = 0` →
   **λ₁ = 16, λ₂ = 4**.
   λ=16: `[[−6,6],[6,−6]]` → `v₂ = v₁` → **(1, 1)**
   λ=4: `[[6,6],[6,6]]` → `v₂ = −v₁` → **(1, −1)**
   *(Orthogonal, as they must be for a symmetric matrix ✓)*
3. **PC1 = (1,1)/√2** — the 45° diagonal. **Yes, it matches**: plotting (1,1), (2,4), (4,2),
   (5,5) shows the points spread mainly from bottom-left to top-right along that diagonal,
   with only a smaller spread across it.
4. PC1: `16/20 = **80%**`. PC2: `4/20 = **20%**`.
5. `Σcov = (1/4)XcᵀXc`. Scaling a matrix by a constant **does not change its eigenvectors**,
   so the **principal component directions are identical**. Every eigenvalue is divided by 4:
   `16/4 = 4` and `4/4 = 1`. The **proportions are unchanged** (`4/5 = 80%`), since the
   constant cancels in the ratio — which is why both routes give the same PCA.
6. The **eigenvalues of `XcᵀXc`** play exactly the role the squared singular values `σᵢ²`
   played in Q8 — and in fact they *are* the squared singular values, since
   `XcᵀXc = VΣ²Vᵀ`. "Explained variance" in PCA and "proportion of energy retained" in
   truncated SVD are the same quantity in different vocabulary.

## Q10 — what a full-mark answer contains
Three moves, in your own words:
1. The course's position: formal tools (linear algebra, calculus, probability) are what make
   a system behave **predictably and reliably at scale**, rather than working once by accident.
2. Tukey's position: data analysis is a **broader empirical discipline** than mathematical
   statistics — practical judgement about messy real problems matters as much as technique.
3. Reconcile them as **complementary, not opposed**, with a concrete example from this module.
   Any of these works: Eckart–Young guarantees the rank-k approximation is optimal, but
   **nothing in the mathematics tells you what k should be** — that is judgement (energy
   target, elbow, domain constraint). Or: L1 and L2 both give mathematically correct but
   *different* answers to "who is nearest", so choosing the ruler is judgement. Or: the
   algebra of PCA runs perfectly on unstandardised data and returns a meaningless PC1 — only
   judgement about units catches that.
   **The key sentence:** formal technique without judgement gives rigorous answers to the
   wrong question; judgement without formal technique doesn't scale or generalise.

---

## Marking yourself

| Score | What to do with Sunday night |
|---|---|
| 40–45 | You're ready. Read the cheat sheet Monday morning and stop. |
| 30–39 | Solid. Reread only the topic files where you dropped marks, redo those drills. |
| 20–29 | Redo every drill in files 01–08, worked on paper. Don't read passively. |
| < 20 | Focus Monday-morning effort on **Q1–Q5** only. Those are the most mechanical marks (24 of 45) and the most reliably recoverable in one evening. |

---

# BONUS QUESTIONS — added after auditing the slide images

These cover two things that live **only inside images** on the lecture slides, so they're
easy to miss and therefore worth being ready for.

## B1 — Computing a small SVD by hand [5]
`A = [[3, 0], [0, 2], [0, 0]]`
1. Give the sizes of U, Σ, Vᵀ in the full SVD.
2. Compute `AᵀA` and its eigenvalues.
3. State the singular values and rank(A).
4. Find V.
5. Find `u₁` using `uᵢ = Avᵢ/σᵢ`.

## B2 — PCA scores [5]
Points `(1,1), (3,3), (5,5), (3,1), (1,3)`.
1. Find the mean and the centered data.
2. Compute `XcᵀXc`, its eigenvalues and eigenvectors.
3. State PC1 as a **unit** vector, and the variance explained by each component.
4. Compute the **score on PC1** for every point.
5. One sentence: what do the scores tell you that the raw coordinates didn't?

---

## BONUS SOLUTIONS

### B1
1. A is 3×2 → `U`: **3×3**, `Σ`: **3×2**, `Vᵀ`: **2×2**. Check `(3×3)(3×2)(2×2) → 3×2` ✓
2. `AᵀA = [[9, 0], [0, 4]]` (already diagonal) → eigenvalues **9 and 4**.
3. `σ₁ = √9 = **3**`, `σ₂ = √4 = **2**`. Both non-zero → **rank(A) = 2**.
4. Eigenvectors of a diagonal matrix are the standard axes: `v₁ = (1,0)`, `v₂ = (0,1)`.
   → **`V = I₂`** (order matters: v₁ goes with the larger eigenvalue 9).
5. `u₁ = Av₁/σ₁ = A(1,0)/3 = (3,0,0)/3 = **(1, 0, 0)**` — unit length ✓

### B2
1. Means: `(1+3+5+3+1)/5 = 2.6`, `(1+3+5+1+3)/5 = 2.6`. Mean `= (2.6, 2.6)`.
   Centered: `(−1.6,−1.6), (0.4,0.4), (2.4,2.4), (0.4,−1.6), (−1.6,0.4)`
   *(Columns sum to 0 ✓)*
2. `Σx² = 2.56+0.16+5.76+0.16+2.56 = 11.2`; `Σy² = 11.2` by symmetry;
   `Σxy = 2.56+0.16+5.76−0.64−0.64 = 7.2`
       XcᵀXc = [ 11.2   7.2 ]
               [  7.2  11.2 ]
   `tr = 22.4`, `det = 125.44 − 51.84 = 73.6`. For the `[[a,b],[b,a]]` form the eigenvalues
   are just `a ± b`: **λ = 18.4 and λ = 4.0**.
   Eigenvectors: **(1,1)** for 18.4, **(1,−1)** for 4.0.
3. **PC1 = (1/√2)(1,1) ≈ (0.707, 0.707)**.
   Variance explained: `18.4/22.4 ≈ **82.1%**` and `4.0/22.4 ≈ **17.9%**`.
4. Score = centered point · (0.707, 0.707) = (x+y)/√2:
   | Point | Centered | (x+y)/√2 | Score |
   |---|---|---|---|
   | (1,1) | (−1.6,−1.6) | −3.2/√2 | **−2.263** |
   | (3,3) | (0.4,0.4) | 0.8/√2 | **0.566** |
   | (5,5) | (2.4,2.4) | 4.8/√2 | **3.394** |
   | (3,1) | (0.4,−1.6) | −1.2/√2 | **−0.849** |
   | (1,3) | (−1.6,0.4) | −1.2/√2 | **−0.849** |
5. The scores collapse each point to a single number measuring position along the direction
   of greatest spread, making the dominant pattern — the three points on the diagonal
   spreading far apart while the two off-diagonal points sit close together — immediately
   readable. Note (3,1) and (1,3) get the **identical** score: PC1 genuinely cannot tell them
   apart, because they differ only along PC2, the direction carrying the other 17.9%.
