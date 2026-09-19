# ONE-PAGE CHEAT SHEET — read this Monday morning, nothing else

## Formulas

**Norms** (v = (v₁,…,vₙ))
`‖v‖₁ = Σ|vᵢ|`  ·  `‖v‖₂ = √(Σvᵢ²)`  ·  `‖v‖∞ = max|vᵢ|`  ·  `dist(u,w) = ‖u−w‖`
Sanity: `‖v‖₁ ≥ ‖v‖₂ ≥ ‖v‖∞`

**Matrix–vector**
Row picture: output entry i = row i · v.
Column picture: `Av = v₁·col₁ + v₂·col₂ + ⋯`  ← learn this one
Linear ⟺ `A(c₁u + c₂w) = c₁Au + c₂Aw`
`(m×n)(n×p) → (m×p)`

**Dot product**
`u·w = Σuᵢwᵢ = ‖u‖‖w‖cos θ`
`u·w = 0 ⟺ orthogonal`
`cos θ = (u·w)/(‖u‖‖w‖)`, range −1 → 1

**Projection onto a line**
`c = (a·b)/(a·a)`  ·  `projₐ(b) = c·a`  ·  `r = b − projₐ(b)`  ·  **CHECK `r·a = 0`**

**Projection onto a subspace**
`AᵀAx̂ = Aᵀb`  →  `x̂ = (AᵀA)⁻¹Aᵀb`   (needs A full column rank)

**Determinant / trace / eigen (2×2)**
`det = ad − bc`  ·  `tr = a + d`
**`λ² − tr·λ + det = 0`**  ← the shortcut
`λ₁+λ₂ = tr`  ·  `λ₁λ₂ = det`  ← free check, use it every time
Eigenvector: solve `(A − λI)v = 0`

**Diagonalization**
`A = PDP⁻¹` (P = eigenvectors as columns, D = eigenvalues on diagonal, orders must match)
`Aⁿ = PDⁿP⁻¹`, and `Dⁿ` = each diagonal entry to the n-th power
`P⁻¹ = Pᵀ` **only if A is symmetric and P's columns are normalised**

**SVD** — `A = UΣVᵀ`, A is m×n
`U` m×m = output basis · `Σ` m×n, `σ₁ ≥ σ₂ ≥ … ≥ 0` · `V` n×n = input basis
Truncated at k: `Uₖ` m×k, `Σₖ` k×k, `Vₖᵀ` k×n → multiplies to m×n
`Avᵢ = σᵢuᵢ` · rank = number of non-zero σ · V = eigvecs of `AᵀA`, U = eigvecs of `AAᵀ`, `σ = √λ`

**Energy**
Total `= Σσᵢ²` ← **SQUARED**
Retained at k `= (σ₁²+⋯+σₖ²)/Σσᵢ²`  ← ratio, no root, KEPT values
Frobenius error `= √(σ²ₖ₊₁ + ⋯)`   ← **has a root**, DROPPED values
Storage at rank k: `k(m + n + 1)` values

**PCA**
Center (`Xc`) → SVD → V's columns = PC directions, `σᵢ²` = variance captured
Shortcut: `XcᵀXc = VΣ²Vᵀ` — eigendecompose it. Eigenvalues **are** `σ²`.
Variance explained = `λᵢ / Σλⱼ`
**Scores (what you plot)** = `Xc · V` = `UΣ`; per point = (centered point) · (unit PC dir)
`Σcov = (1/n)XcᵀXc` or `(1/(n−1))XcᵀXc` → **same eigenvectors**, eigenvalues ÷ that constant,
**same proportions**. (Lecture uses n−1 and writes `C = QΛQᵀ`, `Λ = Σ²/(n−1)`; assignment Q9 uses 1/n.)

**SVD by hand (small A)**
`AᵀA` → eigenvectors = **V**, eigenvalues `λ` → `σ = √λ` → `uᵢ = Avᵢ/σᵢ` → normalise all.

## The 8 traps (his own self-check list)

1. **Rank ≠ shape.** A 2×3 can be full rank. And *full rank ≠ full column rank.*
2. **Negative dot product = angle > 90°**, NOT 180°. 90° is a boundary, not a landmark.
3. **Always verify `r·a = 0`** after a projection.
4. **`P⁻¹ ≠ Pᵀ`** unless you've established orthonormality (needs A symmetric).
5. **Know U, Σ, Vᵀ roles and dimensions.** U = output/documents, V = input/terms.
6. **Energy = sum of SQUARED singular values.**
7. **Mean-center before covariance / PCA.**
8. **Interpret, don't just report.** One sentence of meaning after every number.

## Chains to quote (these earn the "explain" marks)

`det = 0` ⟺ singular ⟺ not full rank ⟺ has a zero eigenvalue ⟺ collapses a dimension ⟺ no inverse

`AᵀA` invertible ⟺ A has full column rank ⟺ unique least-squares solution exists

Eigen fails (non-square / not diagonalizable / complex) → **SVD always works**, because
`AᵀA` and `AAᵀ` are symmetric for any A and the spectral theorem covers symmetric matrices.

PCA = SVD on centered data. "Explained variance" = "energy retained". Same quantity.

**Eckart–Young:** truncated SVD `Aₖ` is the best rank-k approximation among **ALL** rank-k
matrices, in **both** Frobenius and spectral norms, with error set exactly by the dropped σ.

## Exam technique

- **Show steps.** "Unsupported final answers alone will not receive full credit."
- Free checks that cost seconds: `λ₁+λ₂ = tr`, `λ₁λ₂ = det`, `r·a = 0`, centered columns
  sum to 0, SVD shapes multiply back to the original.
- After every computed number, **write one sentence saying what it means.**
- State any assumption you make.
- If stuck on arithmetic, **write the method anyway** — the method carries most of the marks.
