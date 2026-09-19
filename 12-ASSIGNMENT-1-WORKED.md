# Assignment 1 — Every Question Worked Step by Step

## Read this first

Assignment 1 is due **Tuesday 29 September**, *after* your exam, and must be
**handwritten, in your own hand, handed in on paper**. This file is a **study reference**
for the exam, not something to copy.

Your instructor's own instruction 5 says: *"If an AI tool assists you in understanding a
concept, disclose how it was used; the reasoning and final submission must remain your own
work."* If you use this file, **say so on your submission.** He has explicitly permitted
that, so there's no problem — just don't skip the disclosure.

The reason this file exists: the assignment covers **exactly** the exam's assessed scope,
one question per topic. Working through it *is* exam revision. Do each question yourself
first, then check here.

**Marks: 45 total.** Every answer below shows the steps, because the assignment says
*"Unsupported final answers alone will not receive full credit."*

---

## Q1 — Norms and the meaning of distance [4]

`x = (3, −4, 1)`

### Part 1 — the three norms

**L1** — add absolute values:
`‖x‖₁ = |3| + |−4| + |1| = 3 + 4 + 1 = **8**`

**L2** — square, sum, square-root:
`‖x‖₂ = √(3² + (−4)² + 1²) = √(9 + 16 + 1) = **√26 ≈ 5.10**`

**L∞** — the largest absolute entry:
`‖x‖∞ = max(|3|, |−4|, |1|) = max(3, 4, 1) = **4**`

> **Check:** `8 ≥ 5.10 ≥ 4` ✓ — L1 ≥ L2 ≥ L∞ always holds. Free verification.

### Part 2 — comparing two candidates

`q = (0,0)`, `p = (3,0)`, `r = (2,2)`. Distance from `q` is just the norm of the point
itself, since `p − q = p`.

**Under L1:**
`‖p‖₁ = 3 + 0 = 3`
`‖r‖₁ = 2 + 2 = 4`
`3 < 4` → **p is closer under L1.**

**Under L2:**
`‖p‖₂ = √(9 + 0) = 3`
`‖r‖₂ = √(4 + 4) = √8 ≈ 2.83`
`2.83 < 3` → **r is closer under L2.**

**The two norms disagree.** That's the entire point of the question.

### Part 3 — why the norm changes the meaning of "nearest"

> L2 **squares** each coordinate difference before summing, so one large deviation is
> penalised far more heavily than several small deviations adding to the same total. `p`
> concentrates all of its distance in a single coordinate (3), while `r` spreads it across
> two (2 and 2); squaring makes the concentrated one more expensive. "Nearest" is therefore
> not a property of the data alone — it depends on which ruler you chose.

---

## Q2 — Matrix as a function, column picture, and rank [5]

`A = [[1, 0, 1], [0, 1, 1]]` (2×3), `z = (2, −1, 3)`

### Part 1 — ordinary row-by-column

Row 1 · z: `(1)(2) + (0)(−1) + (1)(3) = 2 + 0 + 3 = 5`
Row 2 · z: `(0)(2) + (1)(−1) + (1)(3) = 0 − 1 + 3 = 2`

**`Az = (5, 2)`**

### Part 2 — as a linear combination of columns

A's columns are `c₁ = (1,0)`, `c₂ = (0,1)`, `c₃ = (1,1)`. The entries of `z` are the weights:

    Az = 2·c₁ + (−1)·c₂ + 3·c₃
       = 2(1,0) + (−1)(0,1) + 3(1,1)
       = (2,0) + (0,−1) + (3,3)
       = (5, 2)   ✓ same answer

> This is the column picture: **`Az` is a weighted sum of A's columns, weighted by z's entries.**

### Part 3 — rank and a dependence relation

`c₁ = (1,0)` and `c₂ = (0,1)` are clearly independent — neither is a multiple of the other.
Can `c₃` be built from them? Yes:

> **`c₃ = c₁ + c₂`**, since `(1,0) + (0,1) = (1,1)` ✓

So only 2 of the 3 columns carry independent information.
**rank(A) = 2.**

### Part 4 — rank-deficient? full column rank?

