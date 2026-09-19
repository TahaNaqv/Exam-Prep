# Method Recipes — the mechanical skills, done slowly

The topic files assume you can already perform these sub-steps. If any of them is where you
actually get stuck, this is the file. Each one is a fixed procedure — learn the procedure
once and it works every time.

---

## R1 — Solving `(A − λI)v = 0` for an eigenvector

This is the step people stall on, because it looks like a normal equation-solving problem
and behaves differently.

### What you're being asked

Find a vector `v` that the matrix `(A − λI)` sends to **zero**. There are infinitely many
answers (if `v` works, so does `2v`, `−v`, `100v`), so you're finding a **direction**, and
picking any convenient representative of it.

### The procedure

**Step 1. Build `A − λI`.** Subtract λ from each diagonal entry only. Nothing else changes.

    A = [ 4  1 ]    λ = 5    →    A − 5I = [ 4−5   1  ] = [ −1   1 ]
        [ 2  3 ]                            [  2   3−5 ]   [  2  −2 ]

**Step 2. Write out the two equations.** Read them straight off the rows:

    Row 1:  −1·v₁ + 1·v₂ = 0
    Row 2:   2·v₁ − 2·v₂ = 0

**Step 3. Notice the rows are redundant.** Row 2 is `−2 ×` Row 1. **This always happens**,
and it is your confirmation that λ was computed correctly.

> If the two rows are **not** redundant, you have the wrong λ — go back and recheck the
> characteristic equation. (A non-redundant pair would force `v = (0,0)`, which is never
> accepted as an eigenvector.)

**Step 4. Use one row. Solve for one variable in terms of the other.**

    −v₁ + v₂ = 0   →   v₂ = v₁

**Step 5. Pick the simplest value.** Set the free variable to 1 (or whatever clears
fractions):

    v₁ = 1  →  v₂ = 1  →  **v = (1, 1)**

**Step 6. Verify with `Av = λv`.** `A(1,1) = (4+1, 2+3) = (5,5) = 5(1,1)` ✓

### Worked again, with a fraction

`A = [[4,1],[2,3]]`, λ = 2:

    A − 2I = [ 2  1 ]      Row 1:  2v₁ + v₂ = 0  →  v₂ = −2v₁
             [ 2  1 ]      Row 2:  2v₁ + v₂ = 0  (identical ✓)

Set `v₁ = 1` → `v₂ = −2` → **`v = (1, −2)`**

> **Tip:** if solving gives `v₁ = ½v₂`, don't write `(0.5, 1)` — set `v₂ = 2` instead and
> write `(1, 2)`. Whole numbers are less error-prone and equally correct.

---

## R2 — Inverting a 2×2 matrix (and verifying `PDP⁻¹`)

### The formula

    [ a  b ]⁻¹      1     [  d  −b ]
    [ c  d ]    = ──────  [ −c   a ]
                  ad − bc

In words: **swap the diagonal entries, negate the off-diagonal entries, divide everything by
the determinant.**

If `ad − bc = 0` there is no inverse — the matrix is singular.

### Worked: verifying the Q6 diagonalization

`P = [[1, 1], [1, −2]]`

**Step 1. Determinant.** `det(P) = (1)(−2) − (1)(1) = −2 − 1 = −3`

**Step 2. Apply the formula.** Swap diagonal (1 and −2), negate off-diagonal (both 1s):

    P⁻¹ = (1/−3) [ −2  −1 ]  =  [  2/3   1/3 ]
                 [ −1   1 ]     [  1/3  −1/3 ]

**Step 3. Sanity check `PP⁻¹ = I`.**

    Row 1 of P · Col 1 of P⁻¹ = (1)(2/3) + (1)(1/3) = 1  ✓
    Row 1 of P · Col 2 of P⁻¹ = (1)(1/3) + (1)(−1/3) = 0 ✓

**Step 4. Confirm `PDP⁻¹ = B`** with `D = diag(5,2)`:

    PD = [ 1   1 ][ 5  0 ] = [ 5   2 ]
         [ 1  −2 ][ 0  2 ]   [ 5  −4 ]

    (PD)P⁻¹ = [ 5   2 ][  2/3   1/3 ] = [ 10/3+2/3   5/3−2/3 ] = [ 4  1 ]  ✓ = B
              [ 5  −4 ][  1/3  −1/3 ]   [ 10/3−4/3   5/3+4/3 ]   [ 2  3 ]

> Multiplying by a **diagonal** matrix on the right just scales each **column** — column 1
> by 5, column 2 by 2. That shortcut saves time and mistakes.

---

## R3 — Least squares with actual numbers (the normal equations)

Topic 3 derives `x̂ = (AᵀA)⁻¹Aᵀb` but never plugs numbers in. Here it is end to end.

### Setup: fit a straight line through three points

Points `(1,1), (2,2), (3,2)`. Model: `y = c₀ + c₁x`. Written as `Ax̂ ≈ b`:

    A = [ 1  1 ]        b = [ 1 ]        x̂ = [ c₀ ]
        [ 1  2 ]            [ 2 ]             [ c₁ ]
        [ 1  3 ]            [ 2 ]

The first column of 1s is the intercept; the second holds the x-values. Three equations, two
unknowns — **overdetermined**, so no exact solution exists. That's why we project.

**Step 1. Compute `AᵀA`.**

    AᵀA = [ 1  1  1 ][ 1  1 ] = [ 1+1+1      1+2+3   ] = [ 3   6 ]
          [ 1  2  3 ][ 1  2 ]   [ 1+2+3   1+4+9     ]   [ 6  14 ]
                     [ 1  3 ]

