# Topic 5 — Diagonalization (and where it breaks)

## 1. The idea

You have eigenvectors — directions where the matrix is *just a scaling*. What if you
rebuilt the entire matrix out of them?

> **A = P D P⁻¹**
>
> - **P** = eigenvectors as **columns**
> - **D** = eigenvalues on the **diagonal**, in the **matching order**, zeros elsewhere
> - **P⁻¹** = the inverse of P

**The geometric story (this is what a "explain what this means" question wants):**

> To understand what a messy matrix does: **rotate into the eigenbasis** — the coordinate
> system where the matrix is just a scaling — **do the scaling**, then **rotate back**.

Read right-to-left, matching how `PDP⁻¹v` is actually applied:
1. `P⁻¹` — re-express `v` in eigenvector coordinates
2. `D` — scale each eigen-direction by its own eigenvalue (trivially easy)
3. `P` — convert back to the original coordinates

A complicated transformation is revealed as a simple stretch, viewed from the right angle.

## 2. Building it — worked example

From Topic 4: `B = [[4,1],[2,3]]`, eigenpairs `λ=5 → (1,1)` and `λ=2 → (1,−2)`.

    P = [ 1   1 ]        D = [ 5   0 ]
        [ 1  −2 ]            [ 0   2 ]

**Order must match:** column 1 of P is the eigenvector for the entry `D₁₁ = 5`. If you
swap the columns of P you must swap the diagonal of D too. Both are correct; mismatched
is wrong.

(If you ever need it: for 2×2, `[[a,b],[c,d]]⁻¹ = (1/det)·[[d,−b],[−c,a]]`.
Here `det(P) = −2−1 = −3`, so `P⁻¹ = (−1/3)[[−2,−1],[−1,1]]`.)

## 3. Why anyone bothers: powers

> **Aⁿ = P Dⁿ P⁻¹**

Why that's true: `A² = (PDP⁻¹)(PDP⁻¹) = PD(P⁻¹P)DP⁻¹ = PD²P⁻¹`, and it telescopes.
All the inner `P⁻¹P` pairs cancel to the identity.

**The payoff:** `Dⁿ` is trivial — just raise each diagonal entry to the n-th power.
So `D¹⁰ = diag(5¹⁰, 2¹⁰)`. Computing `B¹⁰` directly means **ten full matrix
multiplications**; via diagonalization it's two matrix multiplications and two scalar powers.

More than speed, it tells you the **long-run behaviour** immediately: as n grows, the term
with the largest `|λ|` dominates everything else. Here `5¹⁰` utterly swamps `2¹⁰`, so
`B¹⁰` is essentially a projection onto the `(1,1)` direction, scaled.

**Where this is used:**
- **Markov chains** — long-run behaviour of a random process
- **Network diffusion** — how influence or disease spreads through a graph over time
- **Graphics & robotics** — applying the same transform many times

## 4. ⚠ THE TRAP: P⁻¹ is not Pᵀ

This is item #4 on the instructor's self-check list, and the slides flag it twice.

> **You may replace `P⁻¹` with `Pᵀ` only when P is orthogonal** — i.e. its columns are
> **orthonormal**: mutually orthogonal *and* each of length 1.

That happens when **A is symmetric** (`A = Aᵀ`). Symmetric matrices have eigenvectors that
are automatically orthogonal; normalise them to length 1 and `P⁻¹ = Pᵀ` exactly.

**Check our example:** `B = [[4,1],[2,3]]` — is `B = Bᵀ`? `B₁₂ = 1`, `B₂₁ = 2`. **Not
symmetric.**
And the eigenvectors: `(1,1)·(1,−2) = 1 − 2 = **−1 ≠ 0`. **Not orthogonal.**

> **So for B, `P⁻¹ ≠ Pᵀ`. You must use the genuine inverse.**

By contrast, `A = [[3,1],[1,3]]` *is* symmetric, its eigenvectors `(1,1)` and `(1,−1)`
*are* orthogonal, and after dividing each by `√2` to get length 1, `P⁻¹ = Pᵀ` holds.

**If an exam question asks "is it valid to replace P⁻¹ by Pᵀ here?", the answer is a
two-part response:** state the condition (P must be orthogonal, which happens when A is
symmetric), then **test it on the actual matrix** and say yes or no. Both halves earn marks.