**Rank-deficient?** A is 2×3, so the maximum possible rank is `min(m, n) = min(2,3) = 2`.
Since rank(A) = 2 = the maximum, **A is NOT rank-deficient — it is full rank.**

**Full column rank?** Full column rank means rank = **number of columns** = 3. But
rank(A) = 2 ≠ 3, so **A does NOT have full column rank.**

> ⚠ **This is the trap.** "Full rank" and "full column rank" are different claims, and here
> the answers are opposite. A wide matrix (more columns than rows) can *never* have full
> column rank — you cannot have 3 independent vectors in ℝ².

### Part 5 — is `AᵀA` invertible, without a determinant?

`AᵀA` is `(3×2)(2×3) → **3×3**`.

Use the key result: **`AᵀA` is invertible if and only if A has full column rank.**
From Part 4, A does **not** have full column rank. Therefore:

> **`AᵀA` is singular — not invertible.**

Supporting reason: `rank(AᵀA) = rank(A) = 2`, but a 3×3 matrix needs rank 3 to be
invertible. A rank-2 matrix sitting in a 3×3 shape collapses a dimension, so no inverse exists.

---

## Q3 — Dot product and cosine similarity [4]

`u = (1, 3)`, `w = (2, −1)`

### Part 1 — the dot product

`u · w = (1)(2) + (3)(−1) = 2 − 3 = **−1**`

### Part 2 — cosine similarity

`‖u‖ = √(1² + 3²) = √10`
`‖w‖ = √(2² + (−1)²) = √5`

    cos θ = (u·w) / (‖u‖‖w‖) = −1 / (√10 · √5) = −1/√50