**Step 2. Compute `Aᵀb`.**

    Aᵀb = [ 1  1  1 ][ 1 ] = [ 1+2+2       ] = [  5 ]
          [ 1  2  3 ][ 2 ]   [ 1+4+6       ]   [ 11 ]
                     [ 2 ]

**Step 3. Check invertibility before inverting.** A's columns are `(1,1,1)` and `(1,2,3)` —
neither is a multiple of the other, so they're independent, so A has **full column rank**,
so `AᵀA` **is** invertible. *(Also `det(AᵀA) = 42 − 36 = 6 ≠ 0` ✓)*

**Step 4. Solve `AᵀAx̂ = Aᵀb`.** Don't invert — just solve the 2×2 system:

    3c₀ +  6c₁ =  5
    6c₀ + 14c₁ = 11

Multiply the first by 2: `6c₀ + 12c₁ = 10`. Subtract from the second: `2c₁ = 1` → **`c₁ = ½`**
Substitute back: `3c₀ + 3 = 5` → `3c₀ = 2` → **`c₀ = ⅔`**

> **Best-fit line: `y = ⅔ + ½x`**

**Step 5. Verify the residual is orthogonal to both columns.**

Predictions: `A x̂ = (⅔+½, ⅔+1, ⅔+1½) = (7/6, 5/3, 13/6) ≈ (1.167, 1.667, 2.167)`
Residual: `r = b − Ax̂ = (−1/6, 1/3, −1/6)`

    r · col₁ = −1/6 + 1/3 − 1/6 = 0  ✓
    r · col₂ = (−1/6)(1) + (1/3)(2) + (−1/6)(3) = −1/6 + 2/3 − 1/2 = 0  ✓

> Both zero — equivalently `Aᵀr = 0`, which **is** the normal equations. The fitted line is
> the projection of `b` onto A's column space, and the residual is what's left over,
> perpendicular to everything the model could reach.

**Solving beats inverting.** Never compute `(AᵀA)⁻¹` explicitly if you can solve the system
instead — fewer steps, fewer arithmetic errors.

---

## R4 — Testing linear independence

### Two vectors

Ask: **is one a scalar multiple of the other?** Try to find a single `c` that works for
*every* coordinate.

- `(1,2)` and `(2,4)`: `2/1 = 2` and `4/2 = 2` — same ratio → `c = 2` works → **dependent**
- `(1,2)` and `(3,1)`: `3/1 = 3` but `1/2 = 0.5` — different ratios → no single `c` →
  **independent**

**Shortcut for 2 vectors in ℝ²:** stack them as a matrix and take the determinant.
`det = 0` → dependent. `det ≠ 0` → independent.
`[[1,2],[2,4]]` → `4 − 4 = 0` → dependent ✓

### Three or more vectors

Look for an obvious combination first — on exams there almost always is one:

`(1,0)`, `(0,1)`, `(1,1)` → spot that **`c₃ = c₁ + c₂`** → dependent.

Common patterns to check for: one column is the **sum** of two others; one is a **multiple**
of another; one is a **difference**. (In real data: a "total" column, or the same quantity in
two units — that's **multicollinearity**.)

> **Rank = how many are left after removing the redundant ones.** Above: 3 vectors, 1
> redundant → **rank 2**.

---

## R5 — Getting the angle from a dot product

**Step 1.** `cos θ = (u·w) / (‖u‖‖w‖)`
**Step 2.** `θ = arccos(...)` — use the calculator, but **sanity-check the sign first:**

| `cos θ` | angle | meaning |
|---|---|---|
| = 1 | 0° | same direction |
| > 0 | 0°–90° | acute — broadly agree |
| = 0 | exactly 90° | **orthogonal** |
| < 0 | 90°–180° | obtuse — broadly disagree |
| = −1 | 180° | exactly opposite |

> If your calculator gives an angle whose sign disagrees with the table, you made an
> arithmetic error. `cos θ = −0.141` **must** land between 90° and 180° — and it does, at 98.1°.

---

## R6 — Laying out an exam answer for maximum marks

The assignment says *"Unsupported final answers alone will not receive full credit."* Expect
the same on the paper. Use this four-line shape for every computational part:

```
1. State what you're computing and the formula.
   "Energy = sum of squared singular values, Σσᵢ²"

2. Substitute the actual numbers — visibly.
   "= 4² + 3² + 1² = 16 + 9 + 1"

3. Give the answer, clearly marked.
   "= 26"

4. One sentence of interpretation.
   "So the rank-2 approximation retains 25/26 ≈ 96% of the matrix's total signal."
```

**Line 4 is the one people skip and the one he explicitly marks** ("Have I interpreted the
calculations rather than only reporting them?").

### If you get stuck on arithmetic

**Write the method anyway.** State the formula, state what you'd substitute, say what the
answer would tell you. Most of the marks live in the method, and a blank earns zero while a
correct method with a slipped number earns most of the credit.

### Free checks worth 10 seconds each

| After | Check |
|---|---|
| Any norm | `‖v‖₁ ≥ ‖v‖₂ ≥ ‖v‖∞` |
| Eigenvalues | `λ₁+λ₂ = tr` and `λ₁λ₂ = det` |
| Eigenvector | `Av = λv` by direct multiplication |
| `(A−λI)` rows | they must be redundant |
| Projection | `r · a = 0` |
| Centering | every centered column sums to 0 |
| SVD shapes | the factor sizes multiply back to the original |
| Truncation | retained fraction + error²/total = 1 |
| Matrix inverse | `PP⁻¹ = I` |