## 5. Where diagonalization breaks down — and why SVD exists

Diagonalization is powerful but **it is not universal**. Three failure modes:

**(a) The matrix isn't square.**
`A = PDP⁻¹` requires eigenvectors, which require `Av = λv`, which requires input and
output to live in the same space. The Day 1 raccoon image is **768 × 1024**. Not square.
Diagonalization *doesn't even apply* to the exact demo the course opened with.

**(b) Square but not diagonalizable.**
A shear matrix like `[[1,1],[0,1]]` has a repeated eigenvalue (λ=1 twice) but only **one**
independent eigenvector direction. You can't build a full eigenbasis out of one vector, so
P isn't invertible and the decomposition fails.

**(c) Complex eigenvalues.** A pure rotation in 2D has no real eigenvector at all — it
rotates *every* direction. Its eigenvalues are complex.

> **We need a decomposition that works for any matrix — always. Square or not.
> Diagonalizable or not. No exceptions.**

That is **SVD**, and that gap is the entire reason it exists. Topic 6.

## 6. Quick comparison table (worth copying onto your cheat sheet)

| | Eigendecomposition `A = PDP⁻¹` | SVD `A = UΣVᵀ` |
|---|---|---|
| Works for | square, diagonalizable matrices only | **every** matrix, no exceptions |
| Basis in/out | **same** basis (P both sides) | **two different** bases (V in, U out) |
| Core relation | `Av = λv` (direction preserved) | `Avᵢ = σᵢuᵢ` (direction may rotate) |
| Scalars | eigenvalues λ, can be negative or complex | singular values σ, always **≥ 0**, sorted descending |
| Bases orthogonal? | only if A is symmetric | **always** — U and V are always orthogonal |

## 6b. WORKED EXAMPLES — study these before the drills

---

### Worked Example 1 — build `P` and `D`, and test whether `Pᵀ` may replace `P⁻¹`

> **Problem:** `A = [[3, 2], [1, 4]]`, with eigenpairs `λ=5 → (1,1)` and `λ=2 → (−2,1)`
> (from Topic 4, Worked Example 1). Construct `P` and `D`, explain why `A¹⁰` becomes easy,
> and decide whether `P⁻¹` can be replaced by `Pᵀ`.

**Step 1 — build P from the eigenvectors, as COLUMNS.**

The eigenvectors are `(1,1)` and `(−2,1)`. Stand each one **upright** and place it as a
column:

```
        ┌            ┐
P   =   │  1     −2  │       ← first row: first entries of each eigenvector
        │  1      1  │       ← second row: second entries
        └            ┘
          ↑      ↑
        for    for
        λ=5    λ=2
```

> ⚠ **Columns, not rows.** Writing `[[1,1],[−2,1]]` is the most common mistake here.

**Step 2 — build D from the eigenvalues, on the diagonal, in the MATCHING order.**

Column 1 of P is the eigenvector for λ=5, so 5 goes in position `D₁₁`:

```
        ┌         ┐
D   =   │  5   0  │
        │  0   2  │
        └         ┘
```

> **The orders must correspond.** Swapping P's columns is fine *only* if you also swap D's
> diagonal. Mismatched is simply wrong.

**Step 3 — state the decomposition.**

```
A = P D P⁻¹
```

**Step 4 — (optional) verify by computing `P⁻¹`.** For a 2×2, the inverse is: *swap the
diagonal entries, negate the off-diagonal entries, divide by the determinant.*

```
det(P) = (1)(1) − (−2)(1) = 1 + 2 = 3

         1    ┌          ┐        ┌              ┐
P⁻¹  =  ───   │  1    2  │   =    │  1/3    2/3  │
         3    │ −1    1  │        │ −1/3    1/3  │
              └          ┘        └              ┘
```

Quick sanity check that `PP⁻¹ = I`:

```
row1 of P · col1 of P⁻¹ = (1)(1/3) + (−2)(−1/3) = 1/3 + 2/3 = 1  ✓
row1 of P · col2 of P⁻¹ = (1)(2/3) + (−2)( 1/3) = 2/3 − 2/3 = 0  ✓
```

**Step 5 — why `A¹⁰` becomes easy.**