`√50 = 5√2 ≈ 7.071`, so **cos θ = −1/(5√2) ≈ −0.141**
(And `θ = arccos(−0.141) ≈ **98.1°**.)

### Part 3 — what the sign tells you

> The dot product is negative, so `cos θ < 0`, which means the angle is **greater than 90°**
> (obtuse). That is the complete content of the sign — nothing more specific.

### Part 4 — are they exactly opposite? ⚠

> **No.** Exactly opposite means θ = 180°, which requires `cos θ = −1`. Here
> `cos θ ≈ −0.141`, giving θ ≈ 98.1° — only just past perpendicular.
>
> A negative dot product covers the **entire range 90° < θ ≤ 180°**. "Negative" narrows the
> angle down to "somewhere obtuse"; 180° is one single extreme case inside that whole range.
> 90° is a boundary, not a landmark to compare against.

### Part 5 — replacing `u` with `10u`

**Raw dot product:** `(10u)·w = 10(u·w) = **−10**` — it scales by 10.

**Cosine similarity:** **unchanged at ≈ −0.141.** Here's why:

    cos θ = (10u·w) / (‖10u‖‖w‖) = 10(u·w) / (10‖u‖·‖w‖)

The factor of 10 appears in the numerator and again in `‖10u‖ = 10‖u‖`, so it **cancels**.

> This is exactly why production recommender and embedding-search systems use cosine
> similarity rather than the raw dot product: the raw dot product grows with magnitude, so a
> user who rates everything 5/5 scores highly against almost anyone. Cosine divides
> magnitude out and compares **direction only**.

---

## Q4 — Projection and residual [5]

`a = (3, 1)`, `b = (2, 4)`

### Part 1 — derive the scalar coefficient

The closest point on the line through `a` must be `c·a` for some scalar `c`. The defining
condition is that the **residual is perpendicular to `a`** — otherwise you could slide along
the line and get closer:

    (b − c·a) · a = 0
     b·a − c(a·a) = 0            [expanding, since the dot product distributes]
                c = (a·b)/(a·a)

Now the numbers:
`a·b = (3)(2) + (1)(4) = 6 + 4 = 10`
`a·a = (3)(3) + (1)(1) = 9 + 1 = 10`

**`c = 10/10 = 1`**

### Part 2 — the projection

`projₐ(b) = c·a = 1·(3,1) = **(3, 1)**`

### Part 3 — the residual

`r = b − projₐ(b) = (2,4) − (3,1) = **(−1, 3)**`

### Part 4 — verify orthogonality

`r · a = (−1)(3) + (3)(1) = −3 + 3 = **0** ✓`

> **Always do this step.** It verifies the whole calculation in five seconds, and it is
> item 3 on the instructor's own self-check list.

### Part 5 — why this is the closest point, geometrically

> Take any other point `p` on the line. Then `b`, the projection, and `p` form a **right
> triangle**: the right angle is at the projection (because the residual is perpendicular to
> the line), the segment from the projection to `p` is one leg, and the segment from `b` to
> `p` is the **hypotenuse**. The hypotenuse is always the longest side of a right triangle,
> so the distance from `b` to `p` is strictly greater than the distance from `b` to the
> projection. Since `p` was arbitrary, the projection is the closest point.
>
> Equivalently: from any non-perpendicular point you could slide toward the foot of the
> perpendicular and shorten the distance — so such a point could not have been closest.

---

## Q5 — Determinant, trace, eigenvalues, and eigenvectors [6]

`B = [[4, 1], [2, 3]]`

### Part 1 — determinant and trace

`det(B) = ad − bc = (4)(3) − (1)(2) = 12 − 2 = **10**`
`tr(B) = a + d = 4 + 3 = **7**`

### Part 2 — characteristic equation and eigenvalues

    B − λI = [ 4−λ    1  ]
             [  2    3−λ ]

    det(B − λI) = (4−λ)(3−λ) − (1)(2)
                = 12 − 4λ − 3λ + λ² − 2
                = λ² − 7λ + 10 = 0

> Shortcut check: for any 2×2, the characteristic equation is `λ² − tr·λ + det = 0`.
> Here `λ² − 7λ + 10` ✓ — matches, so the expansion was done correctly.

Factor: `λ² − 7λ + 10 = (λ − 5)(λ − 2) = 0`

**λ₁ = 5, λ₂ = 2**

### Part 3 — an eigenvector for each

**For λ = 5:** solve `(B − 5I)v = 0`.

    B − 5I = [ −1   1 ]
             [  2  −2 ]

Row 1: `−v₁ + v₂ = 0` → `v₂ = v₁`.
(Row 2: `2v₁ − 2v₂ = 0` → the same equation. Rows *always* turn out redundant here — that's
a sign you found the eigenvalue correctly.)

Pick `v₁ = 1`: **`v = (1, 1)`**

**For λ = 2:** solve `(B − 2I)v = 0`.

    B − 2I = [ 2  1 ]
             [ 2  1 ]

Row 1: `2v₁ + v₂ = 0` → `v₂ = −2v₁`.
Pick `v₁ = 1`: **`v = (1, −2)`**

> Eigenvectors are only defined **up to scale** — `(2,2)` or `(−1,−1)` are equally correct
> for λ=5. Pick whatever gives small whole numbers.

### Part 4 — verify one eigenpair

Check λ = 5 with `v = (1,1)`:

    Bv = [4 1][1]  =  (4)(1) + (1)(1)  =  (5, 5)
         [2 3][1]     (2)(1) + (3)(1)

And `λv = 5·(1,1) = (5,5)` ✓ — **`Bv = λv` confirmed.**

*(Optional second check, λ=2: `B(1,−2) = (4−2, 2−6) = (2,−4) = 2·(1,−2)` ✓)*

### Part 5 — why B is invertible

Any **one** of these, stated with its reason:

> - `det(B) = 10 ≠ 0`. A non-zero determinant means the map does not collapse area, so no
>   dimension is destroyed and the transformation can be undone.
> - Neither eigenvalue is zero (5 and 2). A zero eigenvalue would mean some direction is
>   crushed to nothing; none is.
> - B is **full rank** (rank 2) — its columns are linearly independent.

### Part 6 — confirm against trace and determinant

`λ₁ + λ₂ = 5 + 2 = 7` and `tr(B) = 7` ✓
`λ₁ × λ₂ = 5 × 2 = 10` and `det(B) = 10` ✓

> **Both identities hold.** Conceptually: the eigenvalues carry the same information as the
> determinant and trace, just split apart per-direction instead of aggregated. Practically:
> this is a free check — use it on every eigenvalue question you ever do.

---

## Q6 — Diagonalization and its limitations [4]

### Part 1 — construct P and D

Take the eigenvectors from Q5 as the **columns** of P, and the matching eigenvalues on D's
diagonal **in the same order**:

    P = [ 1   1 ]        D = [ 5   0 ]
        [ 1  −2 ]            [ 0   2 ]

> Column 1 of P is the eigenvector for λ = 5, which sits at `D₁₁`. If you swap P's columns
> you must swap D's diagonal too — both orderings are correct, mismatched is wrong.

**Optional verification** (not required, but it proves the construction):
`det(P) = (1)(−2) − (1)(1) = −3`, and using `[[a,b],[c,d]]⁻¹ = (1/det)[[d,−b],[−c,a]]`:

    P⁻¹ = (1/−3) [ −2  −1 ]  =  [ 2/3   1/3 ]
                 [ −1   1 ]     [ 1/3  −1/3 ]

Multiplying out gives `PDP⁻¹ = [[4,1],[2,3]] = B` ✓

### Part 2 — why `B¹⁰` becomes easier

Because the decomposition telescopes:

    B² = (PDP⁻¹)(PDP⁻¹) = PD(P⁻¹P)DP⁻¹ = PD·I·DP⁻¹ = PD²P⁻¹

and repeating gives **`Bⁿ = PDⁿP⁻¹`**.

> `Dⁿ` is trivial — a diagonal matrix raised to a power is just each diagonal entry raised
> to that power, so `D¹⁰ = diag(5¹⁰, 2¹⁰)`. Computing `B¹⁰` directly means **ten full matrix
> multiplications**; this way it's two scalar exponentiations plus two matrix products.
>
> It also reveals the **long-run behaviour** immediately: as n grows the largest `|λ|`
> dominates, so `B¹⁰` is essentially a scaled projection onto the `(1,1)` direction.

### Part 3 — can `P⁻¹` be replaced by `Pᵀ`? ⚠

**The condition:** `P⁻¹ = Pᵀ` holds only when **P is orthogonal** — its columns must be
mutually orthogonal **and** each of unit length (orthonormal). This is guaranteed when the
original matrix is **symmetric**.

**Test it on this matrix:**
- Is `B` symmetric? `B₁₂ = 1` but `B₂₁ = 2`, so `B ≠ Bᵀ`. **Not symmetric.**
- Are the eigenvectors orthogonal? `(1,1) · (1,−2) = 1 − 2 = **−1 ≠ 0`. **Not orthogonal.**

> **No — the replacement is not valid here.** B is not symmetric, its eigenvectors are not
> orthogonal, and P is therefore not an orthogonal matrix. The genuine inverse `P⁻¹` must be
> used.
>
> *(Contrast: for a symmetric matrix like `[[3,1],[1,3]]`, the eigenvectors `(1,1)` and
> `(1,−1)` **are** orthogonal, and after normalising each to length 1, `P⁻¹ = Pᵀ` does hold.
> The answer depends on the matrix — always test, never memorise a verdict.)*

### Part 4 — one reason SVD is more general

> **SVD exists for every matrix, with no exceptions.** Eigendecomposition requires a
> **square** matrix possessing a **full set of independent eigenvectors**, and both
> requirements fail routinely: a 768×1024 image matrix is not square and has no eigenvectors
> at all, and a shear matrix like `[[1,1],[0,1]]` is square but has a repeated eigenvalue
> with only one independent eigenvector, so no eigenbasis can be built.
>
> SVD sidesteps this by using the eigenvectors of `AᵀA` and `AAᵀ`, which are **symmetric for
> any A** and therefore always fully diagonalizable by the spectral theorem.

---

## Q7 — Reading an SVD correctly [5]

`C ∈ ℝ⁶ˣ¹⁰` — 6 documents, 10 terms. Rank-2 approximation `C₂ = U₂Σ₂V₂ᵀ`.

### Part 1 — dimensions

Truncation keeps the first `k = 2` columns of U, the top 2 singular values, and the first 2
rows of `Vᵀ`:

> **`U₂` is 6 × 2**  ·  **`Σ₂` is 2 × 2**  ·  **`V₂ᵀ` is 2 × 10**

**Check:** `(6×2)(2×2)(2×10) → 6×10` ✓ — the same shape as C, as it must be.

### Part 2 — which space is which

> - **U (left singular vectors) → document space.** C has **6 rows = 6 documents**, and U's
>   columns are length-6 vectors, so they are directions in document space. `U₂` groups
>   **documents** by topic.
> - **V (right singular vectors) → term space.** C has **10 columns = 10 terms**, and V's
>   columns are length-10 vectors, so they are directions in term space. `V₂` groups
>   **words** by topic.

Memory hook: **V is the input side** (n = columns), **U is the output side** (m = rows).

### Part 3 — does SVD exist for a non-square C?

> **Yes.** Working for non-square matrices is precisely why SVD exists — it's the gap
> eigendecomposition cannot fill.
>
> **The reason:** V's columns are the eigenvectors of `CᵀC` and U's are the eigenvectors of
> `CCᵀ`. Both of those are **symmetric for any matrix C** (since `(CᵀC)ᵀ = Cᵀ(Cᵀ)ᵀ = CᵀC`),
> and the **spectral theorem** guarantees every symmetric matrix has a full set of real,
> orthogonal eigenvectors. Diagonalization can fail on C itself; it never fails on `CᵀC` or
> `CCᵀ`. That is the whole guarantee, and it doesn't care about C's shape.

### Part 4 — a singular vector with every sign reversed ⚠

> **Not an error.** Singular vectors are only unique **up to sign**. Flipping the signs of
> `uᵢ` and `vᵢ` **together** leaves the product `σᵢuᵢvᵢᵀ` unchanged — the two minus signs
> cancel — so the reconstruction `C₂` is bit-for-bit identical. Different libraries (or the
> same library on different runs) may legitimately return either version.
>
> **Practical consequence:** never read meaning into *which side of zero* a point falls on.
> Only the **grouping** of points is meaningful.

### Part 5 — why rank-2 reveals two latent topics without labels

> Documents about the same subject reuse the same vocabulary, so their rows in C are close
> to linear combinations of a small number of underlying word-usage patterns — the matrix
> has **low effective rank**. SVD finds the directions capturing the most variation in the
> data, and for a document-term matrix those directions **are** the shared vocabulary
> patterns, i.e. the topics.
>
> No labels were needed because the structure was already present in the **co-occurrence
> counts**; SVD simply exposed the redundancy that was there all along. Keeping k=2 keeps the
> two strongest such patterns.
>
> *(Caveat worth adding: raw counts work for a toy example, but real systems preprocess with
> TF-IDF, stopword removal and normalization first, or the recovered topics are mediocre.)*

---

## Q8 — Truncated SVD, energy, and approximation error [5]

`σ₁ = 4`, `σ₂ = 3`, `σ₃ = 1`

### Part 1 — total energy

> **Energy = the sum of the SQUARED singular values.** (Forgetting the square is item 6 on
> the instructor's self-check list.)

    Σσᵢ² = 4² + 3² + 1² = 16 + 9 + 1 = **26**

### Part 2 — energy retained

**Rank-1** — keep σ₁ only:
`16 / 26 = 0.6154 → **61.54%**`

**Rank-2** — keep σ₁ and σ₂:
`(16 + 9) / 26 = 25 / 26 = 0.9615 → **96.15%**`

### Part 3 — Frobenius reconstruction errors

> The error is built from the singular values you **dropped**, and it **has a square root**:
> `‖A − Aₖ‖_F = √(σ²ₖ₊₁ + σ²ₖ₊₂ + ⋯)`

**Rank-1** (dropped σ₂=3 and σ₃=1):
`‖A − A₁‖_F = √(3² + 1²) = √(9+1) = **√10 ≈ 3.162**`

**Rank-2** (dropped σ₃=1 only):
`‖A − A₂‖_F = √(1²) = **1.00**`

> ⚠ Don't confuse the two formulas: **energy retained** is a ratio with no square root using
> the **kept** values; **Frobenius error** is a length with a square root using the
> **dropped** values.
>
> *Cross-check:* retained fraction + error²/total = `25/26 + 1/26 = 1` ✓

### Part 4 — smallest k for ≥95% energy

k=1 → 61.54% ✗ (below 95%)
k=2 → 96.15% ✓ (above 95%)

> **k = 2.**

Note how quickly it saturates — two of three components already carry over 96% of the
signal. That rapid saturation is the entire reason low-rank approximation is useful.

### Part 5 — what Eckart–Young guarantees, precisely

> Among **all** matrices of rank k — not merely those obtained by truncating an SVD — the
> truncated SVD `Aₖ` **minimises** the approximation error to A, in **both** the Frobenius
> norm and the spectral norm. Furthermore, the remaining error is **exactly determined by
> the singular values that were discarded**.

All three clauses matter: *(i)* best among **all** rank-k matrices, *(ii)* in **both** norms,
*(iii)* with a **known, exact** error.

---

## Q9 — Principal Component Analysis via SVD [5]

    X = [ 1  1 ]
        [ 2  3 ]
        [ 3  2 ]
        [ 4  4 ]

### Part 1 — mean and centered data

Column 1 mean: `(1 + 2 + 3 + 4)/4 = 10/4 = 2.5`
Column 2 mean: `(1 + 3 + 2 + 4)/4 = 10/4 = 2.5`
**Mean vector = (2.5, 2.5)**

Subtract the mean from each row:

    Xc = [ 1−2.5   1−2.5 ]     [ −1.5  −1.5 ]
         [ 2−2.5   3−2.5 ]  =  [ −0.5   0.5 ]
         [ 3−2.5   2−2.5 ]     [  0.5  −0.5 ]
         [ 4−2.5   4−2.5 ]     [  1.5   1.5 ]

> **Check:** each column must now sum to zero.
> Col 1: `−1.5 − 0.5 + 0.5 + 1.5 = 0` ✓  Col 2: `−1.5 + 0.5 − 0.5 + 1.5 = 0` ✓

### Part 2 — `XcᵀXc`, its eigenvalues and eigenvectors

`XcᵀXc` is 2×2 with entries `[[Σx², Σxy], [Σxy, Σy²]]` over the centered rows:

`Σx² = (−1.5)² + (−0.5)² + (0.5)² + (1.5)² = 2.25 + 0.25 + 0.25 + 2.25 = 5`
`Σy² = (−1.5)² + (0.5)² + (−0.5)² + (1.5)² = 2.25 + 0.25 + 0.25 + 2.25 = 5`
`Σxy = (−1.5)(−1.5) + (−0.5)(0.5) + (0.5)(−0.5) + (1.5)(1.5)`
`    = 2.25 − 0.25 − 0.25 + 2.25 = 4`

    XcᵀXc = [ 5  4 ]
            [ 4  5 ]

**Eigenvalues:** `tr = 10`, `det = 25 − 16 = 9`, so `λ² − 10λ + 9 = 0`.
Factor: `(λ − 9)(λ − 1) = 0` → **λ₁ = 9, λ₂ = 1**

**Eigenvectors:**

λ=9: `XcᵀXc − 9I = [[−4, 4], [4, −4]]`. Row 1: `−4v₁ + 4v₂ = 0` → `v₂ = v₁` → **(1, 1)**
λ=1: `XcᵀXc − 1I = [[4, 4], [4, 4]]`. Row 1: `4v₁ + 4v₂ = 0` → `v₂ = −v₁` → **(1, −1)**

> Note they are orthogonal — `(1,1)·(1,−1) = 0` ✓ — as they must be, since `XcᵀXc` is
> symmetric.

**Why this shortcut works:** `Xc = UΣVᵀ`, so `XcᵀXc = VΣᵀUᵀUΣVᵀ = VΣ²Vᵀ` (because `UᵀU = I`).
That is an eigendecomposition with **V as the eigenvectors and Σ² as the eigenvalues** — so
eigendecomposing the small 2×2 matrix gives you exactly what a full SVD would, without doing one.

### Part 3 — the first principal component

The eigenvector with the **larger** eigenvalue (λ = 9):

> **PC1 = (1, 1)**, or as a unit vector **`(1/√2)(1,1) ≈ (0.707, 0.707)`**

**Does it match the data?** Plot the four points: `(1,1), (2,3), (3,2), (4,4)`.

> **Yes.** The points run from bottom-left `(1,1)` up to top-right `(4,4)` along the 45°
> diagonal, with the two middle points sitting only slightly off it. The direction of
> greatest spread is clearly `(1,1)`, exactly as computed.

*(Don't skip this sentence — item 8 on the self-check list is "interpret the calculations
rather than only reporting them".)*

### Part 4 — proportion of variance explained

Total = `9 + 1 = 10`.

> **PC1: `9/10 = 90%`**  ·  **PC2: `1/10 = 10%`**

So a single number per point — its position along the diagonal — retains 90% of the
structure of this two-column dataset.

### Part 5 — the covariance route

`Σcov = (1/n) XcᵀXc = (1/4) [[5,4],[4,5]] = [[1.25, 1.00], [1.00, 1.25]]`

> **Multiplying a matrix by a positive scalar does not change its eigenvectors** — it only
> multiplies each eigenvalue by that scalar. Concretely, if `Mv = λv` then
> `(cM)v = c(Mv) = (cλ)v`, so `v` is still an eigenvector with eigenvalue `cλ`.
>
> **Therefore:**
> - The **principal component directions are identical**: still `(1,1)` and `(1,−1)`.
> - The **eigenvalues are divided by n = 4**: `9/4 = 2.25` and `1/4 = 0.25`.
> - The **proportions are unchanged**: `2.25 / 2.5 = 90%` — the constant cancels in the ratio.
>
> That is why both routes give the same PCA. *(Note: many texts, including this course's own
> slides, use `1/(n−1)` instead of `1/n`; the same argument applies verbatim and the
> proportions are still 90%/10%.)*

### Part 6 — connection to "energy" from Q8

> The **eigenvalues of `XcᵀXc` play exactly the role the squared singular values `σᵢ²` played
> in Q8** — and in fact they *are* the squared singular values, since `XcᵀXc = VΣ²Vᵀ`.
>
> "Proportion of variance explained" in PCA and "proportion of energy retained" in truncated
> SVD are therefore **the same quantity computed the same way**, just described in two
> different vocabularies. This is unsurprising once you see that **PCA is simply SVD applied
> to centered data**.

---

## Q10 — Tukey and the purpose of formal tools [2]

**You must write this yourself** — it's a reflective question and generic phrasing reads as
generic. See `09-tukey-and-concept-questions.md` §3 for the full framing. The structure:

1. **The course's position:** formal tools (linear algebra, calculus, probability) are what
   make a system behave predictably and reliably **at scale**, rather than working once by
   accident.
2. **Tukey's position:** data analysis is a **broader empirical discipline** than mathematical
   statistics — practical judgement about messy real problems matters as much as technique.
3. **Reconcile them as complementary, with a concrete example from this module.** Strongest
   options:
   - Eckart–Young guarantees the rank-k approximation is optimal *for a given k*, but
     **nothing in the mathematics tells you what k should be** — that's an energy target, an
     elbow, or a domain constraint. Judgement.
   - L1 and L2 give **different but equally correct** answers to "who is nearest" (Q1!).
     Choosing the ruler is judgement, not a theorem.
   - The algebra of PCA runs perfectly on unstandardised data and returns a meaningless PC1.
     Only judgement about units catches that.

> **The sentence that ties it together:** formal technique without judgement produces
> rigorous answers to the wrong question; judgement without formal technique doesn't scale
> or generalise. This course teaches the first so that you can exercise the second responsibly.

---

## Final self-check (the instructor's own list)

Before submitting, confirm you have:

- ☐ Distinguished **rank from matrix shape** (Q2 — full rank but *not* full column rank)
- ☐ Distinguished a **negative dot product from exactly opposite** vectors (Q3)
- ☐ Checked that the **projection residual is orthogonal** to the target direction (Q4)
- ☐ Kept **`P⁻¹` distinct from `Pᵀ`** unless orthogonality is established (Q6 — here it is *not*)
- ☐ Identified the **roles and dimensions of U, Σ, Vᵀ** (Q7)
- ☐ Defined **SVD energy as the sum of squared singular values** (Q8)
- ☐ **Mean-centered** before computing a covariance matrix (Q9)
- ☐ **Interpreted** the calculations rather than only reporting them (all of them)
- ☐ Written your name and section on page 1, questions in order, by hand
- ☐ **Disclosed any AI assistance**, per instruction 5
