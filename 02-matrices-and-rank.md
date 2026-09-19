# Topic 2 — Matrices & Rank

## 1. A matrix is a function

Forget "grid of numbers". The lecture's actual definition:

> **A matrix is a function that takes a vector in and gives a vector out.**

`Av` is `A` *applied to* `v`. Same idea as `f: ℝⁿ → ℝᵐ` from Day 1. `A` is what that
function looks like written down.

### The row picture (the mechanical way)

`A = [[2, 1], [3, 4]]`, `v = (5, 2)`

Row 1: (2)(5) + (1)(2) = 12
Row 2: (3)(5) + (4)(2) = 23
→ `Av = (12, 23)`

Each output entry = that row dotted with `v`.

### The column picture (the way that actually explains things)

`A`'s columns are `(2,3)` and `(1,4)`. `v = (5,2)` says: **"take 5 of the first column,
2 of the second."**

    5·(2,3) + 2·(1,4) = (10,15) + (2,8) = (12,23)   ← same answer

> **Av is a linear combination of A's columns, weighted by v's entries.**

In ML notation, with a data matrix `X` and weights `w`:

    Xw = w₁x₁ + w₂x₂ + ⋯ + wₚxₚ

This is Topic 1's "linear combination" doing real work for the first time. **Learn the
column picture.** Everything from here — column space, rank, span, SVD — is built on it.

**Column space** = the set of every output `Av` could possibly produce, over all possible `v`
= everything A's columns can build by linear combination. (Also called the *span* of the columns.)

## 2. What "linear" actually means

A matrix isn't just any function — it's a **linear transformation**:

- **Additivity:** `A(u + w) = Au + Aw`
- **Homogeneity:** `A(cu) = c·Au`

Together: `A(c₁u + c₂w) = c₁Au + c₂Aw`. **A combination of inputs maps to the same
combination of outputs.** That is the definition of linear.

**Not every simple function is linear.** The lecture's counterexample, worth remembering
because "simple" and "linear" are not synonyms:

`f(v) = v + (1,1)` — just a shift.
- `f(u) + f(w) = u + w + (2,2)`
- `f(u + w) = u + w + (1,1)`

Not equal → **a shift is not linear.**

**Why this matters in ML (good exam one-liner):** every neural-network layer is a linear
map (matrix multiply) followed by a **nonlinear** activation. If you stacked linear layers
alone they'd collapse into a single matrix and depth would buy you nothing. The
nonlinearity is the entire reason deep networks work.

## 3. What a matrix does geometrically

Because the map is linear, a straight edge must map to a straight edge (an edge is a
combination of its two endpoints). So to see what a matrix does to a square, **you only
have to check the 4 corners** and join them up.

- A rotation matrix `R = [[cos θ, −sin θ], [sin θ, cos θ]]` turns the square. Same shape,
  same size, just rotated.
- **Composition:** "rotate, then scale" is a single matrix `SR`. Not a new rule — just
  multiplication. **Order matters: `SR ≠ RS` in general.**

## 4. Mechanics you must not fumble

**Matrix multiplication** — inner dimensions must match:
`(m×n)(n×p) → (m×p)`

    [1 2 0]   [2 0]     [4  6]
    [3 1 4] × [1 3]  =  [7  7]
              [0 1]
    (2×3)   ×  (3×2)  →  (2×2)

**Transpose `Aᵀ`** — flip across the diagonal; rows become columns:

    [1 2 3]ᵀ    [1 4]
    [4 5 6]  =  [2 5]
                [3 6]

An `m×n` matrix transposes to `n×m`. `Aᵀ` is not bookkeeping — `AᵀA` and `Aᵀb` are
everywhere in projection, regression and gradients.

## 5. Linear independence

The question: **does this vector add a genuinely new direction, or could I have built it
from the ones I already have?**

- `v₁ = (1,2)`, `v₂ = (2,4)`: is `v₂ = c·v₁`? Yes, `c = 2`. → **dependent.**
  `v₂` adds nothing.
- `v₁ = (1,2)`, `v₃ = (3,1)`: we'd need `c = 3` (first entry) *and* `c = 0.5` (second
  entry) simultaneously. Impossible. → **independent.** A genuinely new direction.

> **A set is linearly dependent exactly when one vector can be written as a linear
> combination of the others.**

**How to test on paper (2 vectors):** try to scale one into the other. If a single
constant works for *every* coordinate, dependent. If not, independent.

## 6. Rank

> **rank(A) = the dimension of A's column space
> = how many of its columns are genuinely independent
> = how much non-redundant information the matrix carries.**

Example: `M = [[2,1],[4,2]]`. Column 2 = ½ × column 1. Two columns, but a basis for what
they span needs only **one** vector. So `rank(M) = 1`, even though `M` is 2×2.

### ⚠ THE TRAP — the instructor called this out explicitly

18% of the class got this wrong on the diagnostic, and it's item #1 on his self-check list:

> **Rank has nothing to do with shape.**

