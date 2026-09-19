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
