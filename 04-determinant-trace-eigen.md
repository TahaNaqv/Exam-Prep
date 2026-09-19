# Topic 4 — Determinant, Trace, Eigenvalues & Eigenvectors

These are the most **mechanical** marks on the paper. The recipe never changes. Drill it
until your hand does it without your brain.

## 1. Trace

> **tr(A) = sum of the diagonal entries.**

`B = [[4,1],[2,3]]` → `tr(B) = 4 + 3 = 7`. That's the whole definition.

## 2. Determinant

For a 2×2 matrix `A = [[a,b],[c,d]]`:

> **det(A) = ad − bc**

`B = [[4,1],[2,3]]` → `det(B) = (4)(3) − (1)(2) = 12 − 2 = **10**`

### What it *means*

> **det(A) is the factor by which A scales area** (volume in 3D, "hypervolume" in nD).

Take the unit square (area 1), apply A, and the output parallelogram has area `|det(A)|`.
The `ad − bc` formula falls straight out of tracking that area — it isn't a rule from nowhere.

A negative determinant means the transformation also **flips orientation** (like a mirror).

### The critical case: det(A) = 0

> **det(A) = 0 ⟺ the transformation collapses a dimension ⟺ A has no inverse.**

Area becomes zero because the square gets flattened onto a line. And this is **exactly**
what "rank-deficient" meant in Topic 2 — redundant rows/columns, a destroyed dimension,
no way to undo the map. Three names for one situation:

    det(A) = 0  ⟺  A is singular  ⟺  A is not full rank  ⟺  A has a zero eigenvalue

(For 3×3 and larger: cofactor expansion along any row or column. The slides say this is
shown on the board if asked and is **not required for core assessment** — don't burn
Sunday on it.)

## 3. Eigenvectors and eigenvalues

**The motivating question:** apply a matrix to every point on a circle. Most directions
get rotated *and* stretched. **Are there directions that only get scaled — never rotated?**

Yes. Those are the eigenvectors.

> **Av = λv**
>
> - **Eigenvector `v`** — a direction A doesn't rotate off of; it only stretches, shrinks,
>   or flips it.
> - **Eigenvalue `λ`** — the scale factor along that direction.

A matrix can have several eigenvector directions, each with its own eigenvalue.

An eigenvector is only defined **up to scale** — if `v` works, so does `2v` or `−v`. So
"find *an* eigenvector" means pick any convenient one; small whole numbers are fine.

## 4. The recipe (memorise this — it is the same every single time)

**Step 1.** Build the characteristic equation: `det(A − λI) = 0`

For 2×2, this always reduces to:

> **λ² − tr(A)·λ + det(A) = 0**

That shortcut is worth memorising — it turns Step 1 into arithmetic you already did.

**Step 2.** Solve the quadratic for λ.

**Step 3.** For each λ, solve `(A − λI)v = 0` to get the eigenvector.
(The two rows will be redundant — that's expected and is a sign you did it right. Use one
row, pick a convenient value for one entry, read off the other.)

**Step 4.** Verify: check `Av = λv` by direct multiplication.

## 5. Worked example — `B = [[4,1],[2,3]]`

**Trace and determinant:** `tr = 7`, `det = 10`.

**Characteristic equation:**

    det(B − λI) = det([[4−λ, 1], [2, 3−λ]])
                = (4−λ)(3−λ) − (1)(2)
                = 12 − 4λ − 3λ + λ² − 2
                = λ² − 7λ + 10 = 0

(Matches the shortcut: `λ² − 7λ + 10` ✓)

**Solve:** `(λ − 5)(λ − 2) = 0` → **λ₁ = 5, λ₂ = 2**

**Eigenvector for λ = 5:**

    B − 5I = [[−1, 1], [2, −2]]

Row 1 gives `−v₁ + v₂ = 0`, i.e. `v₂ = v₁`. Take **v = (1, 1)**.
(Row 2 gives `2v₁ − 2v₂ = 0` — the same equation. Good.)

**Eigenvector for λ = 2:**

    B − 2I = [[2, 1], [2, 1]]

Row 1 gives `2v₁ + v₂ = 0`, i.e. `v₂ = −2v₁`. Take **v = (1, −2)**.

**Verify λ = 5:** `B(1,1) = (4+1, 2+3) = (5,5) = 5·(1,1)` ✓
**Verify λ = 2:** `B(1,−2) = (4−2, 2−6) = (2,−4) = 2·(1,−2)` ✓

## 6. The identity that checks your work for free

> **λ₁ + λ₂ = tr(A)**   and   **λ₁ · λ₂ = det(A)**
>
> (In general: sum of eigenvalues = trace, product of eigenvalues = determinant.)

Here: `5 + 2 = 7 = tr` ✓ and `5 × 2 = 10 = det` ✓

**Use this on every eigenvalue question.** It catches sign errors and arithmetic slips in
about three seconds, and quoting it is often worth a mark on its own.

The conceptual reading: eigenvalues carry the *same information* as determinant and trace,
just split apart into individual directions instead of aggregated.

### Invertibility, stated three ways