- A 2×3 matrix can be **full rank** (rank 2).
- A square 2×2 matrix can be **rank-deficient** (rank 1).
- **Independence is the test. Shape never was.**

### Terminology (get these right, they're worth marks)

- **Max possible rank** of an m×n matrix is `min(m, n)`.
- **Rank-deficient** = `rank(A) < min(m, n)`.
- **Full column rank** = rank equals the *number of columns*. (Needs rank = n, so
  requires m ≥ n — a wide matrix can never have full column rank.)
- **Full row rank** = rank equals the number of rows.
- **Singular matrix** = a square matrix with no inverse (i.e. not full rank).
- **Multicollinearity** = the real-data name for rank deficiency in your feature columns.
  E.g. you add a "total score" column that is the sum of existing columns — one more
  column, zero more rank, zero more information.

These are different! A matrix can be *not* rank-deficient by the `min(m,n)` definition and
still *lack full column rank*. Exam question 2 turns on exactly this distinction.

## 7. Rank-deficiency destroys information

Apply `M = [[2,1],[4,2]]` (rank 1) to every point of a square. Common guess: it shrinks.
What actually happens: **the entire 2D square is flattened onto a single line.**

It isn't shrunk — a whole dimension is **destroyed**. And it's **not reversible**: once
many different input points have been mapped onto the same output point, no matrix can
undo it. That is exactly why:

- `Ax = b` sometimes has **no** clean solution,
- least squares exists at all,
- and `det = 0` ⟺ no inverse (Topic 4).

## 8. The key result that closes the loop

> **`AᵀA` is invertible if and only if `A` has full column rank.**

Fitting a linear model requires inverting `AᵀA` (you'll see why in Topic 3). So:
redundant/collinear features → not full column rank → `AᵀA` singular → **no unique fit.**

You are expected to be able to argue "is `AᵀA` invertible?" **without computing a
determinant** — just check whether the columns of A are independent.

Useful companion fact: `rank(AᵀA) = rank(A)`.

## 9. Drills

**D1.** `A = [[1,0,1],[0,1,1]]`, `z = (2,−1,3)`.
(a) Compute `Az` row-by-column.
(b) Recompute it as a linear combination of A's columns.
(c) What is rank(A)? Give one dependence relation among the columns.
(d) Is A rank-deficient in the `rank < min(m,n)` sense? Does it have full *column* rank?
(e) Without a determinant: is `AᵀA` invertible?

**D2.** Is `f(v) = 3v` linear? Is `f(v) = v + (2,0)`? Prove each in one line.

**D3.** `B = [[1,2],[2,4]]`. Find rank(B). What does B do to a square, geometrically?

**D4.** True or false, with a reason: "A 3×5 matrix must be rank-deficient."

**D5.** A dataset has columns `height_cm`, `height_m`, `weight`. What is the rank of that
3-column matrix at most, and what's the real-data name for the problem?

---

### Answers

**D1.**
(a) Row 1: (1)(2)+(0)(−1)+(1)(3) = 5. Row 2: (0)(2)+(1)(−1)+(1)(3) = 2. → `Az = (5, 2)`.
(b) `2·(1,0) + (−1)·(0,1) + 3·(1,1) = (2,0) + (0,−1) + (3,3) = (5,2)` ✓ same.
(c) Columns are `(1,0)`, `(0,1)`, `(1,1)`. The first two are independent, and
`col₃ = col₁ + col₂`. So **rank(A) = 2**.
(d) A is 2×3, so `min(m,n) = 2`, and rank = 2. **Not rank-deficient** — it is full rank.
But it has **3 columns and rank 2**, so it does **not** have full column rank.
*(This is the trap: full rank ≠ full column rank.)*
(e) `AᵀA` is 3×3 but `rank(AᵀA) = rank(A) = 2 < 3`. A needs full column rank for `AᵀA`
to be invertible, and it doesn't have it. **`AᵀA` is singular — not invertible.**

**D2.** `f(v) = 3v` is linear: `f(u+w) = 3(u+w) = 3u + 3w = f(u)+f(w)` ✓ and
`f(cu) = 3cu = c·f(u)` ✓.
`f(v) = v + (2,0)` is **not**: `f(u+w) = u+w+(2,0)` but `f(u)+f(w) = u+w+(4,0)`. Not equal.

**D3.** `col₂ = 2·col₁`, so **rank = 1**. It flattens the whole square onto the single
line spanned by `(1,2)` — a dimension is destroyed and the map is not invertible.

**D4.** **False.** Max rank is `min(3,5) = 3`, and a 3×5 matrix can perfectly well have
rank 3 (full rank). Shape is not the test; independence is. *(It can never have full
**column** rank, though — 5 columns can't be independent in ℝ³.)*

**D5.** `height_m = height_cm / 100`, so those two columns are dependent. Rank is **at
most 2**. The name is **multicollinearity** (rank deficiency in the feature matrix), and
it makes `XᵀX` singular, so the regression has no unique solution.