```
A² = (PDP⁻¹)(PDP⁻¹) = PD(P⁻¹P)DP⁻¹ = PD·I·DP⁻¹ = PD²P⁻¹
```

The inner `P⁻¹P` collapses to the identity, and this telescopes:

```
Aⁿ = P Dⁿ P⁻¹
```

> `Dⁿ` is **trivial** — a diagonal matrix to a power is just each diagonal entry to that
> power: `D¹⁰ = diag(5¹⁰, 2¹⁰)`. Computing `A¹⁰` directly needs **ten full matrix
> multiplications**; this way it's two scalar powers plus two matrix products.
>
> It also reveals the long-run behaviour instantly: `5¹⁰` dwarfs `2¹⁰`, so `A¹⁰` is
> essentially a scaled projection onto the `(1,1)` direction.

**Step 6 — can `P⁻¹` be replaced by `Pᵀ`? Test it, never assume.**

The condition: **`P⁻¹ = Pᵀ` only when P is orthogonal** — its columns must be mutually
orthogonal *and* each of length 1. That is guaranteed when **A is symmetric**.

*Test (a): is A symmetric?*

```
A₁₂ = 2        A₂₁ = 1        2 ≠ 1   →   A is NOT symmetric
```

*Test (b): are the eigenvectors orthogonal?* Dot them:

```
(1,1) · (−2,1) = (1)(−2) + (1)(1) = −2 + 1 = −1  ≠  0   →   NOT orthogonal
```

> **Answer: No — the replacement is not valid here.** A is not symmetric and its
> eigenvectors are not orthogonal, so P is not an orthogonal matrix. You must use the
> genuine inverse `P⁻¹` computed in Step 4.

---

### Worked Example 2 — the symmetric case, where `Pᵀ` IS allowed

> **Problem:** `A = [[5, 2], [2, 5]]`. Build P and D. Is `P⁻¹ = Pᵀ` valid?

**Step 1 — eigenvalues.** `tr = 10`, `det = 25 − 4 = 21`.

```
λ² − 10λ + 21 = 0   →   (λ − 7)(λ − 3) = 0   →   λ = 7, 3
```

**Step 2 — eigenvectors.**

```
λ=7:  A − 7I = [ −2   2 ]   →  −2v₁ + 2v₂ = 0  →  v₂ = v₁   →  (1, 1)
                [  2  −2 ]

λ=3:  A − 3I = [  2   2 ]   →   2v₁ + 2v₂ = 0  →  v₂ = −v₁  →  (1, −1)
                [  2   2 ]
```

**Step 3 — test orthogonality.**

```
(1,1) · (1,−1) = 1 − 1 = 0   ✓  ORTHOGONAL
```

> This is **not luck.** `A` is symmetric (`A₁₂ = A₂₁ = 2`), and symmetric matrices always
> have orthogonal eigenvectors. That's the spectral theorem, and it's also why SVD always
> exists (Topic 6).

**Step 4 — orthogonal is not yet enough: NORMALISE.**

`Pᵀ = P⁻¹` requires columns of **length 1**, not merely perpendicular. Check:

```
‖(1,1)‖ = √(1+1) = √2  ≠  1
```

So divide each column by its length `√2`:

```
             1    ┌          ┐
P    =     ────   │  1    1  │              D  =  [ 7   0 ]
            √2    │  1   −1  │                    [ 0   3 ]
                  └          ┘
```

**Step 5 — conclude.**

> **Yes, `P⁻¹ = Pᵀ` is valid here** — but only after normalising. A is symmetric, so its
> eigenvectors are orthogonal; dividing each by `√2` makes them orthonormal, which makes P
> an orthogonal matrix, for which the inverse and the transpose coincide.

> **Compare with Worked Example 1, where the answer was NO.** Same question, opposite
> answers. **Always test the matrix in front of you** — never memorise a verdict.

---

### Worked Example 3 — when diagonalization fails entirely

> **Problem:** Why can't `S = [[1, 1], [0, 1]]` be diagonalized?

**Step 1 — eigenvalues.** `tr = 2`, `det = (1)(1) − (1)(0) = 1`.

```
λ² − 2λ + 1 = 0   →   (λ − 1)² = 0   →   λ = 1 (repeated twice)
```

**Step 2 — find the eigenvectors.**