`B` is invertible because:
- `det(B) = 10 ≠ 0`, **or**
- neither eigenvalue is zero (product = det = 10 ≠ 0), **or**
- `B` is full rank (rank 2) — its columns are independent.

If asked "explain why B is invertible", give one of these *with the reason attached* —
don't just write "det ≠ 0".

## 7. At scale (backup slide — know it exists)

The characteristic-polynomial method **does not scale**. For a 10,000×10,000 matrix you
cannot factor a degree-10,000 polynomial.

**Power iteration:** repeatedly multiply a random vector by A and renormalise.

    v₀ → Av₀ → A²v₀ → … → the top eigenvector

It converges to the eigenvector with the **largest** eigenvalue. This is much closer to
what real systems (including PageRank) actually do.

## 8. Where eigenvalues show up (industry framing — cheap marks on a "why does this matter" question)

- **PageRank** — Google's original ranking algorithm is an eigenvector problem.
- **Structural / vibration analysis** — eigenvalues predict resonance and failure modes.
- **Control systems** — stability is an eigenvalue question.
- **PCA** — the top eigenvector of the covariance matrix is the first principal component
  (Topic 8).

## 9. Drills

**D1.** `B = [[4,1],[2,3]]`. Full treatment: det, trace, characteristic equation, both
eigenvalues, an eigenvector for each, verify one pair, confirm the trace/det identities,
and explain why B is invertible.

**D2.** `A = [[3,1],[1,3]]`. Find both eigenvalues and eigenvectors.
*(This is the lecture's own example from Session 1.)*

**D3.** `M = [[2,1],[4,2]]`. Find det and the eigenvalues. What do they tell you about
invertibility and rank?

**D4.** A 2×2 matrix has `tr = 7` and `det = 6`. Find its eigenvalues without seeing the
matrix.

**D5.** Explain in two sentences why `det(A) = 0` means information has been destroyed.

---

### Answers

**D1.** `det = 10`, `tr = 7`. `λ² − 7λ + 10 = 0` → **λ = 5, 2**.
λ=5 → **(1,1)**; λ=2 → **(1,−2)**. Check: `B(1,1) = (5,5) = 5(1,1)` ✓.
`5+2 = 7 = tr` ✓, `5×2 = 10 = det` ✓. Invertible because `det = 10 ≠ 0` (equivalently,
no eigenvalue is zero, equivalently it is full rank).

**D2.** `tr = 6`, `det = 9 − 1 = 8`. `λ² − 6λ + 8 = 0` → `(λ−4)(λ−2) = 0` → **λ = 4, 2**.
λ=4: `A−4I = [[−1,1],[1,−1]]` → `v₂ = v₁` → **(1,1)**.
λ=2: `A−2I = [[1,1],[1,1]]` → `v₂ = −v₁` → **(1,−1)**.
*(Note these two eigenvectors are orthogonal: `(1,1)·(1,−1) = 0`. That's not luck — A is
symmetric. Remember this for Topic 5.)*

**D3.** `det = (2)(2) − (1)(4) = **0**`. `tr = 4`. `λ² − 4λ + 0 = 0` → `λ(λ−4) = 0` →
**λ = 4, 0**. The zero eigenvalue means M is **singular (not invertible)** and
**rank-deficient** — rank 1, not 2. It flattens the plane onto a line.

**D4.** `λ² − 7λ + 6 = 0` → `(λ−6)(λ−1) = 0` → **λ = 6 and 1**.
*(This is the answer to the "Your Turn" slide in Module 3 Session 1.)*

**D5.** `det(A) = 0` means A scales area to zero, which means it flattens the input space
onto something lower-dimensional — a line or a point. Once many distinct input vectors
have been mapped onto the same output vector, there is no way to tell them apart
afterwards, so no inverse matrix can exist and the information is irrecoverably lost.

---

## 10. The lecture's own "Your Turn" problem

Module 3 Session 1 put this on screen as in-class practice and gave only the answer
(λ = 6, 1). Here it is in full — **do it before you look.**

> `A = [[5, 4], [1, 2]]`
> 1. Build the characteristic polynomial. 2. Solve for λ. 3. Find each eigenvector.

**Solution.** `tr = 5 + 2 = 7`, `det = (5)(2) − (4)(1) = 6`.
`λ² − 7λ + 6 = 0` → `(λ−6)(λ−1) = 0` → **λ = 6 and λ = 1** ✓ (matches the slide)

λ=6: `A − 6I = [[−1, 4], [1, −4]]` → `−v₁ + 4v₂ = 0` → `v₁ = 4v₂` → **v = (4, 1)**
λ=1: `A − I = [[4, 4], [1, 1]]` → `v₁ + v₂ = 0` → **v = (1, −1)**

Verify: `A(4,1) = (20+4, 4+2) = (24, 6) = 6·(4,1)` ✓
       `A(1,−1) = (5−4, 1−2) = (1, −1) = 1·(1,−1)` ✓
Checks: `6 + 1 = 7 = tr` ✓, `6 × 1 = 6 = det` ✓

Note this matrix is **not symmetric**, and its eigenvectors are **not orthogonal**:
`(4,1)·(1,−1) = 3 ≠ 0`. So for this one, `P⁻¹ ≠ Pᵀ` (Topic 5).