```
S − 1I = [ 0   1 ]     Row 1:  0·v₁ + 1·v₂ = 0   →   v₂ = 0
         [ 0   0 ]     Row 2:  0 = 0  (no information)
```

`v₂ = 0`, and `v₁` is free. So every eigenvector looks like `(v₁, 0)` — they are **all
multiples of `(1, 0)`**. There is only **one independent eigenvector direction.**

**Step 3 — why that breaks diagonalization.**

> To build `P` you need **2 independent columns** for a 2×2 matrix. You only have 1, so `P`
> would be singular and `P⁻¹` wouldn't exist. **`S` is not diagonalizable** — even though it
> is square and has real eigenvalues.

**Step 4 — the point.**

> This is one of three ways eigendecomposition fails: **non-square**, **too few independent
> eigenvectors** (this case), or **complex eigenvalues** (e.g. a rotation).
>
> **SVD doesn't care** — it exists for `S`, and for every other matrix. That gap is exactly
> why the next topic exists.

---

### What to notice across all three

- **P = eigenvectors as columns. D = eigenvalues on the diagonal. Orders must match.**
- `Aⁿ = PDⁿP⁻¹`, and `Dⁿ` is trivial — that's the whole payoff.
- **`P⁻¹ = Pᵀ` requires orthonormal columns** → needs A symmetric → **then still normalise**.
- Diagonalization fails for non-square, too-few-eigenvectors, or complex cases. SVD never fails.

Now do the drills.


## 7. Drills

**D1.** Using `B = [[4,1],[2,3]]` and its eigenpairs from Topic 4:
(a) Construct P and D with `B = PDP⁻¹`.
(b) Explain why diagonalization makes `B¹⁰` easier. (Don't compute it.)
(c) Is it valid to replace `P⁻¹` with `Pᵀ` here? State the condition and test it.
(d) Give one reason SVD is more generally applicable than eigendecomposition.

**D2.** `A = [[3,1],[1,3]]` (symmetric). Build P and D. Is `P⁻¹ = Pᵀ` valid? What must
you do to P first?

**D3.** Prove `A² = PD²P⁻¹` in one line.

**D4.** Name the three situations where eigendecomposition fails.

---

### Answers

**D1.**
(a) `P = [[1,1],[1,−2]]`, `D = [[5,0],[0,2]]` (columns of P matched to diagonal of D).
(b) `B¹⁰ = P D¹⁰ P⁻¹`, and `D¹⁰ = diag(5¹⁰, 2¹⁰)` — raising a diagonal matrix to a power
is just raising each diagonal entry, so ten matrix multiplications collapse into two
scalar exponentiations plus two matrix products. It also shows the long-run behaviour is
dominated by the largest eigenvalue, 5.
(c) **No, not valid here.** The condition is that P must be **orthogonal** (columns
mutually orthogonal and of unit length), which is guaranteed when **A is symmetric**.
`B ≠ Bᵀ` (entry 1 vs 2), and directly: `(1,1)·(1,−2) = −1 ≠ 0`, so the eigenvectors are
not orthogonal. The genuine inverse `P⁻¹` must be used.
(d) SVD exists for **every** matrix — including non-square ones like the 768×1024 image,
and square-but-not-diagonalizable ones like a shear — whereas eigendecomposition requires
a square matrix with a full set of independent eigenvectors.

**D2.** From Topic 4 D2: λ=4 → (1,1), λ=2 → (1,−1).
`P = [[1,1],[1,−1]]`, `D = [[4,0],[0,2]]`.
`A` **is** symmetric and `(1,1)·(1,−1) = 0`, so the eigenvectors are orthogonal. But
`Pᵀ = P⁻¹` requires **orthonormal** columns, so you must first **normalise**: divide each
column by `√2`, giving `P = (1/√2)[[1,1],[1,−1]]`. Then `P⁻¹ = Pᵀ` is valid.

**D3.** `A² = (PDP⁻¹)(PDP⁻¹) = PD(P⁻¹P)DP⁻¹ = PD·I·DP⁻¹ = PD²P⁻¹` ∎

**D4.** (1) A is not square. (2) A is square but has fewer independent eigenvectors than
its dimension (e.g. a shear with a repeated eigenvalue). (3) A has complex eigenvalues and
no real eigenvectors (e.g. a rotation).
